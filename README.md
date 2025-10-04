# perekine-builder

This repository orchestrates the build process for `perekine-service`. It listens for changes in the service repository and executes builds using configuration from `perekine-config`.

## Architecture Overview

```
┌─────────────────┐         ┌──────────────────┐         ┌─────────────────┐
│ perekine-service│         │ perekine-builder │         │ perekine-config │
│                 │         │                  │         │                 │
│  Source Code    │─trigger─▶│  Build Process   │─reads──▶│  Configuration  │
│  Changes        │         │                  │         │  Files          │
└─────────────────┘         └──────────────────┘         └─────────────────┘
```

## How It Works

1. **Trigger**: When code is pushed to `perekine-service`, it sends a `repository_dispatch` event to this repo
2. **Checkout**: This repo's workflow checks out:
   - The builder scripts (this repository)
   - The service code at the specific commit
   - The configuration files from `perekine-config`
3. **Build**: Executes the build process using the service code and config
4. **Artifacts**: Produces build artifacts and reports status

## Workflow File

The main workflow is defined in `.github/workflows/build.yml` and:
- Listens for `service-change` repository dispatch events
- Checks out all three repositories
- Reads build configuration
- Executes the build process
- Uploads artifacts

## Build Process

The build process can be customized in the workflow file. By default, it:
1. Displays build information
2. Reads configuration from `perekine-config`
3. Executes build commands on the service code
4. Creates and uploads build artifacts

## Testing

You can manually trigger a build using GitHub's workflow dispatch:
1. Go to Actions tab
2. Select "Build Service" workflow
3. Click "Run workflow"
4. Enter service repository and commit SHA
5. Click "Run workflow"

## Configuration

Build behavior is controlled by files in `perekine-config`:
- `build-config.yml` - Main build configuration
- Other config files as needed
