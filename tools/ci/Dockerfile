FROM ubuntu:24.04 AS builder
LABEL org.opencontainers.image.source="https://github.com/sruonLSB/LandSandBoat-Docker"

# Install dependencies
RUN apt-get update && \
    apt-get install -y \
    software-properties-common \
    cmake \
    git \
    build-essential \
    libmariadb-dev-compat \
    libluajit-5.1-dev \
    libzmq3-dev \
    zlib1g-dev \
    libssl-dev \
    binutils-dev \
    clang-18 && \
    rm -rf /var/lib/apt/lists/*

# Set Clang as the default compiler
ENV CC=/usr/bin/clang-18 \
    CXX=/usr/bin/clang++-18

# Create and set the working directory
WORKDIR /app

# Copy the application code
COPY . /app

# Configure CMake
RUN mkdir -p build && \
    cmake -S . -B build

# Build the application
RUN cmake --build build -j4

# Final stage with only the binaries
FROM ubuntu:24.04

# Install necessary runtime dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libmariadb-dev-compat \
    libluajit-5.1-dev \
    libzmq3-dev \
    zlib1g-dev \
    libssl-dev && \
    rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy binaries from the builder stage
COPY --from=builder /app/xi_connect /app/
COPY --from=builder /app/xi_map /app/
COPY --from=builder /app/xi_search /app/
COPY --from=builder /app/xi_world /app/

# Set the default command (adjust based on your application's entry point)
CMD ["/bin/bash"]