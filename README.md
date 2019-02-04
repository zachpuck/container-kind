# container-kind

`container-kind` creates a container `quay.io/samsung_cnct/kind` which contains 
the tools needed to mimic a lightweight CMC. This lightweight CMC is intended
to be used for CMA testing (e.g. cma-aks, cma-aws, cma-ssh, etc.).

# Usage

For example usage see the `pipeline.yaml` file in one of the `cma-*` 
repositories.

# Development

To use this image for development purposes it must be run as a privilege 
container, e.g.

```
docker run -it --rm --privileged quay.io/samsung_cnct/kind /bin/bash
service docker start
kind create cluster --retain --wait=1m --loglevel=debug
...
```
