no <p align="center">
  <img src="https://github.com/duckycashy/assets/icon" alt="NoTrace" width="15%"/>
</p>

<h1 align="center">NoTrace</h1>

<p align="center">
A client-side Minecraft mod focused on privacy, client fingerprinting protection, and reducing unnecessary client information exposed to servers.
</p>

<p align="center">
<img alt="Minecraft Version" src="https://img.shields.io/badge/Minecraft-26.3-00AF5C">
<img alt="Fabric" src="https://img.shields.io/badge/Loader-Fabric-DBD0B4">
<img alt="Java" src="https://img.shields.io/badge/Java-25-ED8B00">
<img alt="License" src="https://img.shields.io/badge/License-GPL--3.0-blue">
</p>

> [!WARNING]
> NoTrace is a privacy and security project. It is not intended to bypass server rules, evade bans, or provide gameplay advantages. Users are responsible for following the rules of the servers they connect to.

## What it does

NoTrace is designed to reduce the amount of information that a Minecraft server can use to fingerprint or identify a modded client.

- **Vanilla Brand Spoofing** - Present a vanilla-compatible client brand when enabled
- **Channel Spoofing** - Prevent selected mod network channels from exposing installed mods
- **Known-Pack Filtering** - Filter mod-specific known-pack identifiers during the configuration process
- **Pack Cache Isolation** - Keep server resource-pack caches separated between Minecraft accounts
- **Local URL Blocking** - Block resource-pack requests targeting local or private network addresses
- **Server Pack Controls** - Control how required server resource packs are applied
- **Shader Override Protection** - Prevent server resource packs from overriding shaders belonging to protected mods
- **Key Resolution Protection** - Prevent translation-key probes from exposing mods and keybind information
- **Meteor Compatibility** - Handle known key-resolution behavior that can expose Meteor Client
- **Mod Whitelist** - Allow trusted mods to retain their server-facing functionality
- **Chat Signing Control** - Configure how chat message signatures are handled
- **Telemetry Blocking** - Prevent Minecraft telemetry from being sent when enabled
- **Detection Alerts** - Notify the player when supported fingerprinting or probing behavior is detected
- **Mod Menu Integration** - Access NoTrace settings and information through Mod Menu

## Requirements

- **Minecraft:** 26.3
- **Fabric Loader:** 0.19.5+
- **Fabric API:** Matching Minecraft 26.3
- **Java:** 25+

NoTrace is currently developed specifically for **Minecraft 26.3**.

Older Minecraft versions are not supported by this release.

## Installation

