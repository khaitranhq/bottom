# Docker Guide for Bottom

Build the `btm` binary in a container and extract it to your local filesystem.

## Build and Extract Binary

Build the Docker image:

```bash
docker build -t bottom:latest -f docker/Dockerfile .
```

Extract the binary to a local folder:

```bash
docker create --name bottom-temp bottom:latest
docker cp bottom-temp:/usr/local/bin/btm ./btm
docker rm bottom-temp
```

The binary will be available at `./btm`.

## Run Container Interactively

```bash
docker run -it bottom:latest
```

## Make Binary Executable

```bash
chmod +x ./btm
./btm
```
