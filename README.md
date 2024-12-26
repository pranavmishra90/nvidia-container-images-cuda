# Nvidia CUDA Linux Container Image Sources

Usage of the CUDA container images requires the [Nvidia Container Toolkit](https://github.com/NVIDIA/nvidia-container-toolkit).

Container images are available from:

- https://ngc.nvidia.com/catalog/containers/nvidia:cuda
- https://hub.docker.com/r/nvidia/cuda

## Upstream Repository

Nvidia Container Images on GitLab

https://gitlab.com/nvidia/container-images/cuda

## Building from source

The container image scripts are archived in the `dist/` directory and are available for all supported distros and cuda versions.

Here is an example on how to build an multi-arch container image for Ubuntu `24.04` and CUDA `12.6.3`:

WARNING: cudgl image builds *REQUIRE* a secure registry to push built intermediate images to since buildkit does not easily allow using local image references from the build container.

```bash
# Create the docker buildkit builder

docker buildx create --use --platform linux/x86_64 --config=cuda-buildkit-config.toml --driver-opt image=moby/buildkit:v0.18.1 --name cuda --node cuda
```

```bash
./build.sh -d --image-name pranavmishra90/cuda --cuda-version 12.6.3 --os ubuntu --os-version 24.04 --arch x86_64 --push
```

See `./build.sh --help` for usage.