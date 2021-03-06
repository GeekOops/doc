---
title: GeekOops
---
Collection of awesome SysOps utilities for automated deployment. It is pronounced Geeko-Ops or Geek Oops, depending on your mood :-)

The project lives on [GitHub](https://github.com/GeekOops) and currently consists of a set of `ansible` roles for automated deployment of certain services. The roles are written to be easily usable, minimal yet configurable.

Minimalism is key for GeekOops. Each role is aimed to be self-sufficient, without additional and crazy dependency chains. Each role is also automatically tested to ensure, that the roles remain functional over time.

GeekOops was started in [SUSE Hackweek 2021](https://hackweek.suse.com/20/projects/create-ansible-roles-for-generic-server-stuff). Have a lot of fun!

## Getting started

Checkout the [nginx tutorial post](/posts/20210505-tutorial-nginx) for a basic step-by-step guide. A more how-to is the [example webserver post](/posts/20210326-example-webserver/), where the procedure for setting up a simple `nginx`+`php-fpm` webserver on openSUSE Leap is shown.

# Roles

* [Ansible nginx](/posts/20210326-nginx/)
* [Ansible php-fpm](/posts/20210326-php-fpm/)
* [Ansible PureFTPd](/posts/20210326-pureftpd/)
* [Ansible NEXT](/posts/20210326-next/) (PXE boot server)