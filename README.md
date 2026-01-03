# AutoencoderTshirtGenerator (Experimental)  
**T-shirt → Mannequin “Try-On” (AE / VAE / GAN learning project)**

This repo is a **learning / experiment project** I built around 2022–2023 while trying to really understand how **Autoencoders, VAEs and GANs** behave in practice.

The original idea was simple:

> **Input:** a T-shirt image  
> **Output:** a generated image showing the T-shirt “worn” on a mannequin/person

I didn’t reach a perfect try-on result — and that’s exactly what made this project valuable to me:  
I learned *why* some generative approaches get stuck, what “model capacity” really means, and what extra components are needed for realistic results.

---

## Why I built this
I was fascinated by the “magic” of generative AI:  
**give a model an input image → it learns to produce a new image that follows rules**.

This project was my way of learning that magic hands-on:
- how an **encoder/decoder** compresses and reconstructs images
- how **latent space** and bottlenecks affect details
- why outputs become **blurry/noisy**
- how **conditional generation** changes the game (conditioning both G and D)

---

## What’s inside
There are two main parts:

### 1) Notebook experiments (Autoencoder baselines)
- `autoencoder_rgb_base.ipynb`
- `autoencoder_all_test2.ipynb`

These were my early iterations to understand reconstruction and training dynamics.

### 2) Training scripts (conditional GAN attempt)
- `dataset.py`  → builds paired samples + condition vector  
- `moduls_.py`  → Generator & Discriminator  
- `main.py`     → training loop + checkpoints + output saving

At some point the project evolved into a **conditional GAN** (pix2pix-like direction), where:
- the **Generator** tries to produce a mannequin output given the input T-shirt (+ conditions)
- the **Discriminator** judges (input + output) pairs as real or fake

---

## Dataset format (paired)
My loader expects folders like `model_*` that contain:
- `input_<id>.jpg`  (T-shirt / input)
- `output_<id>.jpg` (mannequin target)
- `info.txt`        (metadata)

From `info.txt`, I build a small condition vector:
- fit type: `oversize | normal | slim` (one-hot)
- hair: `short | medium` (one-hot)
- skin: numeric value

So each training sample is basically:
**(input_image, condition_vector) → target_output_image**

---

## Model idea (high level)
### Baseline: Autoencoder
Encoder compresses image → Decoder reconstructs.

### Next step: conditional GAN
The Generator encodes the input image and concatenates:
- encoded features
- condition embedding (from metadata)
- noise embedding
then decodes back to an image.

The Discriminator sees:
- `input_image` + `generated_or_real_output` together
and also receives the condition vector.

---

## Training notes
I saved checkpoints and inspected outputs frequently because this project was about learning:
- “Is it improving or just memorizing?”
- “Do I need a bigger model?”
- “Is the dataset alignment good enough?”
- “When does it start collapsing / producing noise?”

---

## Sample outputs
Some outputs are noisy/blurry — this is expected for this setup.

### AI7 output example
If you have the folder in the repo, you can embed outputs like this:

![AI7 output sample](AI7/OUT_{str(id_)}.png)

> If your filename differs (or you have multiple outputs), just update the path.  
> Example: `AI7/OUT_200.png`, `AI7/OUT_400.png`, etc.

---

## References / inspiration
- Conditional GANs (Mirza & Osindero, 2014)
- pix2pix: Image-to-Image Translation with Conditional GANs (Isola et al., 2017)
