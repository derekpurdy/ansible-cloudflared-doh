# Cloudflared DNS over HTTPS

Installation for Cloudflared DOH. This is an ansible playbook of the documentation found at https://docs.pi-hole.net/guides/dns/cloudflared/#configuring-dns-over-https

Performs the following actions:

1. Downloads the cloudflared binary for the appropriate architecture.
2. Installs the cloudflared binary.
3. Creates the cloudflared user and group.
4. Configures the cloudflared service.
5. Starts and enables the cloudflared service.

## Ansible Galaxy

https://galaxy.ansible.com/ui/standalone/roles/derekpurdy/cloudflared_doh

## Example Playbook

```yaml
    ---

    - hosts:
      - pi

      roles:
        - role: derekpurdy.cloudflared_doh
          vars:
            cloudflared_use_enviorment_file: false # to use config file instead of enviorment variables
            cloudflared_service_name: "cloudflared-proxy.service" # to change the service name to cloudflared-proxy
            cloudflared_service_file: "/etc/systemd/system/cloudflared-proxy.service"
          become: true
```

### Unintallation

```yaml
    ---

    - hosts:
      - pi
      # need to import the uninstall tasks and specify the variables as the role defaults will not be available
      tasks:
        - name: Uninstall cloudflared
          ansible.builtin.import_tasks: roles/cloudflared_doh/tasks/uninstall.yml
          vars:
            cloudflared_config_file: "/opt/cloudflared/config.yml"
            cloudflared_enviorment_file: "/etc/defaults/cloudflared"
            cloudflared_bin_location: "/usr/local/bin/cloudflared"
            cloudflared_service_file: "/etc/systemd/system/cloudflared-proxy.service"
            cloudflared_service_name: "cloudflared-proxy.service"
            cloudflared_user: cloudflared
            cloudflared_group: cloudflared
          tags: uninstall
```

## Role Variables

```yaml
# User and group under which the cloudflared service will run
cloudflared_user: cloudflared
cloudflared_group: cloudflared

# Name of the cloudflared service
cloudflared_service_name: "cloudflared.service"

# Whether to use an environment file for configuration
cloudflared_use_enviorment_file: true # if false, use config file

# Path to the cloudflared configuration file
cloudflared_config_file: "/opt/cloudflared/config.yml"

# Path to the cloudflared environment file
cloudflared_enviorment_file: "/etc/defaults/cloudflared"

# Location of the cloudflared binary
cloudflared_bin_location: "/usr/local/bin/cloudflared"

# Path to the cloudflared service file
cloudflared_service_file: "/etc/systemd/system/cloudflared.service"

# Time in seconds to wait before restarting the service
cloudflared_service_restart_sec: 10

# Enable or disable DNS proxy
cloudflared_proxy_dns: true

# Address on which cloudflared will listen
cloudflared_address: 0.0.0.0

# Port on which cloudflared will listen
cloudflared_port: 5053

# Upstream DNS servers to use
# could be a list of URLs of any DoH server (see https://dnscrypt.info/public-servers/)
cloudflared_upstream_dns:
  - "https://1.1.1.1/dns-query"
  - "https://1.0.0.1/dns-query"

# Enable or disable metrics
cloudflared_enable_metrics: true

# IP address for metrics listening
cloudflared_metrics_listen_ip: 0.0.0.0

# Port for metrics listening
cloudflared_metrics_listen_port: 49312

# URLs for downloading cloudflared binaries for different architectures
cloudflared_url_deb: "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb"
cloudflared_url_arm: "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm"
cloudflared_url_arm64: "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64"
cloudflared_url_rpm: "https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-x86_64.rpm"

# Disable automatic updates
cloudflared_no_autoupdate: true

# Protocol to use for communication
cloudflared_protocol: "http2"
```

## Testing the cloudflared service

```bash
# Check the status of the cloudflared service
sudo systemctl status cloudflared # or cloudflared-proxy

dig @localhost -p 5053 google.com
```

## Author Information

This role was created by Derek Purdy
