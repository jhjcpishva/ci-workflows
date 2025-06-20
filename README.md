# ci-workflows

This repository contains reusable GitHub Actions workflows.  
Currently, it provides a shared workflow for building and pushing Docker images.

## Usage

In your target repository, add the following workflow:

```yaml
# .github/workflows/docker-build.yml
name: Docker build

on:
  push:
    branches: [main]
    tags: ['*']
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  docker:
    uses: jhjcpishva/ci-workflows/.github/workflows/docker-build.yml@main
    with:
      platforms: linux/amd64,linux/arm64
    secrets:
      GHCR_TOKEN: ${{ secrets.GHCR_TOKEN }}
```

Replace jhjcpishva with your actual GitHub organization or username.

## Required Secrets

- `GHCR_TOKEN`: A personal access token (PAT) or `GITHUB_TOKEN` with permissions to push images to GitHub Container Registry (`ghcr.io`)

## Notes

- The platforms input is optional; defaults to `linux/amd64`, `linux/arm64`
- This workflow automatically tags the image as `latest` or with the pushed tag name (e.g. `v1.2.3`)
- Pull requests only run the build (not push)
