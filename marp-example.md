---
marp: true
math: mathjax
# theme: rose-pine
# theme: rose-pine-dawn
theme: rose-pine-moon 
# theme: uncover

---

<style lang=css>
/* @theme rose-pine */
/*
Rosé Pine theme create by RAINBOWFLESH
> www.rosepinetheme.com
MIT License https://github.com/rainbowflesh/Rose-Pine-For-Marp/blob/master/license

palette in :root
*/

@import "default";
@import "schema";
@import "structure";

:root {
    --base: #191724;
    --surface: #1f1d2e;
    --overlay: #26233a;
    --muted: #6e6a86;
    --subtle: #908caa;
    --text: #e0def4;
    --love: #eb6f92;
    --gold: #f6c177;
    --rose: #ebbcba;
    --pine: #31748f;
    --foam: #9ccfd8;
    --iris: #c4a7e7;
    --highlight-low: #21202e;
    --highlight-muted: #403d52;
    --highlight-high: #524f67;

    font-family: Pier Sans, ui-sans-serif, system-ui, -apple-system,
        BlinkMacSystemFont, Segoe UI, Roboto, Helvetica Neue, Arial, Noto Sans,
        sans-serif, "Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol,
        "Noto Color Emoji";
    font-weight: initial;

    background-color: var(--base);
}





/* Common style */
h1 {
    color: var(--rose);
    padding-bottom: 2mm;
    margin-bottom: 12mm;
}
h2 {
    color: var(--rose);
}
h3 {
    color: var(--rose);
}
h4 {
    color: var(--rose);
}
h5 {
    color: var(--rose);
}
h6 {
    color: var(--rose);
}
a {
    color: var(--iris);
}
p {
    font-size: 20pt;
    font-weight: 600;
    color: var(--text);
}
code {
    color: var(--text);
    background-color: var(--highlight-muted);
}
text {
    color: var(--text);
}
ul {
    color: var(--subtle);
}
li {
    color: var(--subtle);
}
/* img {
    background-color: var(--highlight-low);
} */
strong {
    color: var(--text);
    font-weight: inherit;
    font-weight: 800;
}
mjx-container {
    color: var(--text);
}
marp-pre {
    background-color: var(--overlay);
    border-color: var(--highlight-high);
}

/* Code blok */
.hljs-comment {
    color: var(--muted);
}
.hljs-attr {
    color: var(--foam);
}
.hljs-punctuation {
    color: var(--subtle);
}
.hljs-string {
    color: var(--gold);
}
.hljs-title {
    color: var(--foam);
}
.hljs-keyword {
    color: var(--pine);
}
.hljs-variable {
    color: var(--text);
}
.hljs-literal {
    color: var(--rose);
}
.hljs-type {
    color: var(--love);
}
.hljs-number {
    color: var(--gold);
}
.hljs-built_in {
    color: var(--love);
}
.hljs-params {
    color: var(--iris);
}
.hljs-symbol {
    color: var(--foam);
}
.hljs-meta {
    color: var(--subtle);
}

  .columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }
  .reference {
    position: absolute;
    bottom: 10px;
    right: 10px;
    font-size: 18px; /* Adjust the font size as needed */
    color: #888;    /* Adjust the color as needed */
  }




    .custom-invert {
    --color-background: #ebbcba
    }


  .split-image-slide {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0;
    margin: 0;
    }



</style>


<div align="center">

#             $\;\;\;\;\;\;\;\;$ Boosting Generative Image Modeling via $\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;$ Joint Image Feature Syntheis

</div>

 $\quad\quad\quad\quad\quad\quad\;$Theodoros Kouzelis, Eystathis Karypidis, Ioannis Kakogeorgiou, $\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad$ Spyros Gidaris, Nikos Komodakis



---