1. Install [Fabric Loader](https://fabricmc.net/use/) for Minecraft 26.3.
2. Install the matching [Fabric API](https://modrinth.com/mod/fabric-api).
3. Download the latest NoTrace release.
4. Place the NoTrace `.jar` inside your `.minecraft/mods` folder.
5. Launch Minecraft.

[Mod Menu](https://modrinth.com/mod/modmenu) is optional but recommended for accessing NoTrace's configuration.

## Configuration

NoTrace provides configuration options for controlling individual privacy protections.

The configuration can be accessed through **Mod Menu**.

### Protection

| Setting | Description |
|---------|-------------|
| **Vanilla Brand Spoofing** | Controls whether NoTrace presents a vanilla-compatible client brand. |
| **Channel Spoofing** | Controls filtering of mod network channels. |
| **Known-Pack Filtering** | Filters mod-specific known-pack identifiers. |
| **Pack Cache Isolation** | Separates server resource-pack caches between accounts. |
| **Block Local URLs** | Blocks resource-pack requests targeting local or private addresses. |
| **Server Pack Mode** | Controls how required server resource packs are handled. |
| **Shader Override Protection** | Prevents protected mod shaders from being replaced by server resource packs. |
| **Key Resolution Protection** | Protects against translation-key probing. |
| **Meteor Compatibility** | Applies compatibility handling for Meteor's key-resolution behavior. |
| **Fake Default Keybinds** | Controls whether vanilla keybinds return their default values during protected resolution. |

### Whitelist

The whitelist allows specific mods to continue exposing functionality that requires communication with a server.

**Modes:**

- **OFF** — Protect all supported mod information.
- **AUTO** — Automatically allow mods that appear to require server communication.
- **CUSTOM** — Manually select which mods are allowed.

Whitelisted mods may expose information that NoTrace would otherwise filter.

### Privacy

| Setting | Description |
|---------|-------------|
| **Chat Signing** | Configure chat signature behavior. |
| **Telemetry Blocking** | Prevent Minecraft telemetry from being sent to Mojang. |
| **Show Alerts** | Display notifications when supported detection attempts are detected. |
| **Show Toasts** | Display important protection notifications. |
| **Log Detections** | Record detection events in the Minecraft log. |

## Feature Details

### Vanilla Brand Spoofing

Minecraft clients can expose their client brand during server communication.

NoTrace can replace the normal modded client brand with a vanilla-compatible brand.

When enabled, NoTrace also applies the protections required to keep other mod-specific information from immediately contradicting the spoofed brand.

---

### Channel Spoofing

Mods can register custom network channels that servers can use to identify installed mods.

NoTrace can filter selected mod channels before they are exposed to the server.

Mods that genuinely require server communication can be exempted through the whitelist.

---

### Known-Pack Filtering

Modern Minecraft versions can exchange known-pack information during the configuration process.

Mods can add their own identifiers to this information.

NoTrace can remove protected mod-specific entries while preserving entries required for normal Minecraft functionality and whitelisted mods.

---

### Pack Cache Isolation

Server resource packs are normally stored in a shared cache.

This can potentially allow a server to associate a previously downloaded resource pack with a particular Minecraft installation.

NoTrace can maintain separate resource-pack cache locations for different Minecraft accounts.

---

### Local URL Blocking

A malicious resource-pack URL can potentially target local or private network addresses.

NoTrace checks resource-pack destinations and blocks requests targeting protected local or private addresses.

This is intended to reduce the ability of a server to use resource-pack downloads as a way to probe the player's local network.

---

### Server Pack Controls

Servers can request resource packs and, in some situations, require them.

NoTrace provides additional control over how server resource packs are applied.

Available behavior will depend on the selected configuration mode.

The goal is to allow the player to keep the resource pack available while preventing unwanted client-side effects from becoming an unnecessary fingerprinting mechanism.

---

### Shader Override Protection

A server resource pack can contain files targeting paths used by installed mods.

A malicious pack could attempt to replace mod shaders, potentially causing visual changes, crashes, or other detectable behavior.

NoTrace can filter shader overrides targeting protected mods while allowing the remainder of the server resource pack to load normally.

---

### Key Resolution Protection

Servers can use translation keys and other text-processing behavior to determine whether particular keys exist on the client.

Modded clients may expose additional keybind translation keys.

For example:

```java
key.meteor-client.open-gui
key.meteor-client.open-commands
```

A server can probe these keys and observe how the client resolves them.

NoTrace intercepts supported key-resolution paths and prevents protected mod keys from revealing information about installed mods.

Vanilla keys can retain their normal behavior depending on the selected configuration.

---

Meteor Compatibility

Some versions of Meteor Client implement their own key-resolution protection.

Certain implementations can respond differently from a vanilla client when a translation key is supplied with a fallback value.

That difference can itself become a fingerprint.

NoTrace includes compatibility handling intended to prevent this behavior from undermining NoTrace's key-resolution protection.

---

Mod Whitelist

Some mods legitimately require server communication.

Examples can include:

- Voice chat
- Map and waypoint synchronization
- Server-side companion mods
- Other mods with legitimate custom networking

The whitelist allows these mods to bypass specific NoTrace protections when necessary.

AUTO mode can identify mods that register network channels.

CUSTOM mode allows the player to select individual mods manually.

---

Chat Signing Control

Minecraft chat messages can contain cryptographic signatures.

NoTrace provides configurable control over chat signing behavior.

Available modes can include:

- OFF — Remove signatures where possible.
- AUTO — Sign only when required.
- ON — Use normal Minecraft behavior.

Disabling signatures may prevent communication with servers that require secure chat.

---

Telemetry Blocking

Minecraft can send telemetry data to Mojang.

NoTrace can prevent supported telemetry requests from being sent.

This does not disable normal gameplay functionality.

---

Alerts

NoTrace can notify the player when supported fingerprinting behavior is detected.

Examples include:

- Key resolution probes
- Suspicious resource-pack destinations
- Local/private URL requests
- Other supported detection attempts

Alerts can be displayed through chat messages, toast notifications, or the game log depending on configuration.

Debugging

NoTrace can provide optional debugging information for development and troubleshooting.

Debug functionality is disabled by default.

When enabled, debugging can expose information such as:

- Detected translation-key probes
- Network channels
- Known-pack entries
- Resource-pack requests
- Protection decisions
- Whitelist status

Debug logging should not be enabled when maximum privacy is the priority.

Compatibility

NoTrace is designed to coexist with other Fabric mods.

Some mods may rely on network channels, known packs, translation keys, or resource-pack functionality to operate correctly.

If a mod stops functioning correctly, adding it to the NoTrace whitelist may resolve the issue.

NoTrace should not be considered a replacement for server-side security software or an absolute guarantee that a client cannot be fingerprinted.

Building from Source

Prerequisites

- Java 25
- Gradle through the included wrapper
- IntelliJ IDEA or another Java IDE

Clone the repository

```
git clone https://github.com/duckycashy/NoTrace.git
cd NoTrace

## Build

Windows:
```
.\gradlew.bat build
```
Linux/macOS:
```
./gradlew build
```
The compiled JAR will be available in:

build/libs/

## Development

NoTrace is developed for Minecraft 26.3 using Fabric.

The project aims to keep its implementation modular and understandable, with individual protections separated so they can be tested and maintained independently.

Privacy protections should be implemented against the actual Minecraft and Fabric 26.3 APIs rather than relying on assumptions from older Minecraft versions.

## References

NoTrace's development is informed by existing Minecraft privacy and security research, including:

- "Fabric" (https://fabricmc.net/)
- "Fabric API" (https://github.com/FabricMC/fabric-api)
- "Mod Menu" (https://github.com/TerraformersMC/ModMenu)
- "ExploitPreventer" (https://github.com/NikOverflow/ExploitPreventer)
- "LiquidBounce" (https://github.com/CCBlueX/LiquidBounce)
- "Meteor Client" (https://github.com/MeteorDevelopment/meteor-client)
- "No Chat Reports" (https://modrinth.com/mod/no-chat-reports)
- "Wurst Wiki" (https://wurst.wiki/sign_translation_vulnerability)

NoTrace is an independent project and is not an official continuation, fork, or endorsed version of any project listed above unless explicitly stated.

License

NoTrace is licensed under the GNU General Public License v3.0 or later (GPL-3.0-or-later).

See "LICENSE" (LICENSE) for the full license text.

Disclaimer

NoTrace is a privacy-focused Minecraft modification designed to reduce unnecessary client information exposure and protect against supported fingerprinting techniques.

It does not guarantee complete anonymity or invisibility to servers.

NoTrace is not intended to bypass server bans, evade moderation, circumvent server security, or provide an unfair gameplay advantage.

Users are responsible for complying with the rules and terms of service of the servers they connect to.