---
name: image-upscaling
description: |
  Upscale and enhance images with Real-ESRGAN, Thera, Topaz, FLUX Upscaler via inference.sh CLI.
  Models: Real-ESRGAN, Thera (any size), FLUX Dev Upscaler, Topaz Image Upscaler.
  Use for: enhance low-res images, upscale AI art, restore old photos, increase resolution.
  Triggers: upscale image, image upscaler, enhance image, increase resolution,
  real esrgan, ai upscale, super resolution, image enhancement, upscaling,
  enlarge image, higher resolution, 4k upscale, hd upscale
allowed-tools: Bash(infsh *)
---

# Image Upscaling

Upscale and enhance images via inference.sh CLI.

## Quick Start

```bash
curl -fsSL https://cli.inference.sh | sh && infsh login

infsh app run infsh/real-esrgan --input '{"image_url": "https://your-image.jpg"}'
```

## Available Upscalers

| Model | App ID | Best For |
|-------|--------|----------|
| Real-ESRGAN | `infsh/real-esrgan` | General upscaling, photos |
| Thera | `infsh/thera` | Any size, flexible |
| FLUX Upscaler | `infsh/flux-1-dev-upscaler` | AI art upscaling |
| Topaz | `falai/topaz-image-upscaler` | Professional quality |

## Search Upscaling Apps

```bash
infsh app list --search "upscal"
infsh app list --search "esrgan"
```

## Examples

### Real-ESRGAN (General Purpose)

```bash
infsh app run infsh/real-esrgan --input '{"image_url": "https://low-res-image.jpg"}'
```

### Thera (Flexible Upscaling)

```bash
infsh app sample infsh/thera --save input.json

# {
#   "image_url": "https://your-image.jpg",
#   "scale": 4
# }

infsh app run infsh/thera --input input.json
```

### FLUX Upscaler (AI Art)

```bash
infsh app run infsh/flux-1-dev-upscaler --input '{"image_url": "https://ai-generated.jpg"}'
```

### Topaz (Professional)

```bash
infsh app run falai/topaz-image-upscaler --input '{"image_url": "https://photo.jpg"}'
```

## Workflow: Generate and Upscale

```bash
# 1. Generate image with FLUX Klein (fast, lower res)
infsh app run infsh/flux-2-klein --input '{"prompt": "landscape painting"}' > image.json

# 2. Upscale the result
infsh app run infsh/real-esrgan --input '{"image_url": "<url-from-step-1>"}'
```

## Use Cases

- **AI Art**: Upscale generated images for print
- **Old Photos**: Restore and enhance resolution
- **Web Images**: Prepare for high-DPI displays
- **Print**: Increase resolution for large prints
- **Thumbnails**: Create high-res versions

## Related Skills

```bash
# Full platform skill (all 100+ apps)
npx skills add inference-sh/skills@inference-sh

# Image generation (generate then upscale)
npx skills add inference-sh/skills@ai-image-generation

# FLUX models
npx skills add inference-sh/skills@flux-image

# Background removal
npx skills add inference-sh/skills@background-removal
```

Browse all image apps: `infsh app list --category image`
