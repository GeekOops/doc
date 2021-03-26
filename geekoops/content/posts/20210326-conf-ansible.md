---
title: "20210326 Conf Ansible"
author: "phoenix:"
date: 2021-03-26T11:21:33+01:00
draft: true
---

How to keep my ansible roles up to date?

# Using git submodules

    cd ~/.ansible/roles
    git init
    git submodule add git@github.com:GeekOops/geekoops-php-fpm.git
    git submodule add git@github.com:GeekOops/geekoops-pureftpd.git
    git submodule add git@github.com:GeekOops/geekoops-next.git
    git submodule add git@github.com:GeekOops/geekoops-grafana.git
    git submodule add git@github.com:GeekOops/geekoops-collectd.git

And then

    git submodule update
