# Changelog

All notable changes to this project are documented in this file.

## [2.0.2] — 2026-09-05

### Fixed
- **Cap `mcp[cli]` below 2.0** — the dependency was open-upper-bound, so `uvx --from git+...` resolved to `mcp 2.x` where `FastMCP` was renamed to `MCPServer`, breaking `from mcp.server.fastmcp import FastMCP` at import. Now pinned `mcp[cli]>=1.3.0,<2`.


## [2.0.0] — 2026-05-08

### Added
- **Blender 4.2+ Extension format** — Converted from legacy addon to `blender_manifest.toml` extension package. Supports drag-and-drop install.
- **Factory-reset persistence** — Hooks `load_factory_preferences_post` and `load_factory_startup_post` to automatically re-enable the addon after `bpy.ops.wm.read_factory_settings()`, preventing the MCP socket from being killed during scene resets.
- **Tag-triggered release workflow** — GitHub Actions workflow publishes releases on version tags (including pre-release tags like `v2.0.0-dev.1`).
- **Hermes Agent MCP configuration** — Documented config snippet for `~/.config/hermes-agent/config.yaml`.

### Changed
- **Project layout flattened** — Addon files moved from `src/blender_mcp_addon/` to repository root. MCP client files moved from `src/blender_mcp/` to `blender_mcp/`.
- **Preferences durability** — All settings (port, autostart, API keys, integration toggles) moved to `AddonPreferences` so they persist across File > New and scene reloads.
- **README overhaul** — Expanded install instructions per OS, added Blender 4.2 drag-drop notes, added autostart documentation.

### Removed
- **Telemetry** — All analytics/telemetry code removed. The addon makes zero outbound HTTP requests unless an asset integration is explicitly enabled.
- **Supabase dependency** — Removed from lockfile.

## [1.x] — Pre-fork

See [upstream BlenderMCP repository](https://github.com/ahujasid/blender-mcp) for earlier history.
