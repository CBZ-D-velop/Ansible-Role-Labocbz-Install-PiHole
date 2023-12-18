# Ansible role: labocbz.install_pihole

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: PiHole](https://img.shields.io/badge/Tech-PiHole-orange)
![Tag: DNS](https://img.shields.io/badge/Tech-DNS-orange)

An Ansible role to install and configure PiHole on your host.

This Ansible role effortlessly installs the latest version of Pi-hole using the official installation script. This role offers extensive configurability, including IP settings, network interfaces, and the option to install the web version.

Pi-hole's power is further harnessed through the deployment of AAAA records and CNAMES. This simplifies and streamlines the DNS resolution process. Additionally, the role empowers you to bolster your online experience by integrating various blocklists targeting advertisements and malicious websites. This fortifies your network against unwanted content and potential threats.

In essence, this Ansible role not only brings the latest version of Pi-hole to your system but also provides a comprehensive suite of configurable options. From network settings to DNS resolution enhancements and built-in blocklists, this role simplifies and enhances your network's management and security.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_pihole_webpassword: "admin"
install_pihole_interface: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['interface'] }}"
install_pihole_ipv4_address: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['address'] }}/{{ (hostvars[ansible_hostname]['ansible_default_ipv4']['address'] + '/' + hostvars[ansible_hostname]['ansible_default_ipv4']['netmask']) | ipaddr('prefix') }}"
install_pihole_query_logging: true
install_pihole_install_web: true
install_pihole_dnsmasq_listening: "single"
install_pihole_dns_1: "1.1.1.1"
install_pihole_dns_2: "1.0.0.1"
install_pihole_dns_fqdn_required: true
install_pihole_dns_bogus_priv: true
install_pihole_dnssec: true
install_pihole_temperatureunit: "C"
install_pihole_webuiboxedlayout: "traditional"
install_pihole_api_exclude_domains: ""
install_pihole_api_exclude_clients: ""
install_pihole_api_query_logs_show: "all"
install_pihole_api_privacy_mode: false

install_pihole_aaaa_list:
  - domain: "{{ ansible_hostname }}"
    ip: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['address'] }}"

install_pihole_cname_list:
  - domain: "pihole.local"
    target: "{{ ansible_hostname }}"

install_pihole_adlists:
  - "https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt"
  - "https://adaway.org/hosts.txt"
  - "https://v.firebog.net/hosts/AdguardDNS.txt"
  - "https://v.firebog.net/hosts/Admiral.txt"
  - "https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt"
  - "https://s3.amazonaws.com/lists.disconnect.me/simple_ad.txt"
  - "https://v.firebog.net/hosts/Easylist.txt"
  - "https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts"
  - "https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts"
  - "https://v.firebog.net/hosts/Easyprivacy.txt"
  - "https://v.firebog.net/hosts/Prigent-Ads.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts"
  - "https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt"
  - "https://hostfiles.frogeye.fr/firstparty-trackers-hosts.txt"
  - "https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt"
  - "https://osint.digitalside.it/Threat-Intel/lists/latestdomains.txt"
  - "https://v.firebog.net/hosts/Prigent-Crypto.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts"
  - "https://bitbucket.org/ethanr/dns-blacklists/raw/8575c9f96e5b4a1308f2f12394abd86d0927a4a0/bad_lists/Mandiant_APT1_Report_Appendix_D.txt"
  - "https://phishing.army/download/phishing_army_blocklist_extended.txt"
  - "https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt"
  - "https://v.firebog.net/hosts/RPiList-Malware.txt"
  - "https://v.firebog.net/hosts/RPiList-Phishing.txt"
  - "https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt"
  - "https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts"
  - "https://urlhaus.abuse.ch/downloads/hostfile/"
  - "https://zerodot1.gitlab.io/CoinBlockerLists/hosts_browser"
  - "https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts"
  - "https://v.firebog.net/hosts/static/w3kbl.txt"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_pihole_webpassword: "admin"
inv_install_pihole_interface: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['interface'] }}"
inv_install_pihole_ipv4_address: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['address'] }}/{{ hostvars[ansible_hostname]['ansible_default_ipv4']['netmask'] }}"
inv_install_pihole_query_logging: true
inv_install_pihole_install_web: true
inv_install_pihole_dnsmasq_listening: "single"
inv_install_pihole_dns_1: "1.1.1.1"
inv_install_pihole_dns_2: "1.0.0.1"
inv_install_pihole_dns_fqdn_required: true
inv_install_pihole_dns_bogus_priv: true
inv_install_pihole_dnssec: true
inv_install_pihole_temperatureunit: "C"
inv_install_pihole_webuiboxedlayout: "traditional"
inv_install_pihole_api_exclude_domains: ""
inv_install_pihole_api_exclude_clients: ""
inv_install_pihole_api_query_logs_show: "all"
inv_install_pihole_api_privacy_mode: false

inv_install_pihole_aaaa_list:
  - domain: "{{ ansible_hostname }}"
    ip: "{{ hostvars[ansible_hostname]['ansible_default_ipv4']['address'] }}"

inv_install_pihole_cname_list:
  - domain: "pihole.local"
    target: "{{ ansible_hostname }}"

