# Stage 1: Builder
FROM rust:bookworm AS builder

# Install dependencies required for building bottom
RUN apt-get update && apt-get install -y \
    pkg-config \
    libssl-dev \
    git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /build

# Copy the project files
COPY src src
COPY Cargo.toml Cargo.toml build.rs ./

# Build the release binary
RUN cargo build --release --features deploy

# Stage 2: Binary extraction (optional - for mounting output)
FROM debian:bookworm-slim AS extractor

WORKDIR /output
COPY --from=builder /build/target/release/btm .

# Stage 3: Runtime
FROM debian:bookworm-slim AS runtime

# Install minimal runtime dependencies
RUN apt-get update && apt-get install -y \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Copy the compiled binary from builder
COPY --from=builder /build/target/release/btm /usr/local/bin/btm

# Set the entrypoint
ENTRYPOINT ["btm"]
