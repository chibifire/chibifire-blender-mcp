# transport-blender-mcp

A Blender addon plus MCP server that lets Claude (Hermes Agent, and other MCP clients) drive Blender for prompt-assisted modeling and scene construction.

Maintained by K. S. Ernest (iFire) Lee. Forked from [BlenderMCP](https://github.com/ahujasid/blender-mcp) by Siddharth Ahuja. The fork exists because the parent sends Blender data to Supabase; this one opens a local socket and makes no outbound HTTP request.

## Features

- TCP socket server inside Blender; JSON command protocol on port 9876 (configurable)
- Auto-connects on Blender launch, and re-enables itself after a factory reset
- Create, modify, delete objects; apply materials; inspect scene and per-object data
- Run arbitrary Python inside Blender, and capture viewport screenshots
- No telemetry and no outbound HTTP requests

## Architecture

Two halves: a Blender 4.2+ extension (`addons/blender_mcp_addon/`) running a TCP socket server inside Blender, and a FastMCP server (`blender_mcp/`) that connects to that socket and exposes the tools to an MCP client. Settings live in the addon's user preferences, so they survive File > New, Open File, and scene resets.

## Install

`docs/install.md` has the full steps: prerequisites, building the addon zip, installing it in Blender 3.0 through 4.2+, the config for Claude Desktop, Claude Code, Cursor, and Hermes Agent, and the environment variables. The short version, once the addon is installed:

```bash
claude mcp add blender -- uvx --from git+https://github.com/v-sekai-multiplayer-fabric/transport-blender-mcp blender-mcp
```

Run one MCP server at a time; two clients will fight over the Blender socket.

## Use

Enable the addon and the server starts on the configured port. Start your MCP client and the tools appear under the hammer icon. Then ask for what you want built — "a low-poly dungeon scene with a dragon guarding gold", "make this car red and metallic", "a procedural spiral staircase with 12 steps". `docs/install.md` covers troubleshooting.

## Protocol and caveats

JSON over TCP: each command is an object with `type` and optional `params`, and each response carries `status` plus either `result` or `message`. `execute_blender_code` runs arbitrary Python inside Blender, so save your work first, and complex operations may need breaking into smaller steps.

## License

MIT, see `LICENSE`. Version history in `CHANGELOG.md`.
