FROM rockylinux/rockylinux:9 AS base

ENV PKG_CMD=yum

FROM base AS base-amd64

ENV NVARCH=x86_64
ENV NVIDIA_REQUIRE_CUDA="cuda>=13.3 brand=unknown,driver>=535,driver<536 brand=grid,driver>=535,driver<536 brand=tesla,driver>=535,driver<536 brand=nvidia,driver>=535,driver<536 brand=quadro,driver>=535,driver<536 brand=quadrortx,driver>=535,driver<536 brand=nvidiartx,driver>=535,driver<536 brand=vapps,driver>=535,driver<536 brand=vpc,driver>=535,driver<536 brand=vcs,driver>=535,driver<536 brand=vws,driver>=535,driver<536 brand=cloudgaming,driver>=535,driver<536 brand=unknown,driver>=570,driver<571 brand=grid,driver>=570,driver<571 brand=tesla,driver>=570,driver<571 brand=nvidia,driver>=570,driver<571 brand=quadro,driver>=570,driver<571 brand=quadrortx,driver>=570,driver<571 brand=nvidiartx,driver>=570,driver<571 brand=vapps,driver>=570,driver<571 brand=vpc,driver>=570,driver<571 brand=vcs,driver>=570,driver<571 brand=vws,driver>=570,driver<571 brand=cloudgaming,driver>=570,driver<571 brand=unknown,driver>=580,driver<581 brand=grid,driver>=580,driver<581 brand=tesla,driver>=580,driver<581 brand=nvidia,driver>=580,driver<581 brand=quadro,driver>=580,driver<581 brand=quadrortx,driver>=580,driver<581 brand=nvidiartx,driver>=580,driver<581 brand=vapps,driver>=580,driver<581 brand=vpc,driver>=580,driver<581 brand=vcs,driver>=580,driver<581 brand=vws,driver>=580,driver<581 brand=cloudgaming,driver>=580,driver<581 brand=unknown,driver>=590,driver<591 brand=grid,driver>=590,driver<591 brand=tesla,driver>=590,driver<591 brand=nvidia,driver>=590,driver<591 brand=quadro,driver>=590,driver<591 brand=quadrortx,driver>=590,driver<591 brand=nvidiartx,driver>=590,driver<591 brand=vapps,driver>=590,driver<591 brand=vpc,driver>=590,driver<591 brand=vcs,driver>=590,driver<591 brand=vws,driver>=590,driver<591 brand=cloudgaming,driver>=590,driver<591 brand=unknown,driver>=595,driver<596 brand=grid,driver>=595,driver<596 brand=tesla,driver>=595,driver<596 brand=nvidia,driver>=595,driver<596 brand=quadro,driver>=595,driver<596 brand=quadrortx,driver>=595,driver<596 brand=nvidiartx,driver>=595,driver<596 brand=vapps,driver>=595,driver<596 brand=vpc,driver>=595,driver<596 brand=vcs,driver>=595,driver<596 brand=vws,driver>=595,driver<596 brand=cloudgaming,driver>=595,driver<596"
ENV NV_CUDA_CUDART_VERSION=13.3.29-1

COPY cuda.repo-x86_64 /etc/yum.repos.d/cuda.repo

FROM base AS base-arm64

