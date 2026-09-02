ARG IMAGE_NAME
FROM ${IMAGE_NAME}:13.2.1-runtime-ubuntu24.04 AS base

ENV NV_CUDA_LIB_VERSION="13.2.1-1"

FROM base AS base-amd64

ENV NV_CUDA_CUDART_DEV_VERSION=13.2.75-1
ENV NV_NVML_DEV_VERSION=13.2.82-1
ENV NV_LIBCUSPARSE_DEV_VERSION=12.7.10.1-1
ENV NV_LIBNPP_DEV_VERSION=13.1.0.48-1
ENV NV_LIBNPP_DEV_PACKAGE=libnpp-dev-13-2=${NV_LIBNPP_DEV_VERSION}

ENV NV_LIBCUBLAS_DEV_VERSION=13.4.0.1-1
ENV NV_LIBCUBLAS_DEV_PACKAGE_NAME=libcublas-dev-13-2
ENV NV_LIBCUBLAS_DEV_PACKAGE=${NV_LIBCUBLAS_DEV_PACKAGE_NAME}=${NV_LIBCUBLAS_DEV_VERSION}

ENV NV_CUDA_NSIGHT_COMPUTE_VERSION=13.2.1-1
ENV NV_CUDA_NSIGHT_COMPUTE_DEV_PACKAGE=cuda-nsight-compute-13-2=${NV_CUDA_NSIGHT_COMPUTE_VERSION}
FROM base AS base-arm64

ENV NV_CUDA_CUDART_DEV_VERSION=13.2.75-1
ENV NV_NVML_DEV_VERSION=13.2.82-1
ENV NV_LIBCUSPARSE_DEV_VERSION=12.7.10.1-1
ENV NV_LIBNPP_DEV_VERSION=13.1.0.48-1
ENV NV_LIBNPP_DEV_PACKAGE=libnpp-dev-13-2=${NV_LIBNPP_DEV_VERSION}

ENV NV_LIBCUBLAS_DEV_PACKAGE_NAME=libcublas-dev-13-2
ENV NV_LIBCUBLAS_DEV_VERSION=13.4.0.1-1
ENV NV_LIBCUBLAS_DEV_PACKAGE=${NV_LIBCUBLAS_DEV_PACKAGE_NAME}=${NV_LIBCUBLAS_DEV_VERSION}

ENV NV_CUDA_NSIGHT_COMPUTE_VERSION=13.2.1-1
ENV NV_CUDA_NSIGHT_COMPUTE_DEV_PACKAGE=cuda-nsight-compute-13-2=${NV_CUDA_NSIGHT_COMPUTE_VERSION}

FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer="NVIDIA CORPORATION <cudatools@nvidia.com>"

RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-cudart-dev-13-2=${NV_CUDA_CUDART_DEV_VERSION} \
    cuda-command-line-tools-13-2=${NV_CUDA_LIB_VERSION} \
    cuda-minimal-build-13-2=${NV_CUDA_LIB_VERSION} \
    cuda-libraries-dev-13-2=${NV_CUDA_LIB_VERSION} \
    cuda-nvml-dev-13-2=${NV_NVML_DEV_VERSION} \
    ${NV_LIBNPP_DEV_PACKAGE} \
    libcusparse-dev-13-2=${NV_LIBCUSPARSE_DEV_VERSION} \
    ${NV_LIBCUBLAS_DEV_PACKAGE} \
    ${NV_CUDA_NSIGHT_COMPUTE_DEV_PACKAGE} \
    && rm -rf /var/lib/apt/lists/*

# Create symlink for nsys if included in nsight-compute package
RUN NSYS_PATH=$(find /opt/nvidia/nsight-compute -type f -name "nsys" 2>/dev/null | head -n 1); \
    if [ -n "$NSYS_PATH" ] && [ ! -f /usr/local/bin/nsys ]; then \
        ln -s "$NSYS_PATH" /usr/local/bin/nsys; \
    fi

# Keep apt from auto upgrading the cublas and nccl packages. See https://gitlab.com/nvidia/container-images/cuda/-/issues/88
RUN apt-mark hold ${NV_LIBCUBLAS_DEV_PACKAGE_NAME}
ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs
