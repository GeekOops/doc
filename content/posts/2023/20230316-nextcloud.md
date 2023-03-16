---
title: "nextcloud (system role)"
author: "phoenix"
date: 2023-03-16T10:17:42+02:00

---
The [geekoops-nc-system role](https://github.com/GeekOops/geekoops-nc-system) is an Ansible role to install and configure `nextcloud` on an openSUSE Leap server using the `nextcloud` system package and `apache2`. There is also the [geekoops-nextcloud role](https://github.com/GeekOops/geekoops-nextcloud) that is more complex and allows more customization.

If desired, the role also installs and configures MariaDB with a custom database and user for you.

# Example

The following example playbook installs nextcloud and creates a `nextcloud` database and database user using the password `password123` (very secure!).


```yaml
---
- hosts: jellyfish
  user: root
  
  roles:
     - role: geekoops-nc-system
       vars:
         db_configure: true
         db_name: 'nextcloud'
         db_user: 'nextcloud'
         db_pass: 'password123'
         firewall_configure: true
         firewall_zone: 'public'
```
