[![workflow Main](https://github.com/mattduguid/workflow-tag-build-release/actions/workflows/workflow.yml/badge.svg)](https://github.com/mattduguid/workflow-tag-build-release/actions/workflows/workflow.yml)

# workflow-tag-build-release

## description

example workflow to release a container to cloud with infrastructure provisioned with terraform

- workflow: tag
  - generate a unique tag for container tag, terraform, etc
- workflow: build
  - build container and push to container registry
- workflow: release
  - release infrastructure as code using terraform and pull container from container registry

## UI

### workflow run and runs

https://github.com/mattduguid/workflow-tag-build-release/actions/workflows/workflow.yml

## API

### workflow run execute
- verb/url
  - POST https://api.github.com/repos/mattduguid/workflow-tag-build-release/actions/workflows/workflow.yml/dispatches
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

### workflow runs view

- verb/url
  - GET https://api.github.com/repos/mattduguid/workflow-tag-build-release/actions/runs
- headers
  - Accept: application/vnd.github+json
  - Authorization: token <PAT_TOKEN>

### workflow run delete

- verb/url
  - DELETE https://api.github.com/repos/mattduguid/workflow-tag-build-release/actions/runs/{run_id}
- headers
  - Accept: application/vnd.github+json
  - Authorization: token <PAT_TOKEN>
