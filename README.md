# Ansible Debian Server Setup

Automated Debian server provisioning with Ansible. Creates a user, configures SSH access, installs Docker, and fixes locale issues for macOS connections.

## Quick Start

### 1. Configure Inventory

Copy the example inventory file and add your servers:

```bash
cp .inventory.ini.example .inventory.ini
nano .inventory.ini
```

Add your Debian server(s):

```ini
[debian_servers]
my-server ansible_host=192.168.1.100 ansible_user=root ansible_port=22
```

### 2. Add Your Public Keys

Copy your SSH public key(s) to the `keys/` directory:

```bash
cp ~/.ssh/id_rsa.pub keys/
```

### 3. Test Connection

```bash
ansible-playbook test-conn.yml
```

### 4. Run the Setup Playbook

```bash
ansible-playbook setup-debian.yml
```

For verbose output (debugging):

```bash
ansible-playbook setup-debian.yml -v

## Playbooks

- **test-conn.yml** - Test SSH connectivity and display system information
- **setup-debian.yml** - Main provisioning playbook (creates user, configures SSH, fixes locale)
- **setup-docker.yml** - Docker installation playbook (installs Docker CE from official repository)
```

## What This Playbook Does

1. **Fixes Locale Issues** - Installs and configures `en_US.UTF-8` locale (fixes macOS SSH warnings)
2. **Creates debian User** - Creates a `debian` user with home directory at `/home/debian`
3. **Configures Sudo** - Grants passwordless sudo access to `debian` user
4. **Deploys SSH Keys** - Copies all `.pub` files from `keys/` to `/home/debian/.ssh/authorized_keys`

## After Deployment

Once the playbook completes, you can SSH into your server as the `debian` user:

```bash
ssh debian@your-server-ip
```

Test Docker access:

```bash
docker ps
docker run hello-world
```
    # This file
├── ansible.cfg                # Ansible configuration
├── .inventory.ini             # Your inventory (gitignored)
├── setup-debian.yml           # Main provisioning playbook
├── setup-docker.yml           # Docker installation playbook
├── test-conn.yml              # Connection test playbook
└── keys/                      # SSH public keys directory
    └── *.pub                  # Your public keys here
```

## Security Notes

- **Never commit private keys** to this repository (only `.pub` files)
- `.inventory.ini` is gitignored to protect server IPs
```

## Security Notes

- **Never commit private keys** to this repository (only `.pub` files)
- The playbook runs as `root` initially to c

## Troubleshooting

### Connection Issues

```bash
# Test basic connectivity
ansible debian_servers -m ping

# Check if host is reachable
ssh root@your-server-ip

# Run with maximum verbosity
ansible-playbook test-conn.yml -vvvv
```

### Locale Warnings

The playbook automatically fixes locale warnings when connecting from macOS. If you still see warnings, ensure the playbook completed successfully.reate the `debian` user
- The `debian` user will have passwordless sudo access
- All public keys in `keys/` directory will have access to the `debian` account
- Recommend disabling root SSH login after initial setup

## Requirements

- Ansible installed on your local machine (`brew install ansible` on macOS)
- Root SSH access to target Debian server(s)
- SSH public key(s) in the `keys/` directory
