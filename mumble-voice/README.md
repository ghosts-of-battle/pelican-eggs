# Mumble voice (JMSB)

Pelican egg for the JMSB mod's Mumble voice server: the stock Mumble server,
set up for the mod. Players install nothing. The mod's voice DLL connects to
it directly when the Arma server's **Voice server type** is Mumble.

Image: `yonvclaw/jmsb-mumble:latest`. It is the official
`mumblevoip/mumble-server` image, pinned, with Pelican's way of starting.
Pelican pulls it on every start, so a new push plus a restart updates the
server. The egg doesn't change.

## Ports

One allocation, the primary. Mumble uses TCP and UDP on the same port; the
mod uses only TCP. Mumble's own port is 64738.

## Variables

| Variable | What |
|---|---|
| Server password | The password players need. Put the same one in the Arma server's CBA settings. |
| Max users | How many may be connected at once. |
| SuperUser password | Mumble's admin account, for the Mumble app. Empty: left as it is; a new server gets a random one. |
| Temporary channels | true (default): each mission gets its own room. |

## The Arma server

CBA settings, JMSB - Voice (Built-in), set on the server:

- Voice server > **Voice server type:** Mumble.
- Voice server (Mumble) > **Mumble server address:** `host:port` as the
  players reach it, e.g. `game9001.ghostsofbattle.com:64738`.
- Voice server (Mumble) > **Mumble server password:** the egg's Server
  password.

## What the start script does

`jmsb-mumble-start`, on every start:

1. Writes `mumble-server.ini` from the variables.
2. Sets the SuperUser password when the variable is set. A new database gets
   a random one.
3. With Temporary channels on, adds one rule to the root channel: everyone
   may make a temporary channel. Stock Mumble allows that to registered
   users only. The mod makes one channel per mission (its room) and enters
   it; the server deletes it when it is empty. Off, every mission shares the
   root channel. Voice still works, but every voice is sent to everyone.
4. Runs the server. ^C stops it.

The server's database and certificate stay in the server's folder, so they
survive restarts and image updates.

## Build

```
docker build -t yonvclaw/jmsb-mumble:latest -t yonvclaw/jmsb-mumble:v1.5.939-1 .
docker push yonvclaw/jmsb-mumble:latest
docker push yonvclaw/jmsb-mumble:v1.5.939-1
```

The Mumble version is `MUMBLE_VERSION` in the Dockerfile.

## Tested (2026-11-01)

The image was built and run as Pelican does: user 988, an empty folder
mounted at /home/container, the startup line in STARTUP. The mod's voicetest
ran against it with a password containing a space and a quote:

- Two clients in different rooms heard nothing of each other.
- Two clients in the same room heard each other.
- The echo test played the voice back.
- ^C stopped the server.
