---
title: SBI for SGBW
summary: An journal club presentation for SBI for SGWB (https://arxiv.org/pdf/2309.07954.pdf)
authors: []
tags: ['Journal Club']
categories: []
date: '20233-10-27'
slides:
  theme: white
  highlight_style: github-light
---

## SBI for SGWB

_Simulation based infernce for Stochastic GW background Analysis_
(Alvey+, 2023)


[arxiv](https://arxiv.org/pdf/2309.07954.pdf) | [swyft](https://github.com/undark-lab/swyft) | [edit](https://github.com/avivajpeyi/dev_site/edit/main/content/slides/journal_club/sbi_for_sgwb/index.md)

NZ Gravity Journal Club

Oct 26th, 2023

---

## Summary

1. LISA "Global fit" + GW background
2. Alvey+'s LISA SGWB model
3. Sim based inference + TMNRE
4. Results, Discussion + future work

---

{{% section %}}

## LISA Data analysis

---

### The data

![lisa_data]

[lisa_data]:https://github.com/avivajpeyi/dev_site/assets/15642823/af1e82e7-f1bc-4306-856e-b11e245cadf3

---

### The "Global fit"

Analyze all the data, simultaneously, block-by-block 

{{< figure src="https://github.com/avivajpeyi/dev_site/assets/15642823/1577656f-3c97-43e9-bc4d-7da09c6686ce" width="1300" height="350">}}

$<10^5$ parameters in the full problem

---

### SGWB estimation methods

Some recent work in LISA SGWB estimation: 

| Noise model    | Signal model   | Noise + Signal |
|----------------|----------------|----------------|
| [Karnesis+ '19] | [Baghi+ '23]   | [Boileau+ '20] |
| [Caprini+ '19] | [Muratore+ '23] | [Olaf+ '23]    |
| [Pieroni+ '20] |                | Aimen+ (WIP)   |

A high precision reconstruction required to extract an SGWB signal

[Karnesis+ '19]:https://arxiv.org/abs/1906.09027
[Caprini+ '19]:https://arxiv.org/abs/1906.09244
[Pieroni+ '20]:https://arxiv.org/abs/2004.01135
[Baghi+ '23]:https://arxiv.org/abs/2302.12573
[Muratore+ '23]:https://arxiv.org/abs/2308.01056
[Boileau+ '20]:https://arxiv.org/abs/2011.05055
[Olaf+ '23]:https://arxiv.org/abs/2303.15929

{{% /section %}}

---

{{% section %}}

## Alvey+'s SBI approach

---

### Motivation for SBI 


{{< speaker_note >}}
- Current SGWB approaches use stochastic sampling methods (MCMC, Nested sampling)  
- These are not _robust_ to foreground transient signals (e.g. massive BH mergers)s
{{< /speaker_note >}}


SBI for SGWB estimation motivations 
1. the 'marginal inference' property
2. likelihood 'free' inference
3. more robust to foreground transient signals (e.g. massive BH mergers)

---

### Signal and noise model

[Signal]:https://user-images.githubusercontent.com/15642823/277888868-c7ac02f7-a2f1-4e49-ada3-f72c0e2eb71f.png
[Noise]: https://user-images.githubusercontent.com/15642823/277888887-50a16f75-854a-4098-b3d4-ca7a829a6324.png


|           | 
|:---------:|
| ![Signal] | 
| ![Noise]  |

---

### BASE Model consists of
- Data:
  - 2 paramter instrumental noise model (only amplitude -- shape of noise curve is fixed)
  - SGWB model, either (a) or (b)
  - data(t) = noise(t) + Sum_signals s_i(t)
- Single TDI channel
- 12 days of data (split into 100 segments, 1 segment ~ 2.9 hours)
- Freq resolution 1/2.9Hours ~ 0.1 mHz

Note: this is ~1% of the full LISA mission duration

---

### Model with transients:
- Same as BASE mode
- In each segement Inject 1 massive BH merger (priors below) if U[0,1] < p 

```python
    Mc = U(8e5, 9e5)
    eta = U(0.16, 0.25)
    chi1 = U(-1.0, 1.0)
    chi2 = U(-1.0, 1.0)
    dist_mpc = U(5e4, 1e5)
    tc = 0.0
    phic = 0.0

```

---

### MLA training:
"Several numerical settings should be chosen for the general structure of the algorithm as well as the network architechture"
- 500K simulations (9:1 train:val split)
- 50 epochs (512 batch size)
- save model weights with the lowest validation loss


---

### END OF SECTION

{{% /section %}}

---

{{% section %}}

## SBI Intro

---
### Traditional problem

$$
p(\theta|d) = \frac{\mathcal{L}(d|\theta)\pi(\theta)}{\color{red}{Z(d)}}= \frac{\mathcal{L}(d|\theta)\pi(\theta)}{\color{red}{\int_{\theta}\mathcal{L}(d|\theta)\pi(\theta) d\theta}}
$$

- _Monte Carlo_: e.g. Rejection sampling
- _Markov-chain MC_: e.g. Metropolis-Hastings, NUTS
- _Variational Inference_: surrogate $p(\theta|d)$

 
{{< hl >}} What if we dont have $\mathcal{L}(d|\theta)$ ?{{< /hl >}}



---
### Simulation based inference:
New term for:
- Approximate Bayes Computation, 
- Likelihood free inference,
- Indirect inference,
- Synthetic likelihood


---

### Algorithm

{{< figure src="https://miro.medium.com/v2/1*oer83KfCCI1AnoqsRtYlRg.png" width="400" height="400" >}}

Compare the 'simulated' data to the 'true' data

{{< speaker_note >}}

- Marginal inference -- SBI its possible to directly target specific parameters for inference, ignore other parameters while still dealing correctly with the ones we dont care about  
- Amortized -- SBI once trained -- we can get answers of the posteriors very quickly

{{< /speaker_note >}}

--- 

### Different SBI methods:

- **Classical**: Rejection ABC ('97), MCMC-ABC ('03)
- **Neural density**: 
  - Neural posterior estimator
  - Neural likelihood estimator
  - Neural _ratio_ estimator (Lnl/evid)
- **Types of NN:**
  - Mixture density networks
  - Normalising flows

---

### Goals for NN + SBI:

- *Speed*: Training faster than MCMC
- *Scalability*: Doesn't fall apart with high D
- *Pre-existing research*: Leverage modern ML tools (flows, NNs ...)


---


### MCMC, VI, SBI 

|                           | MCMC | VI  | SBI     |  
|---------------------------|-----|-----|---------|
| Explicit Likelihood       | ✅   | ✅   | ❌       |  
| Requires gradients        | ✅   | (✅) | ❌       |  
| Targeted inference        | ✅    | ✅   | ❌       |  
| Amortized                 | ❌  | (✅) | ✅       |  
| Specialised architechture | ❌  | ✅   | ✅       |  
| Requires data summaries   | ❌  | ❌   | ✅       |  
| Marginal inference        | ❌  | ❌   | ✅       |  
        
{{< speaker_note >}}
Amortized posterior is one that is not focused on any particular observation
{{< /speaker_note >}}   

---

### END OF SECTION

{{% /section %}}

---

{{% section %}} 

## SBI Math 

__Skipping this, can come back if folks interested__



{{< speaker_note >}}
Library: swyft
Simulation efficient marginal posterior estimation

Target: X
- say there are lots of parameters $\theta$
- Only parameter values that plausiablly generate X will contribut to marginaliation
- NESTED RATIO ESTIMATION finds this region by iteratively cnstraining the initial prior based on 1D marginal posteriors from previous iterations 
- this method approximates the likelihood-to-evidence ratio by zeroing in on the high-likelihood regions 
- method inspired by nested sampling
- After a few iteraintins -- some 1D marginals will be mre constrained than others 

https://pbs.twimg.com/media/E65qN0dWEAAxXCW?format=png&name=900x900

{{< /speaker_note >}}


---

### $D_{KL}$ "Loss" function for training

$$D_{\rm KL}(\tilde{p}, p) = \int \tilde{p}(x) \log \frac{\tilde{p}(x)}{p(x)}\ dx$$

$D_{KL}$ is _not_ symmetric
- $D_{\rm KL}(\tilde{p}, p)$: Variational inference (LnL based)
- $D_{\rm KL}(p, \tilde{p})$: NPE (Simulation based)

**PROBLEM:** how do we avoid evaluating the $p(\theta|d)$?

---

### KL-Divergence and VI

$$D_{\rm KL} [\tilde{p}, p] (\theta) \sim \mathbb{E}_{\theta\sim\tilde{p}(\theta|d)} \log \left[ \frac{\tilde{p}(\theta|d)}{\mathcal{L}(d|\theta)\pi(\theta)} \right] + C$$ 

- **PROBLEM:** $p(\theta|d)$ is $$$
- **SOLUTION:** 
  - $p(\theta|d) \sim \mathcal{L}(d|\theta)\pi(\theta)$ 
  - $0\leq D_{\rm KL} [\tilde{p}, p]\leq Z(d)$
  - Train $\tilde{p}(\theta|d)$

---

### KL-Divergence and SBI

$$D_{\rm KL}[p, \tilde{p}] (\theta, d) \sim -\mathbb{E}_{(\theta,d)\sim p(\theta,d)} \log \tilde{p}(\theta| d) + C $$ 

- **PROBLEM:** $p(\theta|d)$ is $$$
- **SOLUTION:**
  - sample from $p_{\rm joint}(\theta, d) = \mathcal{L}(d|\theta)\pi(\theta)$
  - Train $\tilde{p}(\theta|d)$
  
---

### Marginal SBI vs VI

**Variatinal inference**
- variational posterior $\tilde{p}(\vec{\theta}|d)$ must conver _all_ params likelihoodd model condditioned on

**SBI Marginal inference**
- Can replace $\tilde{p}(\vec{\theta}|d)$ for $\tilde{p}(\theta_1|d)$ without need of doing integrals



---

### END OF SECTION

{{% /section %}}

---


{{< slide background-image="https://user-images.githubusercontent.com/15642823/277592172-be608f89-4e27-489f-b3ab-48011968790d.jpeg">}}

## "Marginal" inference

$${\color{red}p(\theta_{\rm Waldo}| \rm{image})} =$$ 
$$\int {\color{blue}p(\theta_{A}, \theta_{B} ... \theta_{\rm Waldo}| \rm{image})}\ d\theta_A\ d\theta_B\ d\theta_{\rm Waldo} $$

- VI: have to learn _whole_ $\color{blue}p(\vec{\theta}|d)$
- SBI: can focus on specific params $\color{red}p(\theta_{\rm Waldo}|d)$


---

{{% section %}}

## Truncated Marginal Neural Ratio Estimation (TMNRE)

---

### Active learning loop

![loop]

[loop]: https://user-images.githubusercontent.com/15642823/277889707-8e9f5955-b8ac-44e0-8067-808a5ad189d2.png


---

### Network architecture

![network]

[network]: https://user-images.githubusercontent.com/15642823/277868586-284becb9-8f47-4ed9-9a92-6a3e7683470d.png


---

### Truncation example

![trunc]

[trunc]: https://user-images.githubusercontent.com/15642823/277902380-7807ed9e-99ae-40c4-b242-b7e9328306ec.png

---

### END OF SECTION

{{% /section %}}

---

{{% section %}}

## Results + Discussion

---

### MCMC vs SBI fit

{{< figure src="https://user-images.githubusercontent.com/15642823/277888874-1ab882f7-e3d1-47a9-a542-96101b8b92b5.png" width="500px">}}

[corner]: https://user-images.githubusercontent.com/15642823/277888874-1ab882f7-e3d1-47a9-a542-96101b8b92b5.png

---

### Future work

- comples noise model
- longer data duration
- other "SBI" blocks for the global fit

---

### SBI thoughts


- Focused analysis and 'implicit marginalisation' is neat! 
- Fewer evaluations of the model needed -- good in sitations where the model is expensive to evaluate
- doest use gradient information for lnL... sometimes we know the Lnl 
- How does SBI perform if all params are important?
- What if the model for noise/SGWB is not the best?
  - How would this work with a non-parametric model?

---

### END OF SECTION

{{% /section %}}

---

## Other related papers


- [Fast and credible likelihood-free cosmology with TMNRE](https://iopscience.iop.org/article/10.1088/1475-7516/2022/09/004/meta)
- [Improved reconstruction of a stochastic gravitational wave background with LISA](https://arxiv.org/abs/2009.11845)
- [Truncated Marginal Neural Ratio Estimation](https://arxiv.org/abs/2107.01214)
- [Scalable inference with Autoregressive Neural Ratio Estimation](https://arxiv.org/abs/2308.08597)
- [Fast Likelihood-free Reconstruction of Gravitational Wave Backgrounds](https://arxiv.org/pdf/2309.08430.pdf)
- [More plots](https://github.com/avivajpeyi/dev_site/wiki/Materials-for-SBI-journal-club-presentation)

---

