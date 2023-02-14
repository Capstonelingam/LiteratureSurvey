# CogView2

- **transformer** based text to image generation model.

## Pretraining

### CogLM
- Cross Modal general Language model.
- CogLM masks various types of tokens in the sequence of text and image tokens, and learns to predict them autoregressively.

Quoting the paper
- First, we generate a batch of low-resolution images (20 × 20 tokens in CogView2) using the **pretrained CogLM**, and then (optionally) filter out the **bad samples** based on the perplexity of CogLM image captioning, which is the post-selection method introduced in CogView.
- generated images are mapped into 60 × 60-token images by a direct super-resolution module fine-tuned from the pretrained CogLM. These high resolution images usually have inconsistent textures.
- These high-resolution images are refined via another iterative **super-resolution module** fine-tuned from the pretrained CogLM. Most tokens are **re-masked** and **re-generated** in a localparallel autoregressive (LoPAR) way, which is much faster than the original autoregressive generation.


## Method
- CogLM takes as input a concatenation of text and images tokenized by icetk 1 (See § 3.2), whose dictionary contains **20,000 image** tokens and **130,000 text** (both Chinese and English) tokens.
- Concept of building an **attention mask** is used. Where outside mask tokens are called as **context**.
- In the mask regions, the model learns to predict the next token.
![[Pasted image 20230214111328.png]]

Super-resolution modules. Low-resolution images are mapped into high-resolution images
via the direct super-resolution module. In each snapshot during the iterative super-
resolution, all tokens of the same color are generated at the same time. All the local windows work in parallel.

### Local attention
- However, 2D local attention cannot be implemented efficiently using high-level framework, e.g. Pytorch [20]. We develop a customized CUDA kernelto support both 2D local attention, 2D autoregressive local attention and cross-resolution local attention.



## DATASET USED
- **CogView** dataset 5 million text image pairs

## TOOLS USED
### Text tokeniser
- The text tokenizer is trained based on the sentencepiece 3 (unigram algorithm [16]) on a mixed corpus of both English and Chinese. The corpus consists of 25GB plain text – half English, half Chinese.
### Image tokeniser
- The image tokenizer is a multi-compression-rate VQVAE 4 . The main differences between the image tokenizer of CogView2 and that of CogView are mainly the perceptual loss and the multi-compression-rate design.
### VQGAN- To train text image tokens