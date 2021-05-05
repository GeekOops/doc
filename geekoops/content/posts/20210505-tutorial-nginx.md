---
title: "Tutorial: nginx"
author: "phoenix"
date: 2021-05-05T13:35:03+02:00
tags:
  - tutorial
  - nginx

---
Welcome to GeekOops! This post acts as a tutorial to start with the first role of this project: Installing and configuring a `nginx` webserver via the [geekoops-nginx role](https://github.com/GeekOops/geekoops-nginx).

# Tutorial

Let's assume we want to roll this role out on our imaginary `jellyfish` server. We need to have root `ssh` access to `jellyfish`. Ansible also supports `sudo`, but this is not covered here. So first make sure you can do the following

    # Make sure you can login into our imaginary test server as root!
    ssh root@jellyfish

All of the next steps will take place in a temporary directory. I in general keep all my ansible stuff in a dedicated `ansible` folder in my home directory, for this tutorial we are going to use a empty new folder, as you might want to dump everything afterwards.

    # For this tutorial we start in a virgin new folder
    mkdir jellyfish-sandbox
    cd jellyfish-sandbox

Now, clone the [geekoops-nginx role](https://github.com/GeekOops/geekoops-nginx)

    git clone https://github.com/GeekOops/geekoops-nginx

Ansible operates with so-called Playbooks. Playbooks are a `yaml` file that tell ansible what to do. Roughly it consists of a definition of which hosts ansible should operate on and what to do there. For this example we are going to use the following playbook, which you store as `jellyfish.yml`:

    ---
    # Example playbook for nginx on jellyfish
    - hosts: jellyfish
      user: root
      
      roles:
         - role: geekoops-nginx
           vars:
             setup_default_page: true
             default_page_hostname: "{{ansible_host}}"
             config_firewall: true
             firewall_zone: "public"

Last thing is to create a custom inventory. Although you might have put jellyfish into your `~/.ssh/config` already, ansible needs also to know that this is a host for it. For that we create a file called `inventory` with this simple line:

    jellyfish

Ok, when you have done that, then we're ready to rock. Run `ansible-playbook` with the inventory and the playbook:

    ansible-playbook -i inventory jellyfish.yml

Ansible now runs the playbook, `nginx` will be installed and the firewall will open the http (80) and https (443) port for you on the "public" zone.

Done!