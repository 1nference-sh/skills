---
name: background-removal
description: |
  Remove backgrounds from images with BiRefNet via inference.sh CLI.
  Model: BiRefNet (high accuracy background removal).
  Use for: product photos, portraits, e-commerce, transparent PNGs, photo editing.
  Triggers: remove background, background removal, remove bg, transparent background,
  cut out image, background remover, rembg, product photo editing, cutout,
  transparent png, bg removal, photo cutout
allowed-tools: Bash(infsh *)
---

# Background Removal

Remove backgrounds from images via inference.sh CLI.

## Quick Start

```bash
curl -fsSL https://cli.inference.sh | sh && infsh login

infsh app run infsh/birefnet --input '{"image_url": "https://your-photo.jpg"}'
```

## Available Model

| Model | App ID | Best For |
|-------|--------|----------|
| BiRefNet | `infsh/birefnet` | High accuracy, any subject |

## Examples

### Remove Background

```bash
infsh app run infsh/birefnet --input '{"image_url": "https://portrait.jpg"}'
```

### Product Photo

```bash
infsh app run infsh/birefnet --input '{"image_url": "https://product-on-table.jpg"}'
```

### From Local File (upload first)

```bash
# Get the app details to see upload instructions
infsh app get infsh/birefnet

# Run with the uploaded URL
infsh app run infsh/birefnet --input '{"image_url": "https://uploaded-url.jpg"}'
```

## Workflow: Generate and Remove Background

```bash
# 1. Generate an image
infsh app run falai/flux-2-dev --input '{"prompt": "a cute robot mascot"}' > robot.json

# 2. Remove background
infsh app run infsh/birefnet --input '{"image_url": "<url-from-step-1>"}'
```

## Workflow: Remove BG and Composite

```bash
# 1. Remove background from subject
infsh app run infsh/birefnet --input '{"image_url": "https://person.jpg"}' > cutout.json

# 2. Use FLUX inpainting to place on new background
infsh app run infsh/flux-1-fill --input '{
  "prompt": "person standing on a beach",
  "image_url": "<cutout-url>",
  "mask_url": "<mask-for-background>"
}'
```

## Use Cases

- **E-commerce**: Clean product photos
- **Portraits**: Professional headshots
- **Marketing**: Assets for design
- **Social Media**: Profile pictures
- **Design**: Elements for compositions

## Output

Returns a PNG with transparent background.

## Related Skills

```bash
# Full platform skill (all 100+ apps)
npx skills add inference-sh/skills@inference-sh

# Image generation
npx skills add inference-sh/skills@ai-image-generation

# FLUX models (including inpainting)
npx skills add inference-sh/skills@flux-image

# Upscaling
npx skills add inference-sh/skills@image-upscaling
```

Browse all image apps: `infsh app list --category image`
