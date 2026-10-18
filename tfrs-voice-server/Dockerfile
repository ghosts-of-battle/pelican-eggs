# Pelican yolk image for the TFRS voice server.
# Stage 1: build the Native AOT musl binary exactly like upstream voice-server/Dockerfile.
ARG GIT_REF=v2.0.0
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
ARG GIT_REF
RUN apk add --no-cache clang build-base zlib-dev git
RUN git clone --depth 1 --branch "${GIT_REF}" https://github.com/ltsammy/task-force-radio-standalone.git /src
WORKDIR /src/voice-server/src/Tfrs.VoiceServer
RUN dotnet restore Tfrs.VoiceServer.csproj -r linux-musl-x64 \
 && dotnet publish Tfrs.VoiceServer.csproj -c Release -r linux-musl-x64 -o /app

# Stage 2: drop the binary into the Pelican alpine yolk (keeps the yolk entrypoint and container user).
FROM ghcr.io/parkervcp/yolks:alpine
COPY --from=build /app/tfrs-voice-server /usr/local/bin/tfrs-voice-server