### Diffusion / Flow Models (in a nutshell)

  - <span style="font-size:70%"> Given an Image $\mathbf{x}$ and Gaussina noise $\boldsymbol{\epsilon} \sim \mathcal{N}(0,I)$

  - <span style="font-size:70%"> Construct a time dependent noising process:
    - $\mathbf{x}_t = \alpha_t \mathbf{x} + \sigma_t \boldsymbol{\epsilon}$ 
  - <span style="font-size:70%"> Reverse process to generate data from noise:
    - $\mathbf{x}_{t-1} = \mathbf{\tilde{x}_t} +  \boldsymbol{w}_t  \cdot \text{Network Output}$ 

</div>

$\;\;\;\;\;\;\;\;$ ![width:1000px ](figs/dm_overview.png)

---


### Diffusion / Flow Models


$\;\;\;\;\;\;\;\;$ ![width:1000px ](figs/dif_gif.webp)


<div class="reference">Diffusion Models for Image Generation https://learnopencv.com/image-generation-using-diffusion-models/
</div>


---




### Denoising Objective 

  - <span style="font-size:70%"> Construct training sample $\mathbf{x}_t = \alpha_t \mathbf{x} + \sigma_t \boldsymbol{\epsilon}$ 



$\;\;\;\;\;\;\;\;$ ![width:500px ](figs/denoising_0.png)



---


### Denoising Objective 

  - <span style="font-size:70%"> Give $\mathbf{x_t}$ as input to a denoising network $\epsilon_\theta$



$\;\;\;\;\;\;\;\;$ ![width:780px ](figs/denoising_1.png)



---


### Denoising Objective 

  - <span style="font-size:70%"> Loss funtion $\Vert \epsilon_\theta(\mathbf{x_t})  - \epsilon \Vert_2^2$



$\;\;\;\;\;\;\;\;$ ![width:1000px ](figs/denoising.png)



---



### The problem with the Denoising Objective 

  - <span style="font-size:70%"> Loss funtion $\Vert \epsilon_\theta(\mathbf{x_t})  - \epsilon \Vert_2^2$



  - <span style="font-size:70%"> Not capable of eliminating unnecessary details in $\mathbf{x}$

  - <span style="font-size:70%"> Does not result in good representations




---

### DINO (in a nutshell)


-  <span style="font-size:70%">Take different crops from an image 

-  <span style="font-size:10%"> Force representations to be similar


  - <span style="font-size:70%"> Very strong representations

<div class="reference">Caron et al., 2021, Emerging Properties in Self-Supervised Vision Transformers

</div>

![bg right width:600px ](figs/dino.png)




---








### Representation Alignment 

-  <span style="font-size:70%">Align features with the representations of powerful visual encoders</span>. 



<div class="reference">Yu et al., 2025, Representation Alignment for Generation: Training Diffusion Transformers Is Easier Than You Think
</div>

![bg right width:600px ](figs/repa.png)



---


### Representation Alignment 

-  <span style="font-size:70%"> Let $h_t$ be the an intermediate feature


-  <span style="font-size:70%"> $y_* = f(x)$ (e.g. DINOv2)



-  <span style="font-size:70%"> and $h_\phi$ a simple projection layer


-  <span style="font-size:70%"> REPA loss:
    - $-\mathbb{E}[\frac{1}{N} \sum_i \text{sim}((y_*^i), h_\phi(h_t^i))]$





![bg right width:600px ](figs/repa.png)



---




## Representation Alignment 

-  <span style="font-size:70%">$\times17$ faster convergence


-  <span style="font-size:70%">Better generative performance


<div class="reference">Yu et al., 2025, Representation Alignment for Generation: Training Diffusion Transformers Is Easier Than You Think
</div>

![bg right width:600px ](figs/repa_speed.png)



---



## Representation Alignment for VAE Latents

-  <span style="font-size:70%">Aligning the **VAE latents** with DINOv2 also results in better generative performance

<div class="reference">Yao et al., 2025, Reconstruction vs. Generation: Taming Optimization Dilemma in Latent Diffusion Models

</div>

![bg right width:600px ](figs/vavae.png)



---



