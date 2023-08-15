# Cloudflared DNS over HTTPS
Installation for Cloudflared DOH on Raspberry Pi OS

## Example Playbook

    ---

    - hosts:
      - pi

      roles:
        - role: derekpurdy.cloudflared_doh
          become: true

## Author Information
This role was created by Derek Purdy
