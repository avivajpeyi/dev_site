---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Slurm Notes"
subtitle: ""
summary: "Some notes on using slurm for HPC."
authors: []
tags: []
categories: []
date: 2021-10-20T00:20:45+10:00
lastmod: 2021-10-20T00:20:45+10:00
featured: false
draft: false
type: book

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---
 
{{< toc >}}



## Interactive Jobs

If you want to test softwre that requires  GUI, MPI/Parallel/Multiple threads using interactive jobs may be useful.
Note that if you need a GUI -- you'll need to `ssh` with `-X`.


Run the following to start an interactive job:
```bash
sinteractive --ntasks 1 --nodes 1 --time 00:30:00 --mem 2GB
```

Once resources are allocated, you'll notice that you're on a different machine (allocated for your interactive session).


## Jupyter notebooks + Slurm

Once are in an interactive job session you can open a `jupyter` notebook with the following steps:

1. Source envs for you interactive session 
    For example you may run the following:
    ```bash 
    source ~/.bash_profile
    module load git/2.18.0 gcc/9.2.0 openmpi/4.0.2 python/3.8.5
    source venv/bin/activate 
    ```
   
2. Setup tunnel + jupyter instance on cluster
   
   To do this run the following:
    ```bash
    ipnport=$(shuf -i8000-9999 -n1)
    ipnip=$(hostname -i)
    echo "Run on local >>> ssh -N -L $ipnport:$ipnip:$ipnport avajpeyi@ozstar.swin.edu.au"
    jupyter-notebook --no-browser --port=$ipnport --ip=$ipnip
    ```

3. Local connection to interactive job
    - run the command echoed above
    - open the link to the jupyer notebook (printed in the previous window)

4. Run `exit` when done!

    Otherwise the job will keep running until it times out.
 
 
 For convenience I have added the following to my `OzStar .bash_profile`
 ```bash
 # Interactive Jupter notebooks
 alias start_ijob="sinteractive --ntasks 2 --time 00:60:00 --mem 4GB"
 start_jupyter () {
     ipnport=$(shuf -i8000-9999 -n1)
     ipnip=$(hostname -i)
     echo "Run on local >>>"
     echo "ssh -N -L $ipnport:$ipnip:$ipnport avajpeyi@ozstar.swin.edu.au"
     jupcmd=$(jupyter-notebook --no-browser --port=$ipnport --ip=$ipnip)
 }
 export -f start_jupyter
```                     

Now I can start a interactive job by running `start_ijob` and start the jupter notebook with `start_jupyter`.
 
 
## Plot CPU hours used for jobs
The folowing creates a file `jobstats.txt` that contains the CPU time (seconds) for each job run bw the start+end time specified.
```bash
sacct -S 2021-01-01 -E 2021-10-06 -u avajpeyi -X -o "jobname%-40,cputimeraw" --parsable2 > jobstats.txt 
```
 
To plot the data you can use the following: 
{{< gist avivajpeyi 3beed78d92cd5f3520b4a1a93eb97cea >}}
  
{{< figure src="cpuhrs.png" title="Total CPU hours I've used ('19-'22) " lightbox="true" >}}



