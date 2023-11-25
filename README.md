# Self-hosted domain template
This is an Ansible-based Action to deploy the bare minimum of a Docker-based, self-hosted domain. It is currently geared at Debian-based systems, with Cloudflare as a DNS provider, but can easily be adapted.
It uses:

- [Traefik](https://github.com/traefik/traefik) as a reverse-proxy
- [DDCLIENT](https://github.com/ddclient/ddclient) as a dynamic-DNS update service
- [Prometheus](https://github.com/prometheus/prometheus) to monitor the health of the domain

This repository consists of three core tasks:
- `setup-domain.yaml` - the main setup task. It calls the other tasks to setup Docker, followed by DDCLIENT and Prometheus, and sets up Traefik.
- `setup-docker.yaml` - Install Docker CE
- `setup-containers.yaml` - Clones templates of DDCLIENT and  Prometheus (different branches of this repository), and configures them to use your domain (`{{ DOMAIN }}` and your domain host's IP.

Once Docker is set up and containers are ready, `setup-domain.yaml` configures Traefik using your Cloudflare API credentials.  
- `./gitea/workflows/setup-domain.yaml` - Is the CI workflow which installs Ansible, and runs the `setup-domain.yaml` playbook to configure your domain.

# Usage
This template relies on several secrets:
- Cloudflare API key `{{ CF_API_KEY }}`)
- Cloudflare API email (`{{ CF_API_EMAIL }}`)
- Cloudflare API token (`{{ CF_API_TOKEN }}`)
- A valid SSH key for Ansible to use to connect to target systems (`{{ SSH_PRIVATE_KEY }}`)
- A Sudo password (`{{ SUDO_PASS }}`)
- If cloning your Ansible inventory from elsewhere (as I do), an access token (`{{ ANSIBLE_TOKEN }}`) to that repository
