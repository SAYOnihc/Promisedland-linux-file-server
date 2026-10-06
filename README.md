# PromisedLand Linux File Server

A secure Linux file server using **Samba, LVM, ACLs, UFW, Fail2Ban, and SSH hardening** for a 38-user organization.

## Overview

This project involved designing and configuring a Linux-based file server for a fictional organization, PromisedLand, expanding its infrastructure in Independence, Missouri.

The server was built with **Ubuntu Server 22.04 LTS** and provides centralized file storage and controlled access for four departments:

* Operations
* Applications
* CRM
* Finance

The environment uses Linux groups, permissions, ACLs, and Samba to provide secure, department-based access to shared resources.

## Project Goals

* Centralize departmental file storage
* Restrict users to their appropriate department resources
* Provide read-only executive access across departments
* Use LVM for scalable storage management
* Provide Windows-compatible file sharing through Samba
* Implement layered security controls
* Automate user and group creation

## Architecture

The server uses two virtual disks:

```text
Disk 1
└── Ubuntu Server OS
    └── / (ext4)

Disk 2
└── LVM
    └── Volume Group: vg_data
        ├── lv_operations
        ├── lv_applications
        ├── lv_crm
        └── lv_finance
```

Shared directories are organized under `/srv`:

```text
/srv/
├── operations/
├── applications/
├── crm/
└── finance/
```

## Technologies

| Technology              | Purpose                         |
| ----------------------- | ------------------------------- |
| Ubuntu Server 22.04 LTS | Server operating system         |
| Samba                   | SMB/CIFS network file sharing   |
| LVM                     | Flexible storage management     |
| Linux Groups            | Department-based access control |
| POSIX ACLs              | Granular permissions            |
| UFW                     | Host-based firewall             |
| Fail2Ban                | Brute-force protection          |
| OpenSSH                 | Secure remote administration    |
| Netplan                 | Network configuration           |
| Bash                    | User and group automation       |

## Users & Groups

A total of **38 users** were created based on the organization's structure.

Department groups:

```text
operations
applications
crm
finance
exec_read
```

Users were created through a Bash script to improve consistency and reduce repetitive manual configuration.

Each department's users are restricted to their corresponding shared directory.

An executive account, `cshumaker`, was configured with read-only access across the shared directories using ACLs.

## Storage Design

The operating system is separated from the departmental data storage.

The second virtual disk uses LVM with the following logical volumes:

```text
vg_data
├── lv_operations
├── lv_applications
├── lv_crm
└── lv_finance
```

This structure allows departmental storage to be managed independently and provides flexibility for future expansion.

## File Permissions

Department directories are protected using Linux groups and ACLs.

Example ACL configuration:

```bash
sudo setfacl -m u:cshumaker:rx /srv/operations
sudo setfacl -m u:cshumaker:rx /srv/applications
sudo setfacl -m u:cshumaker:rx /srv/crm
sudo setfacl -m u:cshumaker:rx /srv/finance
```

Samba was configured to provide authenticated network access to the shared directories.

## Security

### UFW

Configured the firewall to allow only required services, including SSH and Samba.

### Fail2Ban

Configured Fail2Ban to help protect against repeated authentication attempts and brute-force attacks.

### SSH Hardening

Disabled direct root login and restricted remote administration access.

### Static Networking

Configured a static IP address using Netplan:

```text
192.168.100.99
```

### System Updates

System packages were maintained using:

```bash
sudo apt update
sudo apt upgrade
```

## Automation

A Bash script was created to automate user creation and group assignment based on the organization's structure.

This reduced manual configuration and helped ensure consistent user permissions.

## Skills Demonstrated

* Linux server administration
* Samba configuration
* Linux permissions and ACLs
* LVM storage management
* User and group administration
* Bash scripting
* SSH security
* Firewall configuration
* Fail2Ban
* Network configuration
* Infrastructure design

## Project Demonstration

[![Watch the Project Demonstration](https://img.youtube.com/vi/wQaYPyuvUEA/maxresdefault.jpg)](https://youtu.be/wQaYPyuvUEA)

[Watch the full project demonstration on YouTube](https://youtu.be/wQaYPyuvUEA)

## Documentation

Additional project documentation, architecture diagrams, and screenshots can be organized under:

```text
docs/
├── architecture/
├── storage/
└── screenshots/
```

## Key Takeaway

This project demonstrates my ability to translate organizational requirements into a **secure, structured, and scalable Linux server environment** using industry-relevant systems administration tools and practices.
