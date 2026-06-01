FROM debian:bookworm-20240211

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update

# Install system dependencies
RUN apt-get install -y \
  libcurl4-openssl-dev

# Install R and R packages from apt
RUN apt-get install -y \
  r-base \
  r-cran-base64enc \
  r-cran-covr \
  r-cran-curl \
  r-cran-digest \
  r-cran-dplyr \
  r-cran-httr \
  r-cran-jsonlite \
  r-cran-knitr \
  r-cran-purrr \
  r-cran-rcdk \
  r-cran-rlang \
  r-cran-rmarkdown \
  r-cran-rvest \
  r-cran-stringr \
  r-cran-testthat \
  r-cran-tibble \
  r-cran-usethis \
  r-cran-xml2 \
  r-cran-vcr

# Install R packages from R 
RUN R -e 'options(warn = 2); install.packages("data.tree")'
RUN R -e 'options(warn = 2); install.packages("hoardr")'
RUN R -e 'options(warn = 2); install.packages("plot.matrix")'
RUN R -e 'options(warn = 2); install.packages("robotstxt")'

# Install webchem

RUN R -e 'options(warn = 2); install.packages("webchem")'

# Set R library path to avoid binding from host system
ENV R_LIBS_USER /usr/local/lib/R/site-library

