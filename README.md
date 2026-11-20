# AnyPlayer

AnyPlayer is a planned open-source music player for Linux that brings a personal music library and multiroom playback into one desktop app. Pick an album from local files or a self-hosted server, then choose where to hear it: on your computer, in the living room, or across several rooms in sync.

**Project status:** Early design. The first milestone is to play one track through two different outputs. The implementation language and desktop framework are still open.

## The listening experience

- **Browse your music.** Explore local files and libraries hosted on Navidrome or another Subsonic-compatible server, or on Jellyfin. Synfonium and Feishin are inspirations for how the library should feel to browse.
- **Choose where to play.** Listen directly on the Linux desktop or use the same controls to start playback on a Mopidy server. The aim is to support both an existing server and one managed by AnyPlayer.
- **Control rooms together.** Snapcast keeps speakers in sync. A room view inspired by Iris would let listeners group rooms and adjust their volumes from the player.

## First prototype

Select a track from a Subsonic-compatible library and try it in two places:

1. Select **This computer**. AnyPlayer streams the track through its local playback engine.
2. Select **Living room**. AnyPlayer asks Mopidy to play the same track, and Snapcast carries the audio to the selected room.

Keeping desktop playback independent of Mopidy would let people listen locally and connect to a multiroom system they already run. One alternative under consideration is to route all playback through Mopidy and use a Snapcast client on the desktop.

## Implementation options

Feishin, Finamp and Iris are possible starting points. A later packaging goal is for AnyPlayer to start and stop its own Mopidy and Snapcast services alongside the desktop app. Flatpak is one packaging option under consideration.
