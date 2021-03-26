---
title: "Ansible php-fpm"
author: "phoenix"
date: 2021-03-26T09:42:03+01:00
---
The [geekoops-php-fpm role](https://github.com/GeekOops/geekoops-php-fpm) is intended as a standalone `php-fpm` deployment that works in conjunction with any webserver. I use it with `nginx` in production. In principle it should also work together with `apache2`, but that's something currently untested.

This role works with openSUSE Leap and is intended to ship enough requirements for most web applications to run. I run it in conjunction with Mediawiki and Nextcloud.

## Role Variables

This ansible role comes with a minimal set of configuration parameters.


| Value | Description | Default |
|-------|-------------|---------|
| `configure_php_ini` | Configrue the `php.ini` file | `true` |
| `configure_php_fpm` | Configure php-fpm configuration files  | `true` |
| `enable_php_fpm` | Enable `php-fpm` service | `true` |
| `apcu_enable` | Enable the [APCu](https://www.php.net/manual/en/book.apcu.php) cache | `false` |
| `apcu_shm_size` | APCu cache size | `32M` |
| `php_memlimit` | PHP memory limit | `128M` |
| `php_uploads` | Enable PHP uploads | `On` |
| `php_maxuploadsize` | Max upload size | `256M` |
| `php_maxuploads` | Max uploads in a request | `20` |

# Example

This [example playbook](jellyfish.yml) sets up a webserver with `nginx` and `php-fpm`.

    ---
    - hosts: jellyfish
      user: root
      
      roles:
        - role: geekoops-nginx
          vars:
            config_firewall: true
            firewall_zone: "public"
        - role: geekoops-php-fpm
          vars:
            apcu_enable: true
            apcu_shm_size: 32M
            php_memlimit: 256M
            php_maxuploadsize: 64M
      
        tasks:
        - name: Deploy jellyfish config for nginx
          copy:
            content: |
              server {
                listen 80 default_server;
                listen [::]:80 default_server;
                server_name jellyfish;
                location / {
                  proxy_pass http://127.0.0.1:3000/;
                }
              }
            dest: "/etc/nginx/vhosts.d/jellyfish.conf"
            group: "root"
            owner: "root"
            mode: 0754
          notify: Restart nginx
    
      handlers:
        - name: Restart nginx
          systemd:
            name: nginx
            state: restarted