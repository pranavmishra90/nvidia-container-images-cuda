# Nvidia CUDA Linux Container Image Sources

Usage of the CUDA container images requires the [Nvidia Container Toolkit](https://github.com/NVIDIA/nvidia-container-toolkit).

Container images are available from:

- https://ngc.nvidia.com/catalog/containers/nvidia:cuda
- https://hub.docker.com/r/nvidia/cuda

## Building from source

The container image scripts are archived in the `dist/` directory and are available for all supported distros and cuda versions.

Here is an example on how to build an multi-arch container image for Ubuntu `22.04` and CUDA `12.6.1`:

WARNING: cudgl image builds *REQUIRE* a secure registry to push built intermediate images to since buildkit does not easily allow using local image references from the build container.

```bash
./build.sh -d --image-name pranavmishra90/cuda --cuda-version 12.6.1 --os ubuntu --os-version 22.04 --arch x86_64 --push
```

See `./build.sh --help` for usage.