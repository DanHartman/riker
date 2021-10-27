ARG NODENV_VERSION=16.11.1
ARG NODENV_ROOT=/.nodenv

FROM ubuntu:18.04 AS base
ENV TZ="America/New_York"
ENV DEBIAN_FRONTEND="noninteractive"
ENV LC_ALL C
ENV LANG=en_US.utf8
RUN apt update && apt install -y \
  wget \
  curl \
  git \
  vim \
  tzdata \
  make \
  build-essential \
  libc6-i386 \
  gettext-base \
  lsb-release

FROM base as node
ARG NODENV_ROOT
ARG NODENV_VERSION
ENV NODENV_ROOT=${NODENV_ROOT}
ENV NODENV_VERSION=${NODENV_VERSION}
RUN git clone https://github.com/nodenv/nodenv.git ${NODENV_ROOT}
RUN cd ${NODENV_ROOT} && git fetch && git checkout v1.4.0
ENV PATH="${NODENV_ROOT}/bin:${NODENV_ROOT}/shims:${PATH}"
RUN mkdir -p "${NODENV_ROOT}/plugins"
RUN git clone https://github.com/nodenv/node-build.git "${NODENV_ROOT}/plugins/node-build"
RUN ${NODENV_ROOT}/src/configure
RUN make -C ${NODENV_ROOT}/src
RUN eval "$(nodenv init -)"
RUN nodenv install ${NODENV_VERSION}

FROM base as final
# bring in node
ARG NODENV_ROOT
ARG NODENV_VERSION
ENV NODENV_ROOT=${NODENV_ROOT}
ENV NODENV_VERSION=${NODENV_VERSION}
ENV PATH="${NODENV_ROOT}/bin:${NODENV_ROOT}/shims:${PATH}"
COPY --from=node ${NODENV_ROOT} ${NODENV_ROOT}