## ReDi: Joint image-feature Synthesis

###### 



$\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad$ ![width:150px ](figs/teaser_1.png)



---


## ReDi: Joint image-feature Synthesis

###### Joint Forward Process

$\;\;\;\;\;\;\;\;$ ![width:980px ](figs/teaser_5.png)



---

### ReDi: Joint image-feature Synthesis

###### Joint Reverse Process


$\;\;\;\;\;\;\;\;$  ![width:980px ](figs/teaser_6.png)



---




### ReDi: Joint image-feature Synthesis

<br>

<br>

$\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;$  ![width:700px ](figs/teaser_7.png)



---


### Joint Forward Process

  - <span style="font-size:70%"> VAE Latents:  $\mathbf{x}_t = \sqrt{\bar{\alpha}_t}\mathbf{x}_0 + \sqrt{1-\bar{\alpha}_t} \boldsymbol{\epsilon}_x$


  - <span style="font-size:70%"> DINOv2:  $\mathbf{z}_t = \sqrt{\bar{\alpha}_t}\mathbf{z}_0 + \sqrt{1-\bar{\alpha}_t} \boldsymbol{\epsilon}_z$




$\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;$ ![width:780px ](figs/teaser_5.png)



---

### Joint Denoising Objective

  - <span style="font-size:70%"> $\mathcal{L}_{joint} = \underset{\mathbf{x}_0, \mathbf{z}_0} { \mathbb{E}} \Big[
    \Vert {\boldsymbol{\epsilon}^x_\theta}(\mathbf{x}_t,\mathbf{z}_t, t) - \boldsymbol{\epsilon}_x \Vert_2^2
    +\Vert {\boldsymbol{\epsilon}^z_\theta}(\mathbf{x}_t,\mathbf{z}_t, t) - \boldsymbol{\epsilon}_z \Vert_2^2 \Big]$




$\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;$ ![width:680px ](figs/denoising_joint.png)

---

### Fusing the two modelities in a single input sequence

-  <span style="font-size:70%">Merged Tokens

-  <span style="font-size:70%">Separete Tokens

</div>

![bg right width:600px ](figs/fusion.png)



---


### Fusing the two modelities in a single input sequence

-  <span style="font-size:70%"> Modality-specific **embedding** matrices:
    - $\mathbf{W}_{\text{emp}}^x \in \mathbb{R}^{C_x \times C_d}$
    - $\mathbf{W}_{\text{emp}}^z \in \mathbb{R}^{C_z \times C_d}$
-  <span style="font-size:70%"> Modality-specific **projection** matrices:
    - $\mathbf{W}_{\text{dec}}^x \in \mathbb{R}^{C_d \times C_x}$
    - $\mathbf{W}_{\text{dec}}^z \in \mathbb{R}^{C_d \times C_x}$


![bg right width:600px ](figs/fusion.png)



---



### Merged Tokens

<span style="font-size:70%"> The tokens are summed channel-wise:
-  <span style="font-size:70%"> $\mathbf{h}_t = \mathbf{x}_t \mathbf{W}_{\text{emb}}^x + \mathbf{z}_t \mathbf{W}_{\text{emb}}^z \in \mathbb{R}^{L \times C_d}$


-  <span style="font-size:70%"> The transformer processes $h_t$ to produce $o_t$

-  <span style="font-size:70%"> $\boldsymbol{\epsilon}^x_\theta = \mathbf{o}_t  \mathbf{W}_{\text{dec}}^x, \quad  
\boldsymbol{\epsilon}^z_\theta = \mathbf{o}_t \mathbf{W}_{\text{dec}}^z.$

</div>

![bg right width:400px ](figs/fusion_m.png)



---

### Merged Tokens

-  <span style="font-size:70%">  Early fusion.


-  <span style="font-size:70%">  Maintains computational efficiency, as the token **count remains unchanged**.



</div>

![bg right width:400px ](figs/fusion_m.png)



---







### Separete Tokens

