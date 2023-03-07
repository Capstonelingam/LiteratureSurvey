## Introduction
Our model, Imagen Video, is a cascade of video diffusion models (Ho et al., 2022a;b). It consists of **7 sub-models** which perform text-conditional video generation, spatial super-resolution, and temporal super-resolution. With the entire cascade, Imagen Video generates high definition **1280×768(width × height) videos at 24 frames per second, for 128 frames (≈ 5.3 seconds)—approximately126 million pixels**.

**- Videos are of fixed lenght.**

## Model
- videos are created by diffusion
- Learning to reverse the forward process for generation can be reduced to learning to denoise zt ∼q(zt |x) into an estimate x̂θ (zt , λt ) ≈ x for all t. Like (Song & Ermon, 2019; Ho et al., 2020) and most follow-up work, we optimize the model by minimizing a simple noise-prediction loss:

### Cascaded diffusion models
- Cascaded Diffusion Models (Ho et al., 2022a) are an effective method for scaling diffusion models to **high resolution outputs**, finding considerable success in both class-conditional ImageNet and text to image generation.
- **Cascaded diffusion models generate an image or video at a low resolution, then sequentially increase the resolution of the image or video through a series of super-resolution diffusion models**. Cascaded Diffusion Models can model very high dimensional problems while still keeping each sub-model relativelysimple.
- **Imagen MODEL**
![[Pasted image 20230307085637.png]]
#### SSR Models - Increase spatial resolution for input frames
#### TSR Models - Increase temporal resolution by interpolating frames between frames


- One benefit of cascaded models is that each diffusion model can be trained independently, **allowing one to train all 7 models in parallel**. Additionally, our super-resolution models are general purpose video super-resolution models, and they can be applied to **real videos or samples from generative models other** than the ones presented in this paper.


### Video diffusion architecture
- **2D U-NET ARCHITECTURE** is used are a base model 2D diffusion model archtecture into 3D in a space-time seperable fashion that us to capture dependencies between 2 frames of a video
- Our base video model, which is the first model in the pipeline that generates data at the **lowest frame count and spatial resolution**, uses **temporal attention to mix information across time**. Our SSR and TSR models, on the other hand, **use temporal convolutions instead of temporal attention.**

##### v- PREDICTION - used for fixing color shifting across all models


## Datasets
- We train our models on a combination of an internal dataset consisting of 14 million video text pairs and 60 million image-text pairs, and the publicly available LAION-400M image-text dataset.
- **Evaluation**- FID on individual frames (Heusel et al., 2017), FVD (Unterthiner et al., 2019) for temporal consistency, and frame-wise CLIP scores (Hessel et al., 2021; Park et al., 2021) for video- text alignment.

