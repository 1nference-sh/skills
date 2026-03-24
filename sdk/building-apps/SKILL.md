---
name: building-inferencesh-apps
description: "Build and deploy applications on inference.sh. Use when getting started, understanding the platform, creating apps, configuring resources, or needing an overview of inference.sh app development. Supports both Python and Node.js. Triggers: inference.sh app, infsh app, inf.yml, inference.py, inference.js, deploy app, app development, build app, create app, GPU app, VRAM, app resources, app secrets, app integrations, multi-function app"
---

# Inference.sh App Development

Build and deploy applications on the inference.sh platform. Apps can be written in **Python** or **Node.js**.

## CLI Installation

```bash
curl -fsSL https://cli.inference.sh | sh
```

```bash
infsh update   # Update CLI
infsh login    # Authenticate
infsh me       # Check current user
```

## Quick Start

Always use `infsh app init` to create new apps — it scaffolds the correct project structure, `inf.yml`, and boilerplate code. Don't create these files by hand; the CLI ensures everything is wired up correctly and avoids common mistakes like missing `"type": "module"` in `package.json` or incorrect kernel names in `inf.yml`.

```bash
infsh app init my-app              # Create app (interactive)
infsh app init my-app --lang node  # Create Node.js app
```

## Development Workflow

The standard cycle is: **init → test locally → deploy → run in cloud**.

### 1. Test Locally

Run your app on your own machine before deploying. This catches most issues early.

```bash
infsh app test                     # Uses input.json in the app directory
infsh app test --input '{"prompt": "hello"}'  # Inline JSON
infsh app test --save-example      # Generate a sample input.json from your schema
```

### 2. Deploy

Push your app to the cloud. Use `--dry-run` first to validate configuration without actually deploying.

```bash
infsh app deploy --dry-run         # Validate without deploying
infsh app deploy                   # Deploy for real
```

### 3. Run in Cloud

After deploying, test the live version using `infsh app run`:

```bash
infsh app run user/app --input input.json
infsh app run user/app@version --input '{"prompt": "hello"}'

# Generate sample input for any deployed app
infsh app sample user/app
infsh app sample user/app --save input.json
```

## App Structure

### Python

```python
from inferencesh import BaseApp, BaseAppInput, BaseAppOutput
from pydantic import Field

class AppSetup(BaseAppInput):
    """Setup parameters — triggers re-init when changed"""
    model_id: str = Field(default="gpt2", description="Model to load")

class AppInput(BaseAppInput):
    prompt: str = Field(description="Input prompt")

class AppOutput(BaseAppOutput):
    result: str = Field(description="Output result")

class App(BaseApp):
    async def setup(self, config: AppSetup):
        """Runs once when worker starts or config changes"""
        self.model = load_model(config.model_id)

    async def run(self, input_data: AppInput) -> AppOutput:
        """Default function — runs for each request"""
        return AppOutput(result="done")

    async def unload(self):
        """Cleanup on shutdown"""
        pass

    async def on_cancel(self):
        """Called when user cancels — for long-running tasks"""
        return True
```

### Node.js

```javascript
import { z } from "zod";

export const AppSetup = z.object({
  modelId: z.string().default("gpt2").describe("Model to load"),
});

export const RunInput = z.object({
  prompt: z.string().describe("Input prompt"),
});

export const RunOutput = z.object({
  result: z.string().describe("Output result"),
});

export class App {
  async setup(config) {
    /** Runs once when worker starts or config changes */
    this.model = loadModel(config.modelId);
  }

  async run(inputData) {
    /** Default function — runs for each request */
    return { result: "done" };
  }

  async unload() {
    /** Cleanup on shutdown */
  }

  async onCancel() {
    /** Called when user cancels — for long-running tasks */
    return true;
  }
}
```

## Multi-Function Apps

Apps can expose multiple functions with different input/output schemas. Functions are auto-discovered.

**Python:** Add methods with type-hinted Pydantic input/output models.
**Node.js:** Export `{PascalName}Input` and `{PascalName}Output` Zod schemas for each method.

