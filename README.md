# pipeline-tag-build-release

example pipeline to release a container to cloud with infrastructure provisioned with terraform

## stage: tag

- generate a unique tag for container tag, terraform, etc

## stage: build

- build container and push to container registry

## stage release

- release infrastructure as code terraform and pull container from container registry
