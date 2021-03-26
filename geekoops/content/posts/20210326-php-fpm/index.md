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

A extended example is in the [Example Webserver](/posts/20210326-example-webserver/) post.