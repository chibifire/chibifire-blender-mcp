# Install and use

Moved out of `README.md`, which is capped at 40 lines. Nothing here is changed from
the version that lived there, including the clone and `uvx` URLs.

## Install

### Prerequisites

- Blender 4.2+ (recommended; uses the Extensions system)
- Python 3.10+
- [uv](https://docs.astral.sh/uv/getting-started/installation/)

On macOS:
```bash
brew install uv
```
On Windows:
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### Blender addon

#### 1. Build the addon zip

Clone this repo, then zip the add-on package directory under `addons/`:

macOS / Linux:
```bash
git clone https://github.com/chibifire/chibifire-blender-mcp
cd chibifire-blender-mcp/addons
zip -r ../blender_mcp_addon.zip blender_mcp_addon -x '*/__pycache__/*' '*.pyc'
```

Windows (PowerShell):
```powershell
git clone https://github.com/chibifire/chibifire-blender-mcp
cd chibifire-blender-mcp\addons
Compress-Archive -Path blender_mcp_addon -DestinationPath ..\blender_mcp_addon.zip
```

The result is `blender_mcp_addon.zip` at the repo root.

#### 2. Install in Blender

**Blender 4.2+ (Extensions system, recommended):**

Drag `blender_mcp_addon.zip` onto a Blender window and confirm the install prompt.

Or, via menu: **Edit > Preferences > Get Extensions**, click the **▼** dropdown in the top-right of the panel, choose **Install from Disk...**, and pick the zip. Search for *Blender MCP* and enable it.

**Blender 3.0–4.1 (legacy add-on system):**

**Edit > Preferences > Add-ons**, click **Install...**, pick the zip, then tick the checkbox next to **Interface: Blender MCP**.

#### 3. Configure

Open **Edit > Preferences > Add-ons > Blender MCP** for full settings, or use the 3D Viewport sidebar (press `N`) under the **BlenderMCP** tab for the most common toggles. The server auto-starts on launch by default; uncheck **Auto-connect on Blender launch** to opt out.

#### Updating

To upgrade, repeat steps 1–2 with a fresh clone (or `git pull` and re-zip). Blender replaces the existing install. Your preferences (port, autostart) are stored per-user and survive reinstalls.

#### Uninstall

In the same Add-ons / Get Extensions panel, expand the entry and click **Remove** (or **Uninstall**).

### MCP client config

Point your client at this fork via `uvx --from git+...`. The console script is still `blender-mcp`.

#### Claude Desktop

In `claude_desktop_config.json`:

```json
{
    "mcpServers": {
        "blender": {
            "command": "uvx",
            "args": [
                "--from",
                "git+https://github.com/chibifire/chibifire-blender-mcp",
                "blender-mcp"
            ]
        }
    }
}
```

#### Claude Code

```bash
claude mcp add blender -- uvx --from git+https://github.com/chibifire/chibifire-blender-mcp blender-mcp
```

#### Cursor

Same JSON as Claude Desktop. Use the global MCP setting, or create `.cursor/mcp.json` in your project root.

On Windows, wrap the command in `cmd /c`:

```json
{
    "mcpServers": {
        "blender": {
            "command": "cmd",
            "args": [
                "/c",
                "uvx",
                "--from",
                "git+https://github.com/chibifire/chibifire-blender-mcp",
                "blender-mcp"
            ]
        }
    }
}
```

#### Hermes Agent

In `~/.config/hermes-agent/config.yaml` (or `%APPDATA%\hermes\config.yaml` on Windows):

```yaml
mcp_servers:
  - name: blender
    command: uvx
    args:
      - "--from"
      - "git+https://github.com/chibifire/chibifire-blender-mcp"
      - "blender-mcp"
```

Run only one MCP server at a time. If both Cursor and Claude Desktop launch one, they will fight over the Blender socket.

### Environment variables

- `BLENDER_HOST` (default `localhost`)
- `BLENDER_PORT` (default `9876`)

## Use

![BlenderMCP in the sidebar](../addons/blender_mcp_addon/assets/addon-instructions.png)

1. Enable the addon. The MCP server starts automatically on the configured port (default 9876). To opt out of autostart, uncheck **Auto-connect on Blender launch** in the addon preferences.
2. Start your MCP client. Tools appear under the hammer icon.

To stop or restart the server manually, use the **Disconnect from MCP server** / **Connect to MCP server** button in the BlenderMCP panel.

![BlenderMCP in the sidebar](../addons/blender_mcp_addon/assets/hammer-icon.png)

### What you can ask Claude to do

- "Create a low-poly dungeon scene with a dragon guarding gold"
- "Build a low-poly forest with scattered trees, rocks, and a path"
- "Make this car red and metallic"
- "Model a procedural spiral staircase with 12 steps"
- "Get the current scene info and render it as a three.js sketch"
- "Point the camera at the scene and make it isometric"

## Troubleshooting

- **No connection.** Confirm the panel reads "Disconnect from MCP server" (server is up). If it reads "Connect", autostart was disabled or the previous start failed — click it. Confirm your MCP client is configured. Do not run `uvx ... blender-mcp` manually in a terminal; the client launches it.
- **Timeouts.** Break the request into smaller steps.
- **Persistent failures.** Restart Blender and the MCP client.
