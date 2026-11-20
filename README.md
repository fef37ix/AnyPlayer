AnyPlayer:
The Ultimate Open-Source (Linux) Audio Client & Infrastructure Daemon

AnyPlayer is a next-generation, all-in-one audio application designed for power users and self-hosted audio enthusiasts. It bridges the gap between highly visual modern streaming clients (like Spotify or Feishin) and robust, network-driven multiroom audio backends (like Mopidy and Snapcast).
Instead of forcing users to stitch together separate servers, players, and control apps, AnyPlayer packs the entire self-hosted audio ecosystem into a single, unified Linux desktop experience capable of local playback, remote streaming, and multiroom zone routing.

1. Core Architecture & Philosophy

The foundational idea of AnyPlayer is to act as both a beautiful frontend client and an embedded headless infrastructure provider.

Key Capabilities:

• Hybrid Core: Plays local audio files and connects to remote Subsonic/Jellyfin servers out of the box.
• Embedded Daemons: Packages and automatically spins up a lightweight, background instance of Mopidy (Server) and Snapcast (Snapserver + Snapclient) within the application lifecycle (e.g., via a Flatpak bundle).
• Unified Control Center: Directs local audio playback or seamlessly "casts" and controls the embedded (or an external) Mopidy instance from the exact same interface.
• Native Multiroom Management: Integrates a visual zone-manager directly into the sidebar to group, scale, and adjust volumes of different Snapcast rooms in the network.
• Seamless Playback Handoff: Switches audio routing instantly between your local Linux PC, your phone, or synchronized network speakers.

2. Potential Foundations (Where to Start?)

Instead of building a massive media player from scratch, the project can leverage existing open-source giants:
• Base Option A: Feishin (Electron / React / TypeScript)
	• Why: It offers the most polished, Spotify-like UI available on the Linux desktop today. It already has flawless Subsonic parsing and local file indexing.
	• The Task: Fork Feishin, decouple its hardcoded MPV player engine, and build a modular playback wrapper that can toggle between local streaming and remote Mopidy/JSON-RPC control.
• Base Option B: Finamp (Flutter / Dart)
	• Why: Written in Flutter, making it incredibly lightweight, fast, and multi-platform (Linux Desktop + Android/iOS).
	• The Task: Expand its backend architecture to support the Subsonic API alongside its native Jellyfin integration, then implement the Mopidy/Snapcast control layers.
• Base Option C: Iris (Mopidy Web Frontend)
	• Why: It already natively implements Mopidy control, Spotify/Subsonic integrations, and Snapcast multiroom slider groups.
	• The Task: Wrap Iris into an Electron/Tauri desktop shell, bundle Mopidy and Snapcast executables into the package, and add a local file indexing system to turn a web-app into a native desktop app.

3. First Steps & Proof of Concept (PoC)

To prove this architecture is viable, development should focus on a Minimum Viable Product (MVP) that resolves the core playback routing dilemma.

Step 1: Build the Multi-Backend Audio Service

Create a unified audio controller wrapper inside the app. When a user clicks "Play", this service must decide between two modes:
1. Local Mode: Stream the track directly into the application's local audio engine (for private listening on the PC).
2. Cast Mode: Send a JSON-RPC command to the embedded or network Mopidy instance, letting Mopidy handle the stream pipeline.

Step 2: Address the Architectural Trade-off (The Stream Dilemma)

When designing the PoC, a critical decision must be made regarding how Subsonic streams are handled:
• The Pure Mopidy Approach (Simpler PoC):
	• Eliminate native local streaming entirely. The UI always tells Mopidy to play the song (using mopidy-subidy for Subsonic or mopidy-local for local files). To hear it on the PC, the app simply activates its integrated Snapclient.
	• The Catch: While highly synchronous, this forces the user into a rigid pipeline. It makes it incredibly difficult to connect to an existing, separate Snapcast server on the network without breaking local routing.
• The Smart Hybrid Approach (Target Goal):
	• The frontend app maintains its own independent Subsonic streaming engine. It can play audio locally or act as an independent controller that pushes metadata to external/internal Mopidy instances. This preserves the ability to hook into any pre-existing household Snapcast infrastructure seamlessly.

Step 3: Package the Embedded Daemons

Configure a prototype script that launches a sandboxed, low-overhead instance of mopidy and snapserver on app startup, binds them to localhost ports, and gracefully terminates them when AnyPlayer closes.