Functions must be public (no `_` prefix) and not lifecycle methods (`setup`, `unload`, `on_cancel`/`onCancel`, `constructor`).

Call via API with `"function": "method_name"` in the request body. Set `default_function` in `inf.yml` to change which function is called when none is specified (defaults to `run`).

## Configuring Resources (inf.yml)

### Project Structure

**Python:**
```
my-app/
├── inf.yml           # Configuration
├── inference.py      # App logic
├── requirements.txt  # Python packages (pip)
└── packages.txt      # System packages (apt) — optional
```

**Node.js:**
```
my-app/
├── inf.yml           # Configuration
├── src/
│   └── inference.js  # App logic
├── package.json      # Node.js packages (npm/pnpm)
└── packages.txt      # System packages (apt) — optional
```

### inf.yml

```yaml
name: my-app
description: What my app does
category: image
kernel: python-3.11     # or node-22

# For multi-function apps (default: run)
# default_function: generate

resources:
  gpu:
    count: 1
    vram: 24    # 24GB (auto-converted)
    type: any
  ram: 32       # 32GB

env:
  MODEL_NAME: gpt-4

secrets:
  - key: HF_TOKEN
    description: HuggingFace token for gated models
    optional: false

integrations:
  - key: google.sheets
    description: Access to Google Sheets
    optional: true
```

### Resource Units

CLI auto-converts human-friendly values:
- **< 1000** → GB (e.g., `80` = 80GB)
- **1000 to 1B** → MB

### GPU Types

`any` | `nvidia` | `amd` | `apple` | `none`

> **Note:** Currently only NVIDIA CUDA GPUs are supported.

### Categories

`image` | `video` | `audio` | `text` | `chat` | `3d` | `other`

### CPU-Only Apps

```yaml
resources:
  gpu:
    count: 0
    type: none
  ram: 4
```

### Dependencies

**Python** — `requirements.txt`:
```
torch>=2.0
transformers
accelerate
```

**Node.js** — `package.json`:
```json
{
  "type": "module",
  "dependencies": {
    "zod": "^3.23.0",
    "sharp": "^0.33.0"
  }
}
```

**System packages** — `packages.txt` (apt-installable):
```
ffmpeg
libgl1-mesa-glx
```

### Base Images

| Type | Image |
|------|-------|
| GPU | `docker.inference.sh/gpu:latest-cuda` |
| CPU | `docker.inference.sh/cpu:latest` |

## Reference Files

Load the appropriate reference file based on the language and topic:

### App Logic & Schemas
- [references/python-app-logic.md](references/python-app-logic.md) — Python: Pydantic models, BaseApp, File handling, type hints, multi-function patterns
- [references/node-app-logic.md](references/node-app-logic.md) — Node.js: Zod schemas, File handling, ESM, generators, multi-function patterns

### Debugging, Optimization & Cancellation
- [references/python-patterns.md](references/python-patterns.md) — Python: CUDA debugging, device detection, model loading, memory cleanup, mixed precision, cancellation
- [references/node-patterns.md](references/node-patterns.md) — Node.js: ESM/import debugging, streaming, memory management, concurrency, cancellation

### Secrets & OAuth
- [references/python-secrets-oauth.md](references/python-secrets-oauth.md) — Python: os.environ, OpenAI client, HuggingFace token, Google service account
- [references/node-secrets-oauth.md](references/node-secrets-oauth.md) — Node.js: process.env, OpenAI client, Google credentials JSON

### Usage Tracking
- [references/python-tracking.md](references/python-tracking.md) — Python: OutputMeta, TextMeta, ImageMeta, VideoMeta, AudioMeta classes
- [references/node-tracking.md](references/node-tracking.md) — Node.js: textMeta, imageMeta, videoMeta, audioMeta factory functions

### CLI
- [references/cli.md](references/cli.md) — Full CLI command reference, prerequisites for both languages

## Resources

- **Full Docs**: [inference.sh/docs](https://inference.sh/docs)
- **Examples**: [github.com/inference-sh/grid](https://github.com/inference-sh/grid)
