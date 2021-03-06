CENTOS 7 CIS STIG
================

[![Build Status](https://travis-ci.org/softfactoryio/CENTOS7-CIS.svg?branch=devel)](https://travis-ci.org/softfactoryio/CENTOS7-CIS)


Configure Centos 7 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant. Level 1 and 2 findings will be corrected by default.

This role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

## IMPORTANT INSTALL STEP

If you want to install this via the `ansible-galaxy` command you'll need to run it like this:

`ansible-galaxy install -p roles -r requirements.yml`

With this in the file requirements.yml:

```
- src: https://github.com/softfactoryio/CENTOS7-CIS.git
```

Based on [CIS CentOS Linux 7 Benchmark v2.2.0 - 27-01-2017 ](https://www.cisecurity.org/cis-benchmarks/).

This repo originated from work done by [MindPointGroup](https://github.com/MindPointGroup/RHEL7-CIS) 

Requirements
------------

You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.
If you want to do a dry run without changing anything, set the below sections (centos7cis_section1-6) to false. 

Role Variables
--------------
There are many role variables defined in defaults/main.yml. This list shows the most important.

**centos7cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**centos7cis_section1**: CIS - General Settings (Section 1) (Default: true)

**centos7cis_section2**: CIS - Services settings (Section 2) (Default: true)

**centos7cis_section3**: CIS - Network settings (Section 3) (Default: true)

**centos7cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**centos7cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**centos7cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`centos7cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
centos7cis_avahi_server: false  
centos7cis_cups_server: false  
centos7cis_dhcp_server: false  
centos7cis_ldap_server: false  
centos7cis_telnet_server: false  
centos7cis_nfs_server: false  
centos7cis_rpc_server: false  
centos7cis_ntalk_server: false  
centos7cis_rsyncd_server: false  
centos7cis_tftp_server: false  
centos7cis_rsh_server: false  
centos7cis_nis_server: false  
centos7cis_snmp_server: false  
centos7cis_squid_server: false  
centos7cis_smb_server: false  
centos7cis_dovecot_server: false  
centos7cis_httpd_server: false  
centos7cis_vsftpd_server: false  
centos7cis_named_server: false  
centos7cis_bind: false  
centos7cis_vsftpd: false  
centos7cis_httpd: false  
centos7cis_dovecot: false  
centos7cis_samba: false  
centos7cis_squid: false  
centos7cis_net_snmp: false  
```  

##### Designate server as a Mail server
`centos7cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`centos7cis_is_router: false`  


##### IPv6 required
`centos7cis_ipv6_required: true`  


##### AIDE
`centos7cis_config_aide: true`

###### AIDE cron settings
```
centos7cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`centos7cis_selinux_pol: targeted` 


##### Set to 'true' if X Windows is needed in your environment
`centos7cis_xwindows_required: no` 


##### Client application requirements
```
centos7cis_openldap_clients_required: false 
centos7cis_telnet_required: false 
centos7cis_talk_required: false  
centos7cis_rsh_required: false 
centos7cis_ypbind_required: false 
```

##### Time Synchronization
```
centos7cis_time_synchronization: chrony
centos7cis_time_Synchronization: ntp

centos7cis_time_synchronization_servers:
    - 0.pool.ntp.org
    - 1.pool.ntp.org
    - 2.pool.ntp.org
    - 3.pool.ntp.org  
```  
  
##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
centos7cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
centos7cis_firewall: firewalld
centos7cis_firewall: iptables
``` 
  

Dependencies
------------

Ansible > 2.2

Example Playbook
-------------------------

This sample playbook should be run in a folder that is above the main CENTOS7-CIS / CENTOS7-CIS-devel folder.

```
- name: Harden Server
  hosts: servers
  become: yes

  roles:
    - CENTOS7-CIS
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT
