# Use R base package 4.4.0
ARG R_VERSION="4.4.0"
FROM rocker/r-ver:${R_VERSION}

# Install essential system dependencies required by many R packages and Quarto
RUN apt-get update -y && apt-get install -y --no-install-recommends \
    # Common build tools and libraries
    build-essential \
    cmake \
    make \
    # Libraries for common R package dependencies
    libssl-dev \
    libcurl4-openssl-dev \
    libxml2-dev \
    libcairo2-dev \
    libxt-dev \
    libfreetype6-dev \
    libfontconfig1-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libbz2-dev \
    libpng-dev \
    libtiff-dev \
    libjpeg-dev \
    # Version control and download tools
    git \
    wget \
    ca-certificates \
    pkg-config \
    tree \
    vim-tiny \
    # for packages with fortran dependency
    gfortran \
    # Clean
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# --- Install Quarto CLI ---
ARG QUARTO_VERSION="1.7.30" # Using a recent version as of May 2025

# --- Install Quarto CLI ---
ARG QUARTO_VERSION="1.7.30" # Using a recent version as of May 2025

# Download and install Quarto CLI
RUN TARGET_ARCH="$(dpkg --print-architecture)" && \
    QUARTO_DL_URL="https://github.com/quarto-dev/quarto-cli/releases/download/v${QUARTO_VERSION}/quarto-${QUARTO_VERSION}-linux-${TARGET_ARCH}.deb" && \
    wget "${QUARTO_DL_URL}" -O quarto.deb && \
    dpkg -i quarto.deb && \
    rm quarto.deb
# Set PATH for Quarto executable
ENV PATH="/opt/quarto/bin:${PATH}"

# --- Install Standard and Custom R Packages ---
# Combined installation of core tidyverse, tidymodels, and custom packages.
RUN R -e "install.packages(c(\
    # tidyverse core packages
    'ggplot2', \
    'dplyr', \
    'tidyr', \
    'readr', \
    'purrr', \
    'tibble', \
    'stringr', \
    'forcats', \
    \
    # tidymodels core packages
    'broom', \
    'dials', \
    'infer', \
    'parsnip', \
    'recipes', \
    'rsample', \
    'tune', \
    'workflows', \
    'yardstick', \
    \
    # Custom Addons
    'GT', \
    'colorspace', \
    'janitor', \
    'vroom', \
    'usethis' \
    'styler'
    ), repos = 'https://cloud.r-project.org/')"

# Clean up R package installation cache (applies to all R packages installed above)
RUN rm -rf /tmp/downloaded_packages /tmp/*.rds

# Set a default working directory
WORKDIR /app
