Role Name
=========
1- This role runs the post-patch checks which ideally rechecks most of the things that were checked during the pre-patch checks. Before the post-patch checks kicks in.

2- After the pre-patch checks, the update task is expected to run which will update all the RHEL servers, then if the yum update succeeds, the target servers are rebooted with the reboot task.

3- After the servers are rebooted, the play waits for a specified time to give some application time to start up again and for ssh to become available and after which the post-patch checks kicks in.

4- After the post-patch checks runs successfully, a report is generated which can be delegated to a server of choice. For the purpose of this project, the generated report was delegated to xlab2359.aetna.com and a snippet of the report is shown under the "Expected Result" section.

Requirements
------------
These playbooks were designed to be used in Linux system (RHEL and maybe CentOS). 

Role Variables
--------------
The variables that can be passed to this role are as follows:

*REBOOT VARIABLES*
server_update_reboot_connect_timeout: 5
server_update_reboot_pre_reboot_delay: 30
server_update_reboot_post_reboot_delay: 100
server_update_reboot_reboot_timeout: 600
NB: The values for these variables can be adjusted if need be.

supported_distros:  
  - RedHat
  - CentOS
local_dir is a directory where to store the logs during the time the playbook is running. Default: /tmp/ansible/sys-patching:
local_dir: "/tmp/ansible/sys-patching"
NB: The logs generated can be delegated to the ansible controller or any other server and for the purpose of this patching, logs generated are delegated to xlab2359.aetna.com.

ip_address: "{{ ansible_default_ipv4.address }}"
Patching_type: SYSTEM_PATCH --> Full patch with system reboot (with or without kernel update - unconditional reboot)
vastool_status: "vas_status.stdout.split('\n')[4].split(':')"
vastool_path: "/opt/quest/bin/vastool"
server_update_yum_install_pkgs: '*'
server_update_yum_exclude_pkgs: []
vastool_status: "vas_status.stdout.split('\n')[4].split(':')"
vastool_path: "/opt/quest/bin/vastool"
servicename: vastool

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

Expected Result
---------------

"HOSTNAME: xlab2359.aetnat.com version before the update:"
"      distribution: RedHat "
"      distribution_major_version: 7"
"      distribution_release: Maipo"
"      distribution_version: 7.7"

"HOSTNAME: xlab2359.aetnat.com version after the update:"
"      distribution: RedHat "
"      distribution_major_version: 7"
"      distribution_release: Maipo"
"      distribution_version: 7.7"

HOSTNAME: xlab2359.aetnat.com | rngd.service : OK
HOSTNAME: xlab2359.aetnat.com | vasd.service : OK
HOSTNAME: xlab2359.aetnat.com | atd.service : OK
HOSTNAME: xlab2359.aetnat.com | getty@tty1.service : OK
HOSTNAME: xlab2359.aetnat.com | nscd.service : OK
HOSTNAME: xlab2359.aetnat.com | vgauthd.service : OK
HOSTNAME: xlab2359.aetnat.com | libstoragemgmt.service : OK
HOSTNAME: xlab2359.aetnat.com | network : OK
HOSTNAME: xlab2359.aetnat.com | vmtoolsd.service : OK
HOSTNAME: xlab2359.aetnat.com | rsyslog.service : OK
HOSTNAME: xlab2359.aetnat.com | tuned.service : OK
HOSTNAME: xlab2359.aetnat.com | goferd.service : OK
HOSTNAME: xlab2359.aetnat.com | abrt-oops.service : OK
HOSTNAME: xlab2359.aetnat.com | dbus.service : OK
HOSTNAME: xlab2359.aetnat.com | systemd-logind.service : OK
HOSTNAME: xlab2359.aetnat.com | lvm2-lvmetad.service : OK
HOSTNAME: xlab2359.aetnat.com | rhsmcertd.service : OK
HOSTNAME: xlab2359.aetnat.com | crond.service : OK
HOSTNAME: xlab2359.aetnat.com | abrtd.service : OK
HOSTNAME: xlab2359.aetnat.com | gssproxy.service : OK
HOSTNAME: xlab2359.aetnat.com | polkit.service : OK
HOSTNAME: xlab2359.aetnat.com | chronyd.service : OK
HOSTNAME: xlab2359.aetnat.com | xinetd.service : OK
HOSTNAME: xlab2359.aetnat.com | postfix.service : OK
HOSTNAME: xlab2359.aetnat.com | systemd-udevd.service : OK
HOSTNAME: xlab2359.aetnat.com | sshd.service : OK
HOSTNAME: xlab2359.aetnat.com | systemd-journald.service : OK
HOSTNAME: xlab2359.aetnat.com | irqbalance.service : OK
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-root      8.0G  2.2G  5.9G  28% / : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/sda1                   1014M  214M  801M  22% /boot : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-var       8.0G  3.2G  4.9G  39% /var : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-home      8.0G  449M  7.6G   6% /home : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/uservg-u01        20G   33M   20G   1% /u01 : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-opt       8.0G   70M  8.0G   1% /opt : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-tmp       8.0G   34M  8.0G   1% /tmp : MOUNTED
HOSTNAME: xlab2359.aetnat.com | /dev/mapper/rootvg-midrange  8.0G  331M  7.7G   5% /midrange : MOUNTED
HOSTNAME: xlab2359.aetnat.com | interface eth0 : OK
HOSTNAME: xlab2359.aetnat.com | interface eth0 : 10.80.148.147 : OK
HOSTNAME: xlab2359.aetnat.com | SELinux: OK

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
