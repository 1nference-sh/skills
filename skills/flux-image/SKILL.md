---
name: flux-image
description: |
  Generate images with FLUX models (Black Forest Labs) via inference.sh CLI.
  Models: FLUX.2 Dev, FLUX.2 Klein, FLUX.2 Dev Turbo, FLUX Dev LoRA, FLUX Kontext, FLUX Fill, FLUX ControlNet.
  Capabilities: text-to-image, image editing, inpainting, ControlNet, LoRA fine-tuning.
  Triggers: flux, flux.2, flux dev, flux schnell, flux pro, black forest labs,
  flux image, flux ai, flux model, flux lora, flux kontext, flux inpainting
allowed-tools: Bash(infsh *)
---

# FLUX Image Generation

Generate images with FLUX models via inference.sh CLI.

## Quick Start

```bash
curl -fsSL https://cli.inference.sh | sh && infsh login

infsh app run falai/flux-2-dev --input '{"prompt": "a futuristic city at night"}'
```

## FLUX Models

| Model | App ID | Speed | Quality | Use Case |
|-------|--------|-------|---------|----------|
| FLUX.2 Dev | `falai/flux-2-dev` | Medium | Highest | Production images |
| FLUX.2 Klein | `infsh/flux-2-klein` | Fastest | Good | Previews, iteration |
| FLUX.2 Dev Turbo | `infsh/flux-2-dev-turbo` | Fast | High | Balance |
| FLUX Dev LoRA | `falai/flux-dev-lora` | Medium | High | Custom styles |
| FLUX Kontext | `infsh/flux-1-kontext-dev` | Medium | High | Image editing |
| FLUX Fill | `infsh/flux-1-fill` | Medium | High | Inpainting |
| FLUX ControlNet | `infsh/flux-1-dev-controlnet-union` | Medium | High | Controlled generation |
| FLUX Upscaler | `infsh/flux-1-dev-upscaler` | Medium | High | Upscaling |

## Search FLUX Apps

```bash
infsh app list --search "flux"
```

## Examples

### High-Quality Generation (FLUX.2 Dev)

```bash
infsh app run falai/flux-2-dev --input '{
  "prompt": "professional product photo of headphones, studio lighting, white background",
  "num_images": 1,
  "image_size": "square_hd"
}'
```

### Fast Generation (FLUX.2 Klein)

```bash
infsh app run infsh/flux-2-klein --input '{"prompt": "abstract art, colorful"}'
```

### Image Editing (FLUX Kontext)

```bash
infsh app sample infsh/flux-1-kontext-dev --save input.json

# {
#   "prompt": "change the background to a beach",
#   "image_url": "https://your-image.jpg"
# }

infsh app run infsh/flux-1-kontext-dev --input input.json
```

### Inpainting (FLUX Fill)

```bash
infsh app sample infsh/flux-1-fill --save input.json

# {
#   "prompt": "a red sports car",
#   "image_url": "https://original.jpg",
#   "mask_url": "https://mask.png"
# }

infsh app run infsh/flux-1-fill --input input.json
```

### LoRA Custom Styles

```bash
infsh app sample falai/flux-dev-lora --save input.json

# Add your LoRA weights URL
infsh app run falai/flux-dev-lora --input input.json
```

### ControlNet

```bash
infsh app sample infsh/flux-1-dev-controlnet-union --save input.json

# {
#   "prompt": "a detailed portrait",
#   "control_image_url": "https://pose-reference.jpg"
# }

infsh app run infsh/flux-1-dev-controlnet-union --input input.json
```

### Upscaling

```bash
infsh app run infsh/flux-1-dev-upscaler --input '{"image_url": "https://..."}'
```

## Image Sizes

- `square` - 512x512
- `square_hd` - 1024x1024
- `portrait_4_3` - 768x1024
- `portrait_16_9` - 576x1024
- `landscape_4_3` - 1024x768
- `landscape_16_9` - 1024x576

## Related Skills

```bash
# Full platform skill (all 100+ apps)
npx skills add inference-sh/skills@inference-sh

# All image generation models
npx skills add inference-sh/skills@ai-image-generation

# Upscaling
npx skills add inference-sh/skills@image-upscaling

# Background removal
npx skills add inference-sh/skills@background-removal
```

Browse all apps: `infsh app list`
