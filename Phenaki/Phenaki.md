### Introduction
In essence, videos are just a sequence of images, but this does not mean that generating a long coherent video is easy. In practice, it is a significantly harder task because there is much less high quality data available and the computational requirements are much more severe [9].

- lack of video text models not vast wnough
- Generate temporally coherent and diverse videos conditioned on open domain prompts even when the prompt is a new composition of concepts. The videos can be long (minutes) even though the **model is trained on 1.4 seconds videos (at 8 fps**).
- Generate videos conditioned on a story (i.e. a sequence of prompts)


### Model

![[Pasted image 20230306225245.png]]
The phenaki model constists of three main parts
- Left: C-ViViT encoder architecture. The embeddings of images and video patches from raw frames x are processed by a spatial and then a causal transformer (auto-regressive in time) to generate video tokens z. 
- Center: MaskGiT is trained to reconstruct masked tokens z predicted by a frozen C-ViViT encoder and conditioned on T5X tokens of a given prompt p0 . 
- Right: How Phenaki can generate arbitrary long videos by freezing the past token and generating the future tokens. The prompt can change over time to enable time-variable prompt (i.e.story) conditional generation. The subscripts represent time (i.e. frame number).


## C-ViViT
a novel encoder-decoder architecture that:– Exploits temporal redundancy in videos to improve reconstruction quality over a per frame model while compressing the number of video tokens by 40% or more.
- Allows encoding and decoding of variable length videos given its causal structure.
- a causal variation of ViViT [1] with additional architectural changes for video generation, which can **compress the videos in temporal and spatial dimensions, while staying auto-regressive in time**, This capability allows for generating videos of arbitrary length auto-regressively.
- This is followed by multiple transformer layers over the temporal dimension with causal attention such that each spatial token only observes spatial tokens from previous frames in an auto-regressive manner.
- we modify their architecture to enable self-supervised learn-ing from unlabeled videos.
## MaskGit
Masked bidirectional transformer: In this work, we aim to reduce the sampling time by having a small and fixed sampling step disregarding different video sequence lengths. Inspired by previous work for image generation [8], we use a bidirectional transformer since it can predict different video tokens simultaneously. For training step i, we first sample a mask ratio γi from 0 to 1 and randomly replace dγi · N e tokens with the special token [MASK], where N is the video sequence length. Then we learn the model parameters by minimizing the cross entropy loss on those masked tokens given the encoded text embeddings and unmasked video tokens. During inference, we first label all of the
video tokens as the special token [MASK]. Then, at each inference step, we predict all the masked (unknown) video tokens in parallel conditioned on the text embeddings and unmasked (predicted) video tokens. We keep a ratio βi of the predicted tokens at sampling step i and the remaining tokens are re-masked and re-predicted in the next step.
As discussed in MaskGIT [8], the masking schedule γi and sampling schedule βi have a significant effect on the samples quality therefore we follow the same strategies. Compared to an autoregressive transformer, the number of sampling steps is an order-of-magnitude smaller (typically we use values in the range of 12 to 48). Generally speaking, more sampling steps improves the quality.



