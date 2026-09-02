# Changelog

All notable changes to Dual Display Manager will be documented in this file.

Version numbering follows the format `Major.Minor`. Minor maintenance revisions may additionally use a patch number where required.

---

## [Unreleased]

### Planned Addition

- Documentation for the companion Blueprint **Cinema Source Start – Denon / Marantz**.
- The companion Blueprint can start a compatible AVR and TV when a selected source device becomes active, while protecting an active projector.
- The commercial YAML file remains separate from this public repository.

---

## [1.0] – 2026-09-02

### Initial Release

First public release of **Dual Display Manager for Home Assistant**.

### Added

- Automatic mutual locking between a television and projector.
- Automatic switching between two Denon/Marantz monitor outputs.
- Default support for Monitor 1 using `VSMONI1` and Monitor 2 using `VSMONI2`.
- Configurable AVR monitor commands.
- Automatic TV shutdown when the projector is activated.
- Automatic projector shutdown when the TV becomes active.
- Projector priority logic to prevent delayed TV state reports from immediately overriding a newly started projector.
- Configurable projector priority time, TV state-check delay and switching delays.
- Optional source-device validation, enabled by default.
- Support for multiple Home Assistant `media_player` source entities.
- Automatic return to the TV monitor output when the projector is switched off.
- Optional TV startup using Wake-on-LAN or `media_player.turn_on`.
- Option to disable automatic TV startup.
- Configurable delay after TV startup and repeated AVR TV-output command to improve switching reliability.
- Handling of common Home Assistant device states: `off`, `standby`, `unavailable` and `unknown`.
- Blueprint-based configuration through the Home Assistant interface.
- Distribution without installation-specific entity IDs or fixed MAC addresses.
- Compatibility settings grouped into user-friendly Blueprint sections.
- Advanced timing settings available without requiring manual YAML modification.

### Requirements

- Home Assistant 2024.6 or newer.
- Compatible Denon or Marantz AV receiver.
- Denon AVR integration with access to `denonavr.get_command`.
- Television and projector exposed as Home Assistant `media_player` entities.
- Wake-on-LAN integration when Wake-on-LAN startup is selected.

### Known Limitations

- Version 1.0 is intended for installations using two AVR monitor outputs.
- Receiver behavior may vary by Denon or Marantz model.
- HDMI-CEC behavior varies between manufacturers and devices.
- Some televisions and projectors may require adjusted timing values.
- Other AVR manufacturers are not officially supported in Version 1.0.
- Compatibility with every Home Assistant media-player integration is not guaranteed.

---

## Versioning

Examples:

- `1.0` – Initial release
- `1.1` – New features or compatibility improvements
- `1.2` – Further improvements within Version 1
- `2.0` – Major redesign or significant new functionality

Older releases remain identified by their original version number even when newer versions become available.
