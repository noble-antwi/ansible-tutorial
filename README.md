# ansible-tutorial

This is a journey towards learning Ansible - a powerful automation platform for infrastructure management and configuration.

## Environment Setup

This lab environment was set up using Vagrant with VirtualBox as the provider.

### Infrastructure (Vagrantfile Configuration)

The environment consists of 4 VMs running Ubuntu 20.04 (Focal):

| Hostname | IP Address | Role | CPUs | RAM | Primary User |
|----------|------------|------|------|-----|--------------|
| ansible-controller | 192.168.56.20 | Control Node | 2 | 2GB | ansible |
| web-server | 192.168.56.21 | Web Server | 1 | 1GB | webadmin |
| db-server | 192.168.56.22 | Database Server | 2 | 2GB | dba |
| app-server | 192.168.56.23 | Application Server | 2 | 1.5GB | appdev |

### Vagrant Setup

1. **Initialize the environment**:
   ```bash
   vagrant up
   ```

2. **Key features configured in Vagrantfile**:
   - Private network (192.168.56.0/24) for inter-VM communication
   - Custom users created with sudo privileges (no password required)
   - `ansible` user created on all managed nodes for automation
   - Password authentication enabled for SSH (for initial setup)
   - Essential packages installed: git, vim, curl, wget, net-tools, openssh
   - Ansible automatically installed on the controller node

### SSH Key Configuration

1. Generated SSH key pair on the controller: `ansible-controller` (ed25519)
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/ansible-controller -C "Learning Ansible"
   ```

2. Copied public key to managed nodes:
   ```bash
   ssh-copy-id -i ~/.ssh/ansible-controller.pub 192.168.56.21
   ssh-copy-id -i ~/.ssh/ansible-controller.pub 192.168.56.22
   ssh-copy-id -i ~/.ssh/ansible-controller.pub 192.168.56.23
   ```

3. Configured SSH client (`~/.ssh/config`):
   ```
   Host 192.168.56.*
       IdentityFile ~/.ssh/ansible-controller
       User ansible
   ```

### GitHub SSH Key

Generated separate SSH key for GitHub operations:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/learning-ansible-github
```

### Inventory

The [inventory](inventory) file contains the IP addresses of all managed nodes:
- 192.168.56.21 (web-server)
- 192.168.56.22 (db-server)
- 192.168.56.23 (app-server)