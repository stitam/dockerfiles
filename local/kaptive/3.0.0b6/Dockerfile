FROM debian:bookworm-20241223 AS app

ARG KAPTIVE_VER="3.0.0b6"
ARG blastVer="2.3.0"

ARG DEBIAN_FRONTEND=noninteractive

LABEL base.image="debian:bookworm"
LABEL dockerfile.version="1"
LABEL software="Kaptive"
LABEL software.version=$KAPTIVE_VER
LABEL description="Report information about surface polysaccharide loci for Klebsiella pneumoniae species complex and Acinetobacter baumannii genome assemblies"
LABEL website="https://github.com/klebgenomics/Kaptive"
LABEL license="https://github.com/klebgenomics/Kaptive/blob/master/LICENSE"
LABEL maintainer="Tamas Stirling"
LABEL maintainer.email="stirling.tamas@gmail.com"

# Download kaptive to include reference databases
RUN apt-get update && apt-get install -y wget
RUN wget "https://github.com/klebgenomics/Kaptive/releases/download/v${KAPTIVE_VER}/kaptive-${KAPTIVE_VER}.tar.gz" && \
  tar -xzf kaptive-${KAPTIVE_VER}.tar.gz && \
  rm -f kaptive-${KAPTIVE_VER}.tar.gz && \
  mv /kaptive-${KAPTIVE_VER} /kaptive

# Install conda
# See https://hub.docker.com/r/conda/miniconda3/dockerfile
RUN apt-get -qq update && apt-get -qq -y install curl bzip2 \
    && curl -sSL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o /tmp/miniconda.sh \
    && bash /tmp/miniconda.sh -bfp /usr/local \
    && rm -rf /tmp/miniconda.sh \
    && conda install -y python=3 \
    && conda update conda \
    && apt-get -qq -y remove curl bzip2 \
    && apt-get -qq -y autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/* /var/log/dpkg.log \
    && conda clean --all --yes
ENV PATH=/opt/conda/bin:$PATH

# Install biopython
RUN conda install -y -c conda-forge biopython>=1.83

# Install kaptive
RUN conda install -y -c bioconda kaptive

# Activate conda environment
RUN conda init bash \
    && . ~/.bashrc \
    && conda create --name kaptive \
    && conda activate kaptive

WORKDIR /data

# test layer

FROM app AS test

# test with A. baumannii
RUN wget "https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/016/486/705/GCA_016486705.1_PDT000751301.1/GCA_016486705.1_PDT000751301.1_genomic.fna.gz" && \
  gunzip GCA_016486705.1_PDT000751301.1_genomic.fna.gz
# k locus
RUN kaptive assembly /kaptive/reference_database/Acinetobacter_baumannii_k_locus_primary_reference.gbk GCA_016486705.1_PDT000751301.1_genomic.fna 
# o locus
RUN kaptive assembly /kaptive/reference_database/Acinetobacter_baumannii_OC_locus_primary_reference.gbk GCA_016486705.1_PDT000751301.1_genomic.fna 

# test with K. pneumoniae
RUN wget "https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/022/268/055/GCA_022268055.1_PDT000434809.1/GCA_022268055.1_PDT000434809.1_genomic.fna.gz" && \
  gunzip GCA_022268055.1_PDT000434809.1_genomic.fna.gz
# k locus
RUN kaptive assembly /kaptive/reference_database/Klebsiella_k_locus_primary_reference.gbk GCA_022268055.1_PDT000434809.1_genomic.fna
# o locus
RUN kaptive assembly /kaptive/reference_database/Klebsiella_o_locus_primary_reference.gbk GCA_022268055.1_PDT000434809.1_genomic.fna