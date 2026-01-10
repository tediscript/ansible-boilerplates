# Ansible Server Setup

Automated server provisioning with Ansible. Creates a user, configures SSH access, installs Docker, and fixes locale issues for macOS connections.

## Quick Start

### 1. Configure Inventory

Copy the example inventory file and add your servers:

```bash
cp .inventory.ini.example .inventory.ini
nano .inventory.ini
```

Add your server(s):

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