<span style="font-size:70%"> Tokens are concatenated along the sequence dimension:
-  <span style="font-size:70%"> $\mathbf{h}_t = [\mathbf{x}_t \mathbf{W}_{\text{emb}}^x \,, \, \mathbf{z}_t \mathbf{W}_{\text{emb}}^z] \in \mathbb{R}^{2L \times C_d},$


-  <span style="font-size:70%">  The transformer outputs separate representations $\mathbf{o}_t = [\mathbf{o}_t^x \,, \, \mathbf{o}_t^z]$

-  <span style="font-size:70%"> $\boldsymbol{\epsilon}^x_\theta = \mathbf{o}^x_t  \mathbf{W}_{\text{dec}}^x, \quad  
\boldsymbol{\epsilon}^z_\theta = \mathbf{o}^z_t \mathbf{W}_{\text{dec}}^z.$



![bg right width:400px ](figs/fusion_s.png)




---


### Separete Tokens


-  <span style="font-size:70%"> Preserves modality-specific information.
-  <span style="font-size:70%">  **Increased computation** due to increased token count ($\times 2$).




![bg right width:400px ](figs/fusion_s.png)

---

### Dimensionality-Reduced Visual Representation

-  <span style="font-size:70%"> The channels of $\texttt{DINOv2}$ significantly exceeds that of $\texttt{VAE latent}$



![bg right width:600px ](figs/v_d.png)

---

### Dimensionality-Reduced Visual Representation

-  <span style="font-size:70%"> Sample $N$ features from training set and calculate a PCA projection matrix $P_z$ 


![bg right width:600px ](figs/pca.png)

---


### Dimensionality-Reduced Visual Representation


-  <span style="font-size:70%"> Project to principal subspace: $z_{pca} = z \times P_z$

![bg right width:600px ](figs/pca.png)

---
## Redi Pipeline

$\;\;\;\;\;\;\;\;\;\;$ ![ width:1000px ](figs/method.png)

---


## Represenation Guidance


-  <span style="font-size:70%">  Inspired by Classifier-Free Guidance.


-  <span style="font-size:70%">  During inference we modify the posterior distribution to: $\hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t) \propto p_\theta(\mathbf{x}_t) p( \mathbf{z}_t \vert \mathbf{x}_t)^{w_r}$

- Samples are pushed toward higher likelihood of the conditional distribution $p_\theta(\mathbf{z}_t | \mathbf{x}_t)$


---


## Represenation Guidance


-  <span style="font-size:70%">  Taking the log derivative yields the guided score function:
$$
\begin{align}
    \nabla_{\!\mathbf{x}_t} \text{log} \; \hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t)=&
        \nabla_{\!\mathbf{x}_t} \text{log} \; \big[p_\theta(\mathbf{x}_t) p( \mathbf{z}_t \vert \mathbf{x}_t)^{w_r}
        \big] \\


\end{align}
$$


---




## Represenation Guidance


-  <span style="font-size:70%">  Taking the log derivative yields the guided score function:
$$
\begin{align}
    \nabla_{\!\mathbf{x}_t} \text{log} \; \hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t)=&
        \nabla_{\!\mathbf{x}_t} \text{log} \; \big[p_\theta(\mathbf{x}_t) p( \mathbf{z}_t \vert \mathbf{x}_t)^{w_r}
        \big] \\

    =&
        \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)+
        w_r\big(
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{z}_t \vert \mathbf{x}_t)
        \big) \\

\end{align}
$$


---



## Represenation Guidance


-  <span style="font-size:70%">  Taking the log derivative yields the guided score function:
$$
\begin{align}
    \nabla_{\!\mathbf{x}_t} \text{log} \; \hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t)=&
        \nabla_{\!\mathbf{x}_t} \text{log} \; \big[p_\theta(\mathbf{x}_t) p( \mathbf{z}_t \vert \mathbf{x}_t)^{w_r}
        \big] \\
    \nabla_{\!\mathbf{x}_t} \text{log} \; \hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t)=&
        \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)+
        w_r\big(
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{z}_t \vert \mathbf{x}_t)
        \big) \\
        =& \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)+
        w_r\big(
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t, \mathbf{z}_t)-
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)
        \big).
