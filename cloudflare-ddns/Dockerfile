# syntax=docker/dockerfile:1.7-labs
# Source: https://github.com/favonia/cloudflare-ddns/
# Language: Go
# Windows Container build for cloudflare-ddns

# Build stage - use servercore with Go
FROM golang:1.25.1-windowsservercore-ltsc2022 AS build

ARG GIT_DESCRIBE=dev
ARG REPO_URL=https://github.com/favonia/cloudflare-ddns.git
ARG REPO_BRANCH=main

WORKDIR /src

# Clone the repository using PowerShell for proper variable expansion
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]
RUN git clone --depth 1 --branch $env:REPO_BRANCH $env:REPO_URL .

# Download dependencies (leverage Docker layer caching)
RUN go mod download

# Compile the code
RUN $env:CGO_ENABLED='0'; \
    go build -tags timetzdata -trimpath -ldflags=\"-w -s -X main.Version=$env:GIT_DESCRIBE -buildid=\" \
    -o C:\bin\ddns.exe .\cmd\ddns

# Production stage - minimal nanoserver image
FROM mcr.microsoft.com/windows/nanoserver:ltsc2022 AS prod

# Copy the compiled binary
COPY --from=build /bin/ddns.exe /bin/

# Run as non-administrator user (ContainerUser)
USER ContainerUser

ENTRYPOINT ["C:\\bin\\ddns.exe"]