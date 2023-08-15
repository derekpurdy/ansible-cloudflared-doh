# Cloudflared DNS over HTTPS
Installation for Cloudflared DOH on Raspberry Pi OS

## Example Playbook

    ---

    - hosts:
      - all

      roles:
          - role: derekpurdy.cloudflared-doh
            become: true
