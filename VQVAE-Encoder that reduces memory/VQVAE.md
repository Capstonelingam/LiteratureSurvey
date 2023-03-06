# VQVAE

### GITHUB LINK- https://avdnoord.github.io/homepage/vqvae

---

Learning useful representations without supervision remains a key challenge in machine learning. In this paper, we propose a simple yet powerful generative model that learns such discrete representations.Our model, the Vector Quantised-Variational AutoEncoder (VQ-VAE), differs from VAEs in two key ways: the encoder network outputs discrete, rather than continuous, codes; and the prior is learnt rather than static. In order to learn a discrete latent representation, we incorporate ideas from vector quantisation (VQ). Using the VQ method allows the model to circumvent issues of "posterior collapse" -- where the latents are ignored when they are paired with a powerful autoregressive decoder -- typically observed in the VAE framework. Pairing these representations with an autoregressive prior, the model can generate high quality images, videos, and speech as well as doing high quality speaker conversion and unsupervised learning of phonemes, providing further evidence of the utility of the learnt representations.

---

Q-VAE, a new family of models that combine VAEs with vector quantisation to obtain a discrete latent representation. They argue for learning discrete and useful latent variables. The group train these models using the same standard VAE architecture on CIFAR10, while varying the latent capacity. van den Oord and colleagues believe that this is the first discrete latent variable model that can successfully model long range sequences and fully unsupervisedly learn high-level speech descriptors.

The researchers use ADAM optimiser with learning rate 2e-4 and evaluate performance after 250,000 steps. VAE, VQ-VAE and VIMCO models obtain 4.51 bits/dim, 4.67 bits and 5.14 respectively.

---

## Discrete latent variable
We define a latent embedding space e ∈ RK×D where K is the size of the discrete latent space (i.e.,
a K-way categorical), and D is the dimensionality of each latent embedding vector ei . Note that
there are K embedding vectors ei ∈ RD , i ∈ 1, 2, ..., K. As shown in Figure 1, the model takes an
input x, that is passed through an encoder producing output ze (x). The discrete latent variables z
are then calculated by a nearest neighbour look-up using the shared embedding space e as shown in
equation 1.

-  The input to the decoder is the corresponding embedding vector ek as given in equation 2. One can see this forward computation pipeline as a regular autoencoder with a particular non-linearity that maps the latents to 1-of-K embedding vectors. The complete set of parameters for the model are union of parameters of the encoder, decoder, and the embedding space e.


# method
We train these models using the same standard VAE architecture on CIFAR10, while varying the latent capacity (number of continuous or discrete latent variables, as well as the dimensionality of the discrete space K). The encoder consists of 2 strided convolutional layers with stride 2 and window size 4 × 4, followed by two residual 3 × 3 blocks (implemented as ReLU, 3x3 conv, ReLU, 1x1 conv), all having 256 hidden units. The decoder similarly has two residual 3 × 3 blocks, followed by two transposed convolutions with stride 2 and window size 4 × 4. We use the ADAM optimiser [21] with learning rate 2e-4 and evaluate the performance after 250,000 steps with batch-size 128. For VIMCO we use 50 samples in the multi-sample training objective.

# Why to use vae
- xperiment we show that we can model x = 128 × 128 × 3 images by compressing them to a z = 32 × 32 × 1 discrete space (with K=512) via a purely deconvolutional p(x|z). So a reduction of 128×128×3×8 ≈ 42.6 in bits.
- Essentially use for image compression/ feature etarction by eliminating redundant pixels.


# In this work we have introduced VQ-VAE, a new family of models that combine VAEs with vector
quantisation to obtain a discrete latent representation. We have shown that VQ-VAEs are capable of
modelling very long term dependencies through their compressed discrete latent space which we have
demonstrated by generating 128 × 128 colour images, sampling action conditional video sequences
and finally using audio where even an unconditional model can generate surprisingly meaningful
chunks of speech and doing speaker conversion. All these experiments demonstrated that the discrete
latent space learnt by VQ-VAEs capture important features of the data in a completely unsupervised
manner. Moreover, VQ-VAEs achieve likelihoods that are almost as good as their continuous latent
variable counterparts on CIFAR10 data. We believe that this is the first discrete latent variable model
that can successfully model long range sequences and fully unsupervisedly learn high-level speech
descriptors that are closely related to phonemes.

- can be used for image , video , audio etc.