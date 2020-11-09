Role Name
===========
This is a role that runs the pre-patch checks on RHEL servers that are to be patched which is a requirement before the systems are rebooted and then the post-patch checks kicks in:

1- It runs pre-checks like to check for available disk space on some selected mounted FS, OS distribution and version, mounted filesystems, register some default system services but no custom services check specified yet, system interface check is also part of what the pre-patch checks as well as check SELinux status, etc).

2- For the disk space checks, a threshold is set for some mount points that are likely to be used up during system patch like -

- /
- /var
- /boot 
- /tmp

A condition is set in form of threshold set which if it passes,the server will be patched but if not, it will fail the play and abort the patching of the server and notification email is generated and sent to the team/individual that oversees the patching project.

3- Before the actual pre-checks kicks in, patching time is set and afterwards the pre-check, will check the uptime which will also be part of the post-patch checks in order to ascertain that system reboot was actually initiated.

3- As part of the pre-checks, all the systems that will be patched are all checked to make sure that are RHEL servers and the play will fail if they ain't. A report is generated for this and sent to a delegated server.

5- Another thing the pre-patch check will do is to collect list of nonexistent filesystems that might be specified in fstab file. The main idea is to prevent a situation where a filesystem that doesn't exist but is still listed in the fstab file. The playbook will fail if a filesystem that does not exist but is in the fstab file and this has to be addressed before the playbook will continue.

Requirements
------------
These playbooks were designed to be used in Linux system (RHEL and maybe CentOS), which fails on non-RHEL servers. 

Role Variables
--------------
The variables that can be passed to this role are as follows:

supported_distros:
  - RedHat
  - CentOS

ip_address: "{{ ansible_default_ipv4.address }}"
patching_type: SYSTEM_PATCHING
local_dir: "/tmp/ansible/sys-patching"

local_dir is a directory where to store the logs during the time the playbook is running. Default: /tmp/ansible/sys-patching:
local_dir: "/tmp/ansible/sys-patching"
NB: The logs generated can be delegated to the ansible controller or the managed nodes.

Example Playbook
----------------
---
- name: patching the linux systems using patch-automation roles
  hosts: dev
  gather_facts: True
  remote_user: toweragent
  become: yes
  vars:
    patching_type: "SYSTEM_UPDATE"
  roles:
    - ansible-linux-patch-automation

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
