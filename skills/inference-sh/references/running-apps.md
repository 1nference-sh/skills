# Running Apps

## Basic Run

```bash
infsh app run user/app-name --input input.json
```

## Inline JSON

```bash
infsh app run falai/flux-2-dev --input '{"prompt": "a sunset over mountains"}'
```

## Version Pinning

```bash
infsh app run user/app-name@1.0.0 --input input.json
```

## Generate Sample Input

Before running, generate a sample input file:

```bash
infsh app sample falai/flux-2-dev
```

Save to file:

```bash
infsh app sample falai/flux-2-dev --save input.json
```

Then edit `input.json` and run:

```bash
infsh app run falai/flux-2-dev --input input.json
```

## Workflow Example

### Image Generation with FLUX

```bash
# 1. Get app details
infsh app get falai/flux-2-dev

# 2. Generate sample input
infsh app sample falai/flux-2-dev --save input.json

# 3. Edit input.json
# {
#   "prompt": "a cat astronaut floating in space",
#   "num_images": 1,
#   "image_size": "landscape_16_9"
# }

# 4. Run
infsh app run falai/flux-2-dev --input input.json
```

### Video Generation with Veo

```bash
# 1. Generate sample
infsh app sample google/veo-3-1-fast --save input.json

# 2. Edit prompt
# {
#   "prompt": "A drone shot flying over a forest at sunset"
# }

# 3. Run
infsh app run google/veo-3-1-fast --input input.json
```

### Text-to-Speech

```bash
# Quick inline run
infsh app run infsh/kokoro-tts --input '{"text": "Hello, this is a test."}'
```

## Output

The CLI returns the app output directly. For file outputs (images, videos, audio), you'll receive URLs to download.

Example output:

```json
{
  "images": [
    {
      "url": "https://cloud.inference.sh/...",
      "content_type": "image/png"
    }
  ]
}
```

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| "invalid input" | Schema mismatch | Check `infsh app get` for required fields |
| "app not found" | Wrong app name | Check `infsh app list --search` |
| "quota exceeded" | Out of credits | Check account balance |