ENV NVARCH=sbsa
ENV NVIDIA_REQUIRE_CUDA="cuda>=13.3 brand=unknown,driver>=535,driver<536 brand=grid,driver>=535,driver<536 brand=tesla,driver>=535,driver<536 brand=nvidia,driver>=535,driver<536 brand=quadro,driver>=535,driver<536 brand=quadrortx,driver>=535,driver<536 brand=nvidiartx,driver>=535,driver<536 brand=vapps,driver>=535,driver<536 brand=vpc,driver>=535,driver<536 brand=vcs,driver>=535,driver<536 brand=vws,driver>=535,driver<536 brand=cloudgaming,driver>=535,driver<536 brand=unknown,driver>=570,driver<571 brand=grid,driver>=570,driver<571 brand=tesla,driver>=570,driver<571 brand=nvidia,driver>=570,driver<571 brand=quadro,driver>=570,driver<571 brand=quadrortx,driver>=570,driver<571 brand=nvidiartx,driver>=570,driver<571 brand=vapps,driver>=570,driver<571 brand=vpc,driver>=570,driver<571 brand=vcs,driver>=570,driver<571 brand=vws,driver>=570,driver<571 brand=cloudgaming,driver>=570,driver<571 brand=unknown,driver>=580,driver<581 brand=grid,driver>=580,driver<581 brand=tesla,driver>=580,driver<581 brand=nvidia,driver>=580,driver<581 brand=quadro,driver>=580,driver<581 brand=quadrortx,driver>=580,driver<581 brand=nvidiartx,driver>=580,driver<581 brand=vapps,driver>=580,driver<581 brand=vpc,driver>=580,driver<581 brand=vcs,driver>=580,driver<581 brand=vws,driver>=580,driver<581 brand=cloudgaming,driver>=580,driver<581 brand=unknown,driver>=590,driver<591 brand=grid,driver>=590,driver<591 brand=tesla,driver>=590,driver<591 brand=nvidia,driver>=590,driver<591 brand=quadro,driver>=590,driver<591 brand=quadrortx,driver>=590,driver<591 brand=nvidiartx,driver>=590,driver<591 brand=vapps,driver>=590,driver<591 brand=vpc,driver>=590,driver<591 brand=vcs,driver>=590,driver<591 brand=vws,driver>=590,driver<591 brand=cloudgaming,driver>=590,driver<591 brand=unknown,driver>=595,driver<596 brand=grid,driver>=595,driver<596 brand=tesla,driver>=595,driver<596 brand=nvidia,driver>=595,driver<596 brand=quadro,driver>=595,driver<596 brand=quadrortx,driver>=595,driver<596 brand=nvidiartx,driver>=595,driver<596 brand=vapps,driver>=595,driver<596 brand=vpc,driver>=595,driver<596 brand=vcs,driver>=595,driver<596 brand=vws,driver>=595,driver<596 brand=cloudgaming,driver>=595,driver<596"
ENV NV_CUDA_CUDART_VERSION=13.3.29-1

COPY cuda.repo-arm64 /etc/yum.repos.d/cuda.repo


FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer="NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"

# sha256sum CDF6BA43.pub to find gpg_key_sum
RUN NVIDIA_GPGKEY_SUM=d0664fbbdb8c32356d45de36c5984617217b2d0bef41b93ccecd326ba3b80c87 && \
    curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/rhel9/${NVARCH}/D42D0685.pub | sed '/^Version/d' > /etc/pki/rpm-gpg/RPM-GPG-KEY-NVIDIA && \
    echo "$NVIDIA_GPGKEY_SUM  /etc/pki/rpm-gpg/RPM-GPG-KEY-NVIDIA" | sha256sum -c --strict -

ENV CUDA_VERSION=13.3.1

# Execute OS-specific custom commands

# Add Red Hat UBI AppStream repository for additional packages

# For libraries in the cuda-compat-* package: https://docs.nvidia.com/cuda/eula/index.html#attachment-a
ARG IMAGE_UPGRADE_ID=0
RUN echo "IMAGE_UPGRADE_ID=${IMAGE_UPGRADE_ID}" && \
    ${PKG_CMD} upgrade -y && ${PKG_CMD} install -y \
    cuda-cudart-13-3-${NV_CUDA_CUDART_VERSION} \
    cuda-toolkit-13-3-config-common-${NV_CUDA_CUDART_VERSION} \
    cuda-toolkit-13-config-common-${NV_CUDA_CUDART_VERSION} \
    cuda-toolkit-config-common-${NV_CUDA_CUDART_VERSION} \
    && if ${PKG_CMD} info cuda-compat-13-3 2>/dev/null | grep -q "^Name"; then \
        ${PKG_CMD} install -y cuda-compat-13-3; \
    fi \
    && ${PKG_CMD} clean all \
    && rm -rf /var/cache/yum/*

# nvidia-docker 1.0
RUN echo "/usr/local/cuda/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64:/usr/local/cuda/lib64

COPY NGC-DL-CONTAINER-LICENSE /

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility