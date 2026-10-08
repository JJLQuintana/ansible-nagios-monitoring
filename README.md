# Ansible Nagios Monitoring

Ansible role that installs Nagios Core on both Ubuntu and CentOS servers, as part of a lab on availability monitoring with Infrastructure as Code.

## What this covers
- Installing Nagios with a role-based playbook
- Distribution-aware package installs (`nagios4` on Ubuntu, `nagios` on CentOS)
- Running system updates in `pre_tasks` before the role
- Verifying the install from the CLI and the Nagios web interface

## Lab environment
- Control node: Ubuntu workstation running Ansible
- Managed nodes: one Ubuntu server and one CentOS server (VirtualBox, host-only network)

## Repository structure
```
.
├── ansible.cfg
├── inventory
├── site.yml
└── roles/
    └── remote_server/
        └── tasks/
            └── main.yml
```

## How it works
`site.yml` runs two plays against all hosts:
1. `pre_tasks` update the package index and installed packages (`dnf` on CentOS, `apt` on Ubuntu).
2. The `remote_server` role installs Nagios, with `when: ansible_distribution == ...` selecting the right package manager and package name.

## Usage
```bash
ansible-playbook --ask-become-pass site.yml
```

## Verification
| Host | Command | Result |
|------|---------|--------|
| Ubuntu | `nagios4 --version` | Nagios Core 4.4.6 |
| CentOS | `nagios --version` | Nagios Core 4.4.9 |

On Ubuntu the web interface loaded at `/nagios4/`. On CentOS, `/nagios/` prompted for a login.

## Notes and next steps
- The role installs the package only. Starting and enabling the `nagios` and web server services should be added with the `service` module.
- Web interface credentials (htpasswd) are not provisioned by the playbook.
- Monitoring targets and check configuration are not yet defined.
