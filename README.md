# Hugo Deployment Reusable Workflows
Reusable GitHub Actions workflows for deploying Hugo websites.

## Cloudflare Pages
Example workflow:

```yaml
name: Deploy to Cloudflare Pages
on:
  push:
    paths: [assets/**, content/**, layouts/**, static/**, themes/**, config.toml]
  pull_request_target:
    paths: [assets/**, content/**, layouts/**, static/**, themes/**, config.toml]
  workflow_dispatch:
  
concurrency:
  group: ${{ github.workflow }}
  
jobs:
  deploy:
    uses: AnimMouse/Hugo-Deploy-Workflow/.github/workflows/cloudflare-pages.yaml@v1
    with:
      project_name: ${{ vars.CLOUDFLARE_PROJECT_NAME }}
      account_id: ${{ vars.CLOUDFLARE_ACCOUNT_ID }}
      branch: ${{ github.head_ref || 'main' }}
      ref: ${{ github.event.pull_request.head.sha }}
      cache_resources: true
    secrets:
      api_token: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```