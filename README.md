# Cloudflared DNS over HTTPS
Installation for Cloudflared DOH on Raspberry Pi OS. This is an ansible playbook of the documentation found at https://docs.pi-hole.net/guides/dns/cloudflared/#configuring-dns-over-https

# Ansible Galaxy
https://galaxy.ansible.com/ui/standalone/roles/derekpurdy/cloudflared_doh

## Example Playbook

    ---

    - hosts:
      - pi

      roles:
        - role: derekpurdy.cloudflared_doh
          become: true

## Author Information
This role was created by Derek Purdy
