ARG IMAGE_NAME
FROM ${IMAGE_NAME}:12.6.0-base-amzn2023 as base

ENV NV_CUDA_LIB_VERSION 12.6.0-1

FROM base as base-amd64

ENV NV_NVTX_VERSION 12.6.37-1
ENV NV_LIBNPP_VERSION 12.3.1.23-1
ENV NV_LIBNPP_PACKAGE libnpp-12-6-${NV_LIBNPP_VERSION}
ENV NV_LIBCUBLAS_VERSION 12.6.0.22-1
FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer "NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"

RUN yum install -y \
    cuda-libraries-12-6-${NV_CUDA_LIB_VERSION} \
    cuda-nvtx-12-6-${NV_NVTX_VERSION} \
    ${NV_LIBNPP_PACKAGE} \
    libcublas-12-6-${NV_LIBCUBLAS_VERSION} \
    && yum clean all \
    && rm -rf /var/cache/yum/*

# Add entrypoint items
COPY entrypoint.d/ /opt/nvidia/entrypoint.d/
COPY nvidia_entrypoint.sh /opt/nvidia/
ENV NVIDIA_PRODUCT_NAME="CUDA"
ENTRYPOINT ["/opt/nvidia/nvidia_entrypoint.sh"]