Shorewall
=========

Ansible role which installs and configures shorewall and shorewall6.

Role Variables
--------------

```yaml
shorewall: True
shorewall6: False

shorewall_interfaces:
  - { zone: net, interface: eth0, options: "dhcp,tcpflags,logmartians,nosmurfs,sourceroute=0" }

shorewall_policies:
  - { source: "$FW", dest: all, policy: ACCEPT }
  - { source: net, dest: all, policy: REJECT }
  - { source: all, dest: all, policy: REJECT, log_level: info }

shorewall_rules:
  - section: NEW
    rules:
    - { action: "Invalid(DROP)", source: net, dest: "$FW", proto: tcp }
    - { action: ACCEPT, source: net, dest: "$FW", proto: tcp, dest_port: ssh }
    - { action: ACCEPT, source: net, dest: "$FW", proto: icmp, dest_port: echo-request }
    shorewall_custom_rules:
      github:
      - { rule: 'ACCEPT', src: 'fw', dst: 'net:github.com', proto: 'tcp', dport: '22,443', comment: 'Allow access to GitHub' }

shorewall_zones:
  - { zone: fw, type: firewall }
  - { zone: net, type: ipv4 }
```

Example Playbook
----------------

    - hosts: all
      roles:
         - ansible-shorewall

License
-------

MIT

Author Information
------------------

* Jani Karlsson <jani@karlsson.re> (added EL support, updated for Shorewall5 configs + custom application-specific rules support)
* Farhad Shahbazi
* Sascha Biberhofer

Original project: https://github.com/SphericalElephant/ansible-shorewall
