FROM opensuse/leap:16.0 AS base

ENV PKG_CMD=zypper
RUN ${PKG_CMD} refresh && ${PKG_CMD} --non-interactive install -y ca-certificates binutils glibc-devel kernel-headers curl

FROM base AS base-amd64

ENV NVARCH=x86_64
ENV NVIDIA_REQUIRE_CUDA="cuda>=13.2 brand=unknown,driver>=535,driver<536 brand=grid,driver>=535,driver<536 brand=tesla,driver>=535,driver<536 brand=nvidia,driver>=535,driver<536 brand=quadro,driver>=535,driver<536 brand=quadrortx,driver>=535,driver<536 brand=nvidiartx,driver>=535,driver<536 brand=vapps,driver>=535,driver<536 brand=vpc,driver>=535,driver<536 brand=vcs,driver>=535,driver<536 brand=vws,driver>=535,driver<536 brand=cloudgaming,driver>=535,driver<536 brand=unknown,driver>=570,driver<571 brand=grid,driver>=570,driver<571 brand=tesla,driver>=570,driver<571 brand=nvidia,driver>=570,driver<571 brand=quadro,driver>=570,driver<571 brand=quadrortx,driver>=570,driver<571 brand=nvidiartx,driver>=570,driver<571 brand=vapps,driver>=570,driver<571 brand=vpc,driver>=570,driver<571 brand=vcs,driver>=570,driver<571 brand=vws,driver>=570,driver<571 brand=cloudgaming,driver>=570,driver<571 brand=unknown,driver>=580,driver<581 brand=grid,driver>=580,driver<581 brand=tesla,driver>=580,driver<581 brand=nvidia,driver>=580,driver<581 brand=quadro,driver>=580,driver<581 brand=quadrortx,driver>=580,driver<581 brand=nvidiartx,driver>=580,driver<581 brand=vapps,driver>=580,driver<581 brand=vpc,driver>=580,driver<581 brand=vcs,driver>=580,driver<581 brand=vws,driver>=580,driver<581 brand=cloudgaming,driver>=580,driver<581 brand=unknown,driver>=590,driver<591 brand=grid,driver>=590,driver<591 brand=tesla,driver>=590,driver<591 brand=nvidia,driver>=590,driver<591 brand=quadro,driver>=590,driver<591 brand=quadrortx,driver>=590,driver<591 brand=nvidiartx,driver>=590,driver<591 brand=vapps,driver>=590,driver<591 brand=vpc,driver>=590,driver<591 brand=vcs,driver>=590,driver<591 brand=vws,driver>=590,driver<591 brand=cloudgaming,driver>=590,driver<591"
ENV NV_CUDA_CUDART_VERSION=13.2.51-1

COPY cuda.repo-x86_64 /etc/zypp/repos.d/cuda.repo
RUN chmod 644 /etc/zypp/repos.d/cuda.repo

FROM base AS base-arm64

ENV NVARCH=sbsa
ENV NVIDIA_REQUIRE_CUDA="cuda>=13.2 brand=unknown,driver>=535,driver<536 brand=grid,driver>=535,driver<536 brand=tesla,driver>=535,driver<536 brand=nvidia,driver>=535,driver<536 brand=quadro,driver>=535,driver<536 brand=quadrortx,driver>=535,driver<536 brand=nvidiartx,driver>=535,driver<536 brand=vapps,driver>=535,driver<536 brand=vpc,driver>=535,driver<536 brand=vcs,driver>=535,driver<536 brand=vws,driver>=535,driver<536 brand=cloudgaming,driver>=535,driver<536 brand=unknown,driver>=570,driver<571 brand=grid,driver>=570,driver<571 brand=tesla,driver>=570,driver<571 brand=nvidia,driver>=570,driver<571 brand=quadro,driver>=570,driver<571 brand=quadrortx,driver>=570,driver<571 brand=nvidiartx,driver>=570,driver<571 brand=vapps,driver>=570,driver<571 brand=vpc,driver>=570,driver<571 brand=vcs,driver>=570,driver<571 brand=vws,driver>=570,driver<571 brand=cloudgaming,driver>=570,driver<571 brand=unknown,driver>=580,driver<581 brand=grid,driver>=580,driver<581 brand=tesla,driver>=580,driver<581 brand=nvidia,driver>=580,driver<581 brand=quadro,driver>=580,driver<581 brand=quadrortx,driver>=580,driver<581 brand=nvidiartx,driver>=580,driver<581 brand=vapps,driver>=580,driver<581 brand=vpc,driver>=580,driver<581 brand=vcs,driver>=580,driver<581 brand=vws,driver>=580,driver<581 brand=cloudgaming,driver>=580,driver<581 brand=unknown,driver>=590,driver<591 brand=grid,driver>=590,driver<591 brand=tesla,driver>=590,driver<591 brand=nvidia,driver>=590,driver<591 brand=quadro,driver>=590,driver<591 brand=quadrortx,driver>=590,driver<591 brand=nvidiartx,driver>=590,driver<591 brand=vapps,driver>=590,driver<591 brand=vpc,driver>=590,driver<591 brand=vcs,driver>=590,driver<591 brand=vws,driver>=590,driver<591 brand=cloudgaming,driver>=590,driver<591"
ENV NV_CUDA_CUDART_VERSION=13.2.51-1

COPY cuda.repo-arm64 /etc/zypp/repos.d/cuda.repo
RUN chmod 644 /etc/zypp/repos.d/cuda.repo


FROM base-${TARGETARCH}

ARG TARGETARCH

LABEL maintainer="NVIDIA CORPORATION <sw-cuda-installer@nvidia.com>"

ENV CUDA_VERSION=13.2.0

# Execute OS-specific custom commands

# For libraries in the cuda-compat-* package: https://docs.nvidia.com/cuda/eula/index.html#attachment-a
RUN ${PKG_CMD} --non-interactive --gpg-auto-import-keys refresh && \
    ${PKG_CMD} --non-interactive --gpg-auto-import-keys install \
    cuda-cudart-13-2-${NV_CUDA_CUDART_VERSION} \
    cuda-compat-13-2 \
    && ${PKG_CMD} clean -a \
    && rm -rf /var/cache/zypp/*

# nvidia-docker 1.0
RUN echo "/usr/local/cuda/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64:/usr/local/cuda/lib64

COPY NGC-DL-CONTAINER-LICENSE /

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility