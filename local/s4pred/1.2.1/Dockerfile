FROM debian:bookworm-20260518

ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y \
  git \
  python3 \
  python3-biopython \
  python3-torch \
  wget

# Clone repo
WORKDIR /opt
RUN git clone --depth 1 --branch v1.2.1 https://github.com/psipred/s4pred

# Download weights
WORKDIR /opt/s4pred
RUN wget http://bioinfadmin.cs.ucl.ac.uk/downloads/s4pred/weights.tar.gz && \
    tar -xzf weights.tar.gz && \
    rm weights.tar.gz

# Ensure executable
RUN chmod +x run_model.py

# Add repo to PATH
ENV PATH="/opt/s4pred:${PATH}"

# Default working directory
WORKDIR /data