inv_install_pihole_adlists:
  - "https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt"
  - "https://adaway.org/hosts.txt"
  - "https://v.firebog.net/hosts/AdguardDNS.txt"
  - "https://v.firebog.net/hosts/Admiral.txt"
  - "https://s3.amazonaws.com/lists.disconnect.me/simple_malvertising.txt"
  - "https://s3.amazonaws.com/lists.disconnect.me/simple_ad.txt"
  - "https://v.firebog.net/hosts/Easylist.txt"
  - "https://pgl.yoyo.org/adservers/serverlist.php?hostformat=hosts&showintro=0&mimetype=plaintext"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/UncheckyAds/hosts"
  - "https://raw.githubusercontent.com/bigdargon/hostsVN/master/hosts"
  - "https://v.firebog.net/hosts/Easyprivacy.txt"
  - "https://v.firebog.net/hosts/Prigent-Ads.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts"
  - "https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt"
  - "https://hostfiles.frogeye.fr/firstparty-trackers-hosts.txt"
  - "https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareHosts.txt"
  - "https://osint.digitalside.it/Threat-Intel/lists/latestdomains.txt"
  - "https://v.firebog.net/hosts/Prigent-Crypto.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Risk/hosts"
  - "https://bitbucket.org/ethanr/dns-blacklists/raw/8575c9f96e5b4a1308f2f12394abd86d0927a4a0/bad_lists/Mandiant_APT1_Report_Appendix_D.txt"
  - "https://phishing.army/download/phishing_army_blocklist_extended.txt"
  - "https://gitlab.com/quidsup/notrack-blocklists/raw/master/notrack-malware.txt"
  - "https://v.firebog.net/hosts/RPiList-Malware.txt"
  - "https://v.firebog.net/hosts/RPiList-Phishing.txt"
  - "https://raw.githubusercontent.com/Spam404/lists/master/main-blacklist.txt"
  - "https://raw.githubusercontent.com/AssoEchap/stalkerware-indicators/master/generated/hosts"
  - "https://urlhaus.abuse.ch/downloads/hostfile/"
  - "https://zerodot1.gitlab.io/CoinBlockerLists/hosts_browser"
  - "https://raw.githubusercontent.com/PolishFiltersTeam/KADhosts/master/KADhosts.txt"
  - "https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.Spam/hosts"
  - "https://v.firebog.net/hosts/static/w3kbl.txt"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_pihole"
  tags:
    - "labocbz.install_pihole"
  vars:
    install_pihole_webpassword: "{{ inv_install_pihole_webpassword }}"
    install_pihole_interface: "{{ inv_install_pihole_interface }}"
    install_pihole_ipv4_address: "{{ inv_install_pihole_ipv4_address }}"
    install_pihole_query_logging: "{{ inv_install_pihole_query_logging }}"
    install_pihole_install_web: "{{ inv_install_pihole_install_web }}"
    install_pihole_dnsmasq_listening: "{{ inv_install_pihole_dnsmasq_listening }}"
    install_pihole_dns_1: "{{ inv_install_pihole_dns_1 }}"
    install_pihole_dns_2: "{{ inv_install_pihole_dns_2 }}"
    install_pihole_dns_fqdn_required: "{{ inv_install_pihole_dns_fqdn_required }}"
    install_pihole_dns_bogus_priv: "{{ inv_install_pihole_dns_bogus_priv }}"
    install_pihole_dnssec: "{{ inv_install_pihole_dnssec }}"
    install_pihole_temperatureunit: "{{ inv_install_pihole_temperatureunit }}"
    install_pihole_webuiboxedlayout: "{{ inv_install_pihole_webuiboxedlayout }}"
    install_pihole_api_exclude_domains: "{{ inv_install_pihole_api_exclude_domains }}"
    install_pihole_api_exclude_clients: "{{ inv_install_pihole_api_exclude_clients }}"
    install_pihole_api_query_logs_show: "{{ inv_install_pihole_api_query_logs_show }}"
    install_pihole_api_privacy_mode: "{{ inv_install_pihole_api_privacy_mode }}"
    install_pihole_aaaa_list: "{{ inv_install_pihole_aaaa_list }}"
    install_pihole_cname_list: "{{ inv_install_pihole_cname_list }}"
    install_pihole_adlists: "{{ inv_install_pihole_adlists }}"
  ansible.builtin.include_role:
    name: "labocbz.install_pihole"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-19: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Role install PiHole with install bash and the "unattended" option
* Role use the setupVars to install PiHole so reconfiguration / update is handle
* Role importe a light ban list +- 650K
* Test are not performed for now

### 2023-08-21: Added DNS and CNAME$

* You can now import CNAME and DNS in the PiHole

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-12-18: Idempotency and Optimization

* Lot of action will not up a "changed" state, for idempotency (see bellow why)
* Update Gravity done if only a change noted for AAAA or CNAME records
* PiHole install script have changed, we have to remove /var/www/html/admin to install with script, so "changed" noted and "ok"

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [Install Pi-Hole without user interaction?](https://unix.stackexchange.com/questions/423715/install-pi-hole-without-user-interaction)
* [Pi-hole as part of a post-installation script](https://discourse.pi-hole.net/t/pi-hole-as-part-of-a-post-installation-script/3523)
* [What is setupVars.conf and how do I use it?](https://discourse.pi-hole.net/t/what-is-setupvars-conf-and-how-do-i-use-it/3533/1)
* [The Big Blocklist Collection](https://firebog.net/)
