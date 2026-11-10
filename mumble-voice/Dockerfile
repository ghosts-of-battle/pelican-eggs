# Pelican image for the JMSB mod's Mumble voice server (Mumble, BSD-3-Clause).
# The official Mumble server image, pinned, with Pelican's way of starting:
# Wings mounts the server's folder at /home/container, runs the container as
# its own user, and puts the egg's startup line in STARTUP.
ARG MUMBLE_VERSION=v1.5.939
FROM mumblevoip/mumble-server:${MUMBLE_VERSION}

COPY --chmod=755 entrypoint /usr/local/bin/jmsb-entrypoint
COPY --chmod=755 jmsb-mumble-start /usr/local/bin/jmsb-mumble-start
ENV USER=container HOME=/home/container
WORKDIR /home/container
ENTRYPOINT ["/usr/local/bin/jmsb-entrypoint"]
CMD []
