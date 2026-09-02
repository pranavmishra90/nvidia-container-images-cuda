ARG IMAGE_NAME
FROM ${IMAGE_NAME}:13.3.1-base-opensuse15 AS base

ENV PKG_CMD=zypper

ENV NV_CUDA_LIB_VERSION=13.3.1-1

FROM base AS base-amd64

ENV NV_NVTX_VERSION=13.3.29-1
ENV NV_LIBNPP_VERSION=13.1.2.81-1
ENV NV_LIBNPP_PACKAGE=libnpp-13-3-${NV_LIBNPP_VERSION}
ENV NV_LIBCUBLAS_VERSION=13.6.0.2-1

FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer="NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"

RUN ${PKG_CMD} --non-interactive --gpg-auto-import-keys install \
    cuda-libraries-13-3-${NV_CUDA_LIB_VERSION} \
    cuda-nvtx-13-3-${NV_NVTX_VERSION} \
    ${NV_LIBNPP_PACKAGE} \
    libcublas-13-3-${NV_LIBCUBLAS_VERSION} \
    && ${PKG_CMD} clean -a \
    && rm -rf /var/cache/zypp/*

# Add entrypoint items
COPY entrypoint.d/ /opt/nvidia/entrypoint.d/
COPY nvidia_entrypoint.sh /opt/nvidia/
ENV NVIDIA_PRODUCT_NAME="CUDA"
ENTRYPOINT ["/opt/nvidia/nvidia_entrypoint.sh"]