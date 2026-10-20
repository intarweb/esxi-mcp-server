# ESXi MCP Server

A VMware ESXi/vCenter management server based on [MCP (Model Context Protocol)](https://modelcontextprotocol.io), enabling AI assistants like Claude, Gemini, GPT, and others to directly manage virtual machines via stdio JSON-RPC.

## Features

- **stdio transport** for direct AI assistant integration (Claude Desktop, Cursor, Gemini, etc.)
- ESXi and vCenter Server connections
- API key authentication
- Complete virtual machine lifecycle management
- Real-time performance monitoring
- SSL/TLS secure connection support
- Flexible configuration (YAML/JSON/Environment Variables)

## Installation

### Option 1: uvx (Recommended)

```bash
uvx esxi-mcp-server
```

MCP config (`mcp.json`):

```json
{
  "mcpServers": {
    "esxi-mcp-server": {
      "command": "uvx",
      "args": ["esxi-mcp-server"],
      "env": {
        "VCENTER_HOST": "192.168.0.254",
        "VCENTER_USER": "administrator@vsphere.local",
        "VCENTER_PASSWORD": "your-password",
        "VCENTER_INSECURE": "true"
      }
    }
  }
}
```

### Option 2: pip

```bash
pip install esxi-mcp-server
esxi-mcp-server
```

MCP config:

```json
{
  "mcpServers": {
    "esxi-mcp-server": {
      "command": "esxi-mcp-server",
      "env": {
        "VCENTER_HOST": "192.168.0.254",
        "VCENTER_USER": "administrator@vsphere.local",
        "VCENTER_PASSWORD": "your-password",
        "VCENTER_INSECURE": "true"
      }
    }
  }
}
```

### Option 3: Docker Hub

```bash
docker pull tai040502/esxi-mcp-server
```

MCP config:

```json
{
  "mcpServers": {
    "esxi-mcp-server": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "VCENTER_HOST=192.168.0.254",
        "-e", "VCENTER_USER=administrator@vsphere.local",
        "-e", "VCENTER_PASSWORD=your-password",
        "-e", "VCENTER_INSECURE=true",
        "tai040502/esxi-mcp-server"
      ]
    }
  }
}
```

## Available Tools

| Tool | Description |
|------|-------------|
| `authenticate` | Authenticate using API key (required if API key is configured) |
| `list_vms` | List all virtual machines |
| `create_vm` | Create a new virtual machine |
| `clone_vm` | Clone a VM from a template or existing VM |
| `delete_vm` | Delete a virtual machine |
| `power_on_vm` | Power on a virtual machine |
| `power_off_vm` | Power off a virtual machine |
| `get_vm_performance` | Get CPU, memory, storage, and network metrics |

## Configuration

All settings can be configured via environment variables:

| Env Variable | Description | Required | Default |
|-------------|-------------|----------|---------|
| `VCENTER_HOST` | vCenter/ESXi server address | Yes | - |
| `VCENTER_USER` | Login username | Yes | - |
| `VCENTER_PASSWORD` | Login password | Yes | - |
| `VCENTER_DATACENTER` | Datacenter name | No | Auto-select first |
| `VCENTER_CLUSTER` | Cluster name | No | Auto-select first |
| `VCENTER_DATASTORE` | Storage name | No | Auto-select largest |
| `VCENTER_NETWORK` | Network name | No | - |
| `VCENTER_INSECURE` | Skip SSL verification | No | `false` |
| `MCP_API_KEY` | API access key | No | - |
| `MCP_LOG_FILE` | Log file path | No | stderr |
| `MCP_LOG_LEVEL` | Log level | No | `INFO` |

Alternatively, use a YAML or JSON config file:

```bash
esxi-mcp-server -c config.yaml
```

## Requirements

- Python 3.11+
- pyvmomi >= 7.0
- pyyaml >= 6.0
- mcp[cli]

## License

MIT License

## Author

Bright8192
