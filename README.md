# Ansible in Abyss

An ansible playbook for setting up and maintaining our servers, including docker-compose configuration for our internal tools. We set up SSL encryption as well as oauth via GitHub for accessing internal services.

## Service stack

- `Minecraft` We use our own [Docker images](https://github.com/MineInAbyss/Docker) for our Minecraft servers
- `Portainer` to deploy docker-compose stacks and manage server console
- `filebrowser` for managing server files and more granular permission management
- `Traefik` as a docker-friendly reverse proxy
- `OAuth2 proxy` for authentication via GitHub on our internal subdomain

## Usage

Set up a machine running Debian, have root SSH access.

Install ansible on local machine and cd into this repo.

Run `ansible-galaxy install -r requirements.yml`

Set `ANSIBLE_PRIVATE` to point to a path with `vault.yml` inside it for secrets as defined in `secrets-def.yml`.

Set up inventory (playbook will run on a host/group named `rootserver`) and vault password in a system ansible.cgf file (ex in `/etc/ansible/ansible.cfg`):

```toml
[defaults]
vault_password_file=/path/to/vault_pass
inventory=/path/to/inventory.ini
```

Run `ansible-playbook run.yml`, it will find the vault file on its own, and decrypt it thanks to the ansible config.

### Manual steps

We currently don't automate deploying stacks in portainer, so deploy manually as needed from the `portainer_stacks` folder. These should be manually kept up to date as we make changes in portainer.

Point DNS to this machine, specifically internal subdomain and wildcard *.<internal subdomain>, and all our Minecraft server subdomains.

## Thanks
- https://github.com/notthebee/infra for general setup with our ansible roles
- [Self-hosting SSO with Traefik (Part 2): OAuth2 Proxy](https://joeeey.com/blog/selfhosting-sso-with-traefik-oauth2-proxy-part-2/#option-b-automatic-redirect-to-keycloak) for getting OAuth2 proxy working
- [Jeff Geerling](https://www.jeffgeerling.com/) for great Ansible tutorials and roles
