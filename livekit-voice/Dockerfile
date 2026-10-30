# Pelican yolk image for the JMSB voice server (LiveKit, Apache 2.0).
# Stage 1: the official LiveKit server binary, pinned.
ARG LIVEKIT_VERSION=v1.13.9
FROM livekit/livekit-server:${LIVEKIT_VERSION} AS livekit

# Stage 2: drop the binary into the Pelican alpine yolk (keeps the yolk entrypoint and container user).
FROM ghcr.io/parkervcp/yolks:alpine
COPY --from=livekit /livekit-server /usr/local/bin/livekit-server
