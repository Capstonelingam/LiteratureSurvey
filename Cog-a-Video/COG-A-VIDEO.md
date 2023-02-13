# CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers

---
[[2211.13319.pdf]]
## Some Abstracts
- In this work, we present **9B-parameter** transformer CogVideo, trained by inheriting a
pretrained **text-to-image model, CogView2**. We also propose multi-frame-rate
hierarchical training strategy to better align text and video clips.
- Cog2Video is **open source.**
- Source code avaliable at https://github.com/THUDM/CogVideo.
![[Pasted image 20230211132639.png]]

---
## Challenges faced

- **generated video frames tend to gradually deviate** from the text
prompt, making the generated characters hard to perform the desired actions.
- **Dataset problems** - text-video data are more scarce. The largest annotated text-video dataset, **VATEX [31], has only 41,250 videos**. The retrieval-based text-video pairs, e.g. **Howto100M [16**], are weakly relevant and most of them only describe the scene **without the temporal information.**
---

## Method

### Multi-frame-rate Hierarchical Training.
Here we present the multi-frame-rate hierarchical training and generation. We follow the framework of **VQVAE** [28] and f**irst tokenize each frame into image tokens**. Each training sample consists of 5 frames of tokens, but our training method differs in the construction of training sequences and generation process.
#### Training - PRETRAINING
The key design is to **add a frame-rate token to th**e text and sample frames at this frame rate
to compose a fixed-length training sequence. The motivations are two folds:
- Seperating **long video into clips at fixed frame rate** often leads to **semantic mismatching** .Full clips used but the truncated clip may only contain incomplete actions
- Adjacent frames are simliar . **A giant change over frames ie lets say a scene change will cause high error**. Models will be less inclined to explore long range correlation.
![[Pasted image 20230211134750.png]]

- Therefore, in each training sample, **we want the text and the frames to match as possible**. We predefined a **series of frame rates, and select the lowest frame rate for each text-video pair**, as long as we can sample at least **5 frames at this frame rate in the video**.


#### Generation
The multi-frame-rate hierarchical generation is a recursive process.
- **Sequentially generate Ts** key frames based on a **low frame rate and text.** The input sequence is **[{Frame Rate}{Text} [B] {Frame1} ... {Frame Ts }]**. In practice, we always set Ts = 5 and the minimum sampling frame rate to 1 fps.
- **Recursively interpolate frames based on the text,** frame rate and known frames. In each round of interpolation, we split generated frames into multiple d T2s e-frame blocks overlapping at the beginning and the end, **and interpolate a frame between the successive frames in each block**.The input sequence is [{Frame Rate}{Text} [B] {Frame1} ... {Frame Ts }], where Frame 2i(i = 1, 2, ..., b T2s c) are to be autoregressively generated. By recursively halfing {Frame Rate}, we can conduct finer and finer interpolation to generate videos of many frames.



### DUAL CHANNEL ATTENTION
![[Pasted image 20230211140145.png]]
- CogView2 already has a good command on text to image relations.
- where we only add a new **spatial-temporal attention channel to the pretrained CogView2** [6] at **each transformer layer**. All the parameters in the CogView2 are **frozen in the training**, and only the parameters in the newly added attention layer (See the attention-plus in Figure 3) are trainable. We denote the original attention block in CogView2 as attention-base.
##### MODIFIED AN EXISTING T2I Model to add spacial-temopral attention.
In our training, we tried two kinds of attention, 3D local attention and 3D Swin [14] attention for attention-plus block. Here we depict the 3D local attention, and the latter is a natural replacement introduced in section 3.3. In 3D local attention, the receptive field (RF) for the token at (t, x, y) (where (t, x, y) corresponds tothe coordination along time, height and width), is a 3D block with extent

### SWIN-Attention
the Swin atten-tion provide a chance for parallel generation in faraway regions of different frames.
- Auto-regressive mask. A token can only attend to previous frames or tokens before itself in the current frame.
- Shifted window. Only tokens within the distance of window size in both width and height dimensions can be directly attended to.

# Dataset used - captioned videos
5.4 millions videos - frame rates adjusted





## SOME INPUTS
- Recent autoregressive transformers [17, 36, 34, 35] have also shown their superiority in video generation. Among them, GODIVA [34] and NÜWA [35] focus on the open-domain text-to-video generation. However, they simply generate frames or frame blocks one by one in a chronological order, and may suffer from poor text-video alignment (Cf. § 1).