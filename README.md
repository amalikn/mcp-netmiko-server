
# mcp-netmiko-server

An MCP server that enables LLMs interacting with your network devices
 via SSH (netmiko).

| Tool                                   | Description                                                               |
|:---------------------------------------|:--------------------------------------------------------------------------|
| get_network_device_list                | Return list of network device names and types defined in a TOML file.     |
| send_command_and_get_output            | Send a command to a device and returns its output.                        |
| set_config_commands_and_commit_or_save | Send configuration commands to a device and commit or save automatically. |



<img width="740" alt="mcp-netmiko-demo" src="https://github.com/user-attachments/assets/08ea7feb-25fc-45c9-a70c-83b75c01a725" />


## How to use

* Install

```console
git clone https://github.com/upa/mcp-netmiko-server
cd mcp-netmiko-server

# Write your toml file that lists your devices
vim my-devices.network.toml

# Run via stdio
uv run --with "mcp[cli]" --with netmiko main.py my-devices.network.toml

# Run as an SSE server: URL is http://localhost:10000/sse in this case
uv run --with "mcp[cli]" --with netmiko main.py my-devices.network.toml --sse
```

* Configuration

List your network devices in a toml file like [sample.network.toml](test/sample.network.toml):

```toml
[default]

username = "rouser"
password = "rouserpassword"

[qfx1]

hostname = "172.16.0.40"
device_type = "juniper_junos"

[nexus1]

hostname = "nexus1.lab"
device_type = "cisco_nxos"
```

`[default]` is a special section that defines the default values such
as `username` and `password`. Devices inherit the default values if
not defined on their sections.

For `device_type` values, see [netmiko Supported
Platforms](https://ktbyers.github.io/netmiko/PLATFORMS.html).


* claude desktop config json:

```json
{
  "mcpServers": {
    "netmiko server": {
      "command": "uv",
      "args": [
        "run",
        "--with",
        "mcp[cli]",
        "--with",
        "netmiko",
        "[PATH TO]/mcp-netmiko-server/main.py",
        "[PATH TO]/YOUR-DEVICE.toml"
      ]
    }
  }
}
```

## Local Customization Tracking
- Local machine-specific integration, client wiring, and operational state are tracked under the external data root.
- Local metadata path: `/Volumes/Data/_ai/_mcp/mcp-data/<name>/meta`
- Repo-side capability contract is in `docs/local-capability/`.
- Secrets are never stored in repo docs; only variable names and loading locations are documented.

## Externalized .venv

Repo path ".venv\" is a symlink to canonical cache location under "/Volumes/Data/_ai/_mcp/mcp-working-cache/mcp-netmiko-server/.venv\" to reduce repo-local mutable environment state.

## Local Enhancements Capture (2026-03-13)
- Captured current local changes, configuration updates, and operational enhancements for GitHub publication.
- Includes synchronization with sub-repo link updates where applicable.
- Cross-reference local docs and capability notes added in this repository.