\end{align}
$$


---

## Represenation Guidance


$$
\begin{align}
    \nabla_{\!\mathbf{x}_t} \text{log} \; \hat{p}_\theta(\mathbf{x}_t, \mathbf{z}_t)
        =& \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)+
        w_r\big(
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t, \mathbf{z}_t)-
    \nabla_{\!\mathbf{x}_t} \text{log} \;p_\theta(\mathbf{x}_t)
        \big).
\end{align}
$$


-  <span style="font-size:70%">  From the equivalance between denoisers and scores we have:
$\boldsymbol{\hat{\epsilon}}_\theta(\mathbf{x}_t, \mathbf{z}_t, t) = \boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t) + w_r\left(\boldsymbol{\epsilon}_\theta(\mathbf{x}_t, \mathbf{z}_t, t) - \boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t)\right).$


---



## Represenation Guidance




$\boldsymbol{\hat{\epsilon}}_\theta(\mathbf{x}_t, \mathbf{z}_t, t) = \boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t) + w_r\left(\boldsymbol{\epsilon}_\theta(\mathbf{x}_t, \mathbf{z}_t, t) - \boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t)\right).$



- We train both $\boldsymbol{e_\theta}(\mathbf{x}_t, \mathbf{z}_t, t)$ and $\boldsymbol{e_\theta}(\mathbf{x}_t, t)$ jointly. 

- With probability $p_{drop}=0.2$:
    - Zero out $\mathbf{z}_t$ (setting $\boldsymbol{\epsilon}_\theta(\mathbf{x}_t, t) = \boldsymbol{\epsilon}_\theta(\mathbf{x}_t, \mathbf{0}, t)$)
    
    - Disable the visual representation denoising loss. 



---

# Experimental Results 🔬 📊

---



## Ablation
### Dimentionality reduction 

- Intermediate subspace ($r=8$)
- Retains sufficient expressivity to guide generation 
- Does not dominate model capacity.


![bg right width:650px](figs/pca-1.png)

---


## Ablation

###### Merged Tokens vs. Separate Tokens.
![bg right invert  width:600px ](figs/spvsmr.png)


---


###### Merged Tokens vs. Separate Tokens.

Merged Tokens

- ~Same training and infernce compute

![bg right invert  width:600px ](figs/spvsmr.png)






---


###### Merged Tokens vs. Separate Tokens.

Merged Tokens

- ~Same training and infernce compute

![bg right invert  width:600px ](figs/spvsmr.png)



Separete Tokens

- ~$\times 2$ training and infernce compute
- Slightly better performance

---




## Main benchmark
###### Conditional Generation

- $\sim \times 6$ faster than $\texttt{REPA}$

- Converged performance :
    - $\texttt{REPA}: \;\;$ 5.9 FID

    - $\texttt{ReDi}: \;\;$ 3.3 FID


![bg right invert  width:500px ](figs/fid_main.png)



---




## Main benchmark
###### Conditional Generation w/ CFG



![bg right invert  width:600px ](figs/fid_cfg.png)


---

# Selected Samples

$\;\;\;\;$ ![ width:1200px ](figs/exa.png)


---



### Unconditional Generation 



- $\text{Representation Guidance} \; (\texttt{RG})$ is very usefull for unconditional generation

- $\texttt{ReDi}$ significantly closes the gap between conditional and unconditional generation


![bg right invert  width:600px ](figs/fid_uncond.png)


---

# Representation Guidance

$\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;$ ![ width:800px ](figs/rg.png)


---


# Possible Future Research Directions 

 - Multiple Representations (e.g. $\texttt{DINOv2}$ and $\texttt{CLIP}$)


 - Different dimentionality reduction approach

 - Leverege the generated representations

---

