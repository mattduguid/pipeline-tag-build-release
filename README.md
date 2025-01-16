# pipeline-tag-build-release

## status

[![Pipeline Main](https://github.com/mattduguid/pipeline-tag-build-release/actions/workflows/pipeline.yml/badge.svg)](https://github.com/mattduguid/pipeline-tag-build-release/actions/workflows/pipeline.yml)

## description

example pipeline to release a container to cloud with infrastructure provisioned with terraform

- workflow: tag
  - generate a unique tag for container tag, terraform, etc
- workflow: build
  - build container and push to container registry
- workflow: release
  - release infrastructure as code using terraform and pull container from container registry

## UI

### workflow run and runs

https://github.com/mattduguid/pipeline-tag-build-release/actions/workflows/pipeline.yml

## API

### workflow run
- verb/url
  - POST https://api.github.com/repos/mattduguid/pipeline-tag-build-release/actions/workflows/pipeline.yml/dispatches
- headers
  - Accept: application/vnd.github+json
  - Authorization: token <PAT_TOKEN>
- body
```json
{
  "ref": "main",
  "inputs": {}
}
```

### workflow runs

- verb/url
  - GET https://api.github.com/repos/mattduguid/pipeline-tag-build-release/actions/runs
- headers
  - Accept: application/vnd.github+json
  - Authorization: token <PAT_TOKEN>
