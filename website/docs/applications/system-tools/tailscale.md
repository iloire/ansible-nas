---
title: "Tailscale"
---

Homepage: [https://tailscale.com](https://tailscale.com)

Tailscale is a zero-config VPN that creates a secure network between your devices. It allows you to access your NAS and home network from anywhere without port forwarding or complex firewall rules.

## Usage

Set `tailscale_enabled: true` in your `inventories/<your_inventory>/group_vars/nas.yml` file.

## Prerequisites

1. Create a Tailscale account at [https://tailscale.com](https://tailscale.com)
2. Generate an auth key at [https://login.tailscale.com/admin/settings/keys](https://login.tailscale.com/admin/settings/keys)
   - Use a **Reusable** key for headless server setup
   - Optionally enable **Ephemeral** if you want the device to auto-cleanup when offline

## Specific Configuration

Add to your `nas.yml`:

```yaml
tailscale_enabled: true
tailscale_auth_key: "tskey-auth-XXXXX"  # Your auth key from Tailscale admin

# Optional: Subnet router - access your entire LAN via Tailscale
tailscale_advertise_routes: "192.168.1.0/24"

# Optional: Exit node - route all traffic through your NAS
tailscale_exit_node: true

# Optional: Accept routes from other Tailscale nodes
tailscale_accept_routes: true
```

## Post-Installation

After running the playbook, you need to approve subnet routes and exit node in the Tailscale Admin Console:

1. Go to [https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)
2. Find your NAS and click **Edit route settings**
3. Enable the advertised subnet route (e.g., `192.168.1.0/24`)
4. If configured, enable the exit node

## Features

| Feature | Variable | Description |
|---------|----------|-------------|
| Subnet Router | `tailscale_advertise_routes` | Access your entire LAN through Tailscale |
| Exit Node | `tailscale_exit_node` | Route all internet traffic through your NAS |
| Accept Routes | `tailscale_accept_routes` | See subnets advertised by other nodes |
| Accept DNS | `tailscale_accept_dns` | Use Tailscale's MagicDNS (default: true) |

## Configuration Options

```yaml
# Core settings
tailscale_enabled: false
tailscale_auth_key: ""
tailscale_hostname: "{{ ansible_nas_hostname }}"

# Network features
tailscale_advertise_routes: ""      # e.g., "192.168.1.0/24"
tailscale_exit_node: false
tailscale_accept_routes: false
tailscale_accept_dns: true

# Docker settings
tailscale_container_name: "tailscale"
tailscale_image_name: "tailscale/tailscale"
tailscale_image_version: "latest"
tailscale_data_directory: "{{ docker_home }}/tailscale"
tailscale_memory: 256m
```
