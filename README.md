# Kandinsky 6

# Kandinsky6 GGUF for ComfyUI

GGUF-compatible Kandinsky 6 Pro nodes for my **Kandinsky 6.0 Pro 5s** and **Kandinsky 6.0 Pro Distilled 5s** GGUF releases.

This is a modified version of the official Kandinsky 6 ComfyUI implementation with the additional compatibility handling required to run Kandinsky through **ComfyUI-GGUF**.

It supports:

- **Text-to-video + audio**
- **Image-to-video + audio**
- **Kandinsky 6.0 Pro 5s**
- **Kandinsky 6.0 Pro Distilled 5s**
- **GGUF Q3_K_S / Q4_K_S / Q5_K_S / Q6_K**
- **10-step PiFlow inference for Distilled Pro**
- **50-step inference + MagCache support for regular Pro**

> This repository is intended for the GGUF releases from **RealRebelAI**.  
> If you are using the native/W4A8 Kandinsky models instead, use the official Kandinsky 6 nodes.

---

# Installation

GGUF users need **both** of my compatibility repositories:

### 1. Kandinsky6_GGUF

This repository provides the Kandinsky-side GGUF compatibility changes.

```bash
git clone https://github.com/RealRebelAI/Kandinsky6_GGUF.git
```

Clone it into:

```text
ComfyUI/custom_nodes/Kandinsky6_GGUF
```

### 2. ComfyUI-GGUF-Rebel

My ComfyUI-GGUF fork is also required:

https://github.com/RealRebelAI/ComfyUI-GGUF-Rebel

```bash
git clone https://github.com/RealRebelAI/ComfyUI-GGUF-Rebel.git
```

Clone it into:

```text
ComfyUI/custom_nodes/ComfyUI-GGUF-Rebel
```

Your custom nodes folder should look roughly like:

```text
ComfyUI/
└── custom_nodes/
    ├── Kandinsky6_GGUF/
    └── ComfyUI-GGUF-Rebel/
```

Restart ComfyUI after installing both repositories.

> **Do not install the official Kandinsky6 node package alongside `Kandinsky6_GGUF`.**
>
> `Kandinsky6_GGUF` is the Kandinsky node set for the GGUF workflow and replaces the stock Kandinsky nodes for this setup.

---

# Why Are Modified Nodes Required?

Kandinsky 6 contains FP32 Linear operations that normally use ComfyUI's standard weight-casting path.

GGUF weights are stored in packed quantized form. They must be dequantized before these Kandinsky FP32 operations are executed.

`Kandinsky6_GGUF` adds GGUF-aware handling so packed weights are correctly passed through the GGUF dequantization path before reaching Kandinsky's FP32 Linear operations.

`ComfyUI-GGUF-Rebel` provides the corresponding GGUF loader compatibility for newer ComfyUI Linear operations, including support for:

```text
input_act
act_weight
act_eps
residual
residual_scale
```

Both pieces are currently required for the Kandinsky GGUF releases.

---

# Requirements

- **ComfyUI 0.38.0+**
- **Python 3.10+**
- `Kandinsky6_GGUF`
- `ComfyUI-GGUF-Rebel`
- Required Kandinsky text encoders, VAEs, audio models, and vocoder

Model loading and offloading are handled through ComfyUI.

The standalone Diffusers inference pipeline is **not required**.

No manual ComfyUI core modifications are required when using the two repositories above.

---

# Model Releases

The matching quantized models are available here:

https://huggingface.co/realrebelai/Kandinsky-6.0-Pro-5s_GGUFs

The repository contains GGUF and W4A8 conversions of:

- **Kandinsky 6.0 Pro 5s**
- **Kandinsky 6.0 Pro Distilled 5s**

For **W4A8**, use the official Kandinsky nodes.

For **GGUF**, use this repository together with `ComfyUI-GGUF-Rebel`.

## Models

Open a bundled workflow and click **Download models** in its setup note,
or choose **Kandinsky 6 → Kandinsky 6 — Download models** from the top menu.

The button downloads Pro distill 5s, Qwen3.5-9B (~19.3 GB), Qwen2.5, CLIP, the video/audio VAEs,
BigVGAN and VSR, including all required JSON configs. Files are placed in
ComfyUI's model folders automatically; existing files, including configured
extra model paths, are reused. Nothing is downloaded during extension install
or startup. Allow enough disk space for large HF weights.

For gated/private models, obtain access and run `hf auth login` on the ComfyUI
server first.

### Manual downloads

Download the files separately if you prefer not to use the button. Destinations
below are relative to your **ComfyUI folder**; create missing directories.

