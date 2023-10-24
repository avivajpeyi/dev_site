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

# SBI Intro

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
### SBI: new term for

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

{{< slide background-image="https://user-images.githubusercontent.com/15642823/277592172-be608f89-4e27-489f-b3ab-48011968790d.jpeg">}}

## "Marginal" inference

$${\color{red}p(\theta_{\rm Waldo}| \rm{image})} =$$ 
$$\int {\color{blue}p(\theta_{A}, \theta_{B} ... \theta_{\rm Waldo}| \rm{image})}\ d\theta_A\ d\theta_B\ d\theta_{\rm Waldo} $$

- VI: have to learn _whole_ $\color{blue}p(\vec{theta}|d)$
- SBI: can focus on specific params $\color{red}p(\theta_{\rm Waldo}|d)$


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


## Code Highlighting

Inline code: `variable`

Code block:

```python
porridge = "blueberry"
if porridge == "blueberry":
    print("Eating...")
```

---

## Math

In-line math: $x + y = z$

Block math:

$$
f\left( x \right) = \;\frac{{2\left( {x + 4} \right)\left( {x - 4} \right)}}{{\left( {x + 4} \right)\left( {x + 1} \right)}}
$$

---

## Fragments

Make content appear incrementally

```
{{</* fragment */>}} $\mathbf{y} =  $ {{</* /fragment */>}}
{{</* fragment */>}} $X\boldsymbol\beta$ {{</* /fragment */>}}
{{</* fragment */>}} $+ \boldsymbol\varepsilon$ {{</* /fragment */>}}
```

Press `Space` to play!

{{< fragment >}} $\mathbf{y} =  $ {{< /fragment >}}
{{< fragment >}} $X\boldsymbol\beta$ {{< /fragment >}}
{{< fragment >}} $+ \boldsymbol\varepsilon$ {{< /fragment >}}

---

A fragment can accept two optional parameters:

- `class`: use a custom style (requires definition in custom CSS)
- `weight`: sets the order in which a fragment appears

---

## Speaker Notes

Add speaker notes to your presentation

```markdown
{{%/* speaker_note */%}}

- Only the speaker can read these notes
- Press `S` key to view

{{%/* /speaker_note */%}}
```

Press the `S` key to view the speaker notes!

{{< speaker_note >}}

- Only the speaker can read these notes
- Press `S` key to view

{{< /speaker_note >}}

---

## Themes

- black: Black background, white text, blue links (default)
- white: White background, black text, blue links
- league: Gray background, white text, blue links
- beige: Beige background, dark text, brown links
- sky: Blue background, thin dark text, blue links

---

- night: Black background, thick white text, orange links
- serif: Cappuccino background, gray text, brown links
- simple: White background, black text, blue links
- solarized: Cream-colored background, dark green text, blue links

---

{{< slide background-image="boards.webp" >}}

## Custom Slide

Customize the slide style and background

```markdown
{{</* slide background-image="boards.webp" */>}}
{{</* slide background-color="#0000FF" */>}}
{{</* slide class="my-style" */>}}
```

---

## Custom CSS Example

Let's make headers navy colored.

Create `assets/css/reveal_custom.css` with:

```css
.reveal section h1,
.reveal section h2,
.reveal section h3 {
  color: navy;
}
```

---

## Questions?

[Ask](https://discord.gg/z8wNYzb)

[Documentation](https://wowchemy.com/docs/content/slides/)

---

## SBI for SGWB

_Simulation based infernce for Stochastic GW background Analysis_
(Alvey et al, 2023)


[arxiv](https://arxiv.org/pdf/2309.07954.pdf) | [swyft](https://github.com/undark-lab/swyft)

NZ Gravity Journal Club

Oct 26th, 2023

---

## Summary

1. SBI intro
2. Alvey et al's model
3. Results + future work

---
