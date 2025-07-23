FROM debian:bookworm-20241223 AS app

ARG METAMARC_VER="1.0.0"

ARG DEBIAN_FRONTEND=noninteractive

LABEL base.image="debian:bookworm"
LABEL dockerfile.version="1"
LABEL software="Meta-MARC"
LABEL software.version=$METAMARC_VER
LABEL description="Metagenomic Markov Models for Antimicrobial Resistance Characterization"
LABEL website="https://github.com/lakinsm/meta-marc"
LABEL license="https://github.com/lakinsm/meta-marc/blob/master/LICENSE"
LABEL maintainer="Tamas Stirling"
LABEL maintainer.email="stirling.tamas@stifabio.com"

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    gzip \
    tar \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Install Meta-MARC
RUN git clone --single-branch https://github.com/lakinsm/meta-marc.git \
  && cd meta-marc \
  && echo "yes" | ./Build.sh

# Set PATH
ENV PATH="/meta-marc/bin:${PATH}"

# set final working directory to /data
WORKDIR /data