| Download | Destination |
| --- | --- |
| [Pro distill 5s — default transformer](https://huggingface.co/kandinskylab/Kandinsky-6.0-Pro-distill-5s-Diffusers/resolve/main/transformer/diffusion_pytorch_model.safetensors) | `models/diffusion_models/Kandinsky-6.0-Pro-distill-5s-Diffusers/transformer/diffusion_pytorch_model.safetensors` |
| [Qwen3.5-9B — beautifier](https://huggingface.co/Comfy-Org/Qwen3.5/resolve/main/text_encoders/qwen3.5_9b_bf16.safetensors) | `models/text_encoders/qwen3.5_9b_bf16.safetensors` |
| [Qwen2.5 — text encoder](https://huggingface.co/Comfy-Org/Qwen-Image_ComfyUI/resolve/main/split_files/text_encoders/qwen_2.5_vl_7b.safetensors) | `models/text_encoders/qwen_2.5_vl_7b.safetensors` |
| [CLIP-L — text encoder](https://huggingface.co/Comfy-Org/HunyuanVideo_repackaged/resolve/main/split_files/text_encoders/clip_l.safetensors) | `models/text_encoders/clip_l.safetensors` |
| [HunyuanVideo VAE](https://huggingface.co/Comfy-Org/HunyuanVideo_repackaged/resolve/main/split_files/vae/hunyuan_video_vae_bf16.safetensors) | `models/vae/hunyuan_video_vae_bf16.safetensors` |
| [v1-44.pth — audio VAE](https://huggingface.co/hkchengrex/MMAudio/resolve/main/ext_weights/v1-44.pth) | `models/audio_vae/v1-44.pth` |
| [bigvgan_generator.pt — vocoder](https://huggingface.co/nvidia/bigvgan_v2_44khz_128band_512x/resolve/main/bigvgan_generator.pt) | `models/audio_vae/bigvgan_vocoder/bigvgan_generator.pt` |
| [config.json — vocoder config](https://huggingface.co/nvidia/bigvgan_v2_44khz_128band_512x/resolve/main/config.json) | `models/audio_vae/bigvgan_vocoder/config.json` |

Both bundled workflows also need the files in the
[SR manual-download table](https://github.com/kandinskylab/kandinsky-6-sr/blob/main/comfyui/README.md#manual-downloads).
Keep all listed JSON configs in their specified folders.

For non-distilled Pro, use [this transformer instead](https://huggingface.co/kandinskylab/Kandinsky-6.0-Pro-5s-Diffusers/resolve/main/transformer/diffusion_pytorch_model.safetensors),
saved as `models/diffusion_models/Kandinsky-6.0-Pro-5s-Diffusers/transformer/diffusion_pytorch_model.safetensors`;
see **Run** below for its sampling settings. HF access may be required for this
non-distilled checkpoint. You do not need both transformers.

Models on another drive can use ComfyUI's configured extra model paths.
For audio, the simplest option is a symbolic link (directory junction on Windows)
from `models/audio_vae` to your audio-model folder on that drive, keeping the
same layout above; BigVGAN is loaded from that folder. Restart ComfyUI after
placing the files and select them in the loader nodes. The download button is
not required for manual installation.

## Run

For faster inference, try SageAttention: launch ComfyUI with `--use-sage-attention` instead of `--use-flash-attention` (requires `sageattention` in ComfyUI's Python environment and a supported NVIDIA GPU).

Open **Workflow → Browse Templates → kandinsky6** and choose:

- **Kandinsky 6.0 Text to Video+Audio**
- **Kandinsky 6.0 Image to Video+Audio**

Select the downloaded models and run the workflow to save an MP4 with audio.
Edit the video/audio captions in **Beautify Prompt**; it expands them automatically.
For I2VA it also sees the original **Load Image** reference. The result appears in
**Preview as Text**. Set `enabled=false` to use your captions directly.
Put exact spoken lines in the video caption as `<S>Look there!<E>`; describe the
voice and other sounds in the audio caption. Thinking and MTP are off; no vLLM,
Transformers model or separate LLM server is needed. Qwen3.5 is managed by ComfyUI
and is separate from the Qwen2.5 text encoder required by Kandinsky.
Before your first generation, use **Download models** or the manual tables above,
including the companion JSON configs and audio files.
Use `weight_dtype=default` in **Load Diffusion Model** for the first run.
I2VA includes a portrait; replace it in **Load Image** to use your own image.

**Kandinsky 6 Sampler** selects PiFlow automatically for distilled Pro; use
**10 steps, CFG=1, denoise=1** and audio VAE **scaling=0.417**. Its sampler/scheduler
selectors apply only to non-distilled models. **MagCache automatically bypasses
distilled models**, even if its node is connected.
For [non-distilled Pro](https://huggingface.co/kandinskylab/Kandinsky-6.0-Pro-5s-Diffusers),
select its transformer and use **50 steps, CFG=5, audio scaling=0.5302**.
Set MagCache's `steps` to the sampler's step count; it is calibrated for
**non-distilled Pro only**, not Lite or VSR. Both templates include VSR.
The default VSR scale is **2.25x**. For **4x**, select `4x` in both the VSR node
and the latent-upscaler loader; **2x/2.25x** use the loader's `2x` entry.
VSR's `use_nabla` defaults to **off**, using ComfyUI's selected attention backend
without NABLA warmup. Enable it to compare sparse NABLA attention; its first run
compiles kernels. The models themselves are not compiled.

For standalone video upscaling, use the
[SR extension and template](https://github.com/kandinskylab/kandinsky-6-sr/tree/main/comfyui).

## Compatibility

If native Qwen fails because `comfy-kitchen` requires a newer NVIDIA driver,
update the driver or use its official Python-only wheel. For ComfyUI **0.38.0**
with `comfy-kitchen==0.2.36`, this was tested on H100 with driver 570 and Torch 2.10.
Run with **ComfyUI's Python**, then restart:

```bash
python -m pip install --force-reinstall --no-deps \
  https://files.pythonhosted.org/packages/38/23/a6787aac01d7c28ae3cb07579ba839297a35e6fad66baac096916246cc7f/comfy_kitchen-0.2.36-py3-none-any.whl
```

Inference stays on GPU. For another ComfyUI version, match its own
`comfy-kitchen` requirement rather than forcing this pin.
