ARG IMAGE_NAME

FROM ${IMAGE_NAME}:13.3.0-cudnn-runtime-ubuntu24.04 AS base

FROM base AS base-arm64

ENV NV_TENSORRT_VERSION=11.0.0.114-1+cuda13.3
ENV NV_TENSORRT_PACKAGE_NAME=tensorrt-libs
ENV NV_TENSORRT_PACKAGE=${NV_TENSORRT_PACKAGE_NAME}=${NV_TENSORRT_VERSION}
FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer="NVIDIA CORPORATION <cudatools@nvidia.com>"
LABEL com.nvidia.tensorrt.version="${NV_TENSORRT_VERSION}"


RUN apt-get update && \
    if apt-cache madison ${NV_TENSORRT_PACKAGE_NAME} | awk '{print $3}' | grep -Fxq "${NV_TENSORRT_VERSION}"; then \
        echo "Installing ${NV_TENSORRT_PACKAGE_NAME}=${NV_TENSORRT_VERSION}"; \
        apt-get install -y --no-install-recommends ${NV_TENSORRT_PACKAGE}; \
    else \
        echo "Warning: ${NV_TENSORRT_PACKAGE_NAME}=${NV_TENSORRT_VERSION} not found in repo, falling back to latest available"; \
        apt-get install -y --no-install-recommends ${NV_TENSORRT_PACKAGE_NAME}; \
    fi \
    && apt-mark hold ${NV_TENSORRT_PACKAGE_NAME} \
    && rm -rf /var/lib/apt/lists/*
