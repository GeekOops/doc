---
title: "Example: Webserver (nginx+php-fpm)"
author: "phoenix"
date: 2021-03-26T10:49:29+01:00
---
In this post we are going to setup our example `jellyfish` host to run `nginx `and `php-fpm`. The provided [example playbook](jellyfish.yml) should be a good starting point for your own webserver.

In addition, the exaple playbook also setups the `jellyfish.conf` nginx virtual host file to run `php` files with `php-fpm` and we create a typical `phpinfo.php` file to test our setup

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
      - name: Deploy phpinfo script
        copy:
          content: "<?php phpinfo(); phpinfo(INFO_MODULES); ?>"
          dest: "/srv/www/phpinfo.php"
          group: "www"
          owner: "wwwrun"
          mode: 0754
    
      handlers:
        - name: Restart nginx
          systemd:
            name: nginx
            state: restarted

# Example

For this example, ensure you have installed `ansible` on your host machine

    sudo zypper in ansible

Then, we need a working directory, let's say `jellyfish`.

    mkdir jellyfish
    cd jellyfish

All next steps should be run in this directory.

## No install required: JeOS VM

The most easy way of just getting a openSUSE Leap VM running, is to use the JeOS image, available at https://get.opensuse.org/leap. Download the KVM or XEN HVM image, import it into your `virt-manager` and your're ready to go!

Once you fire the machine up, you just need a handful of configuration steps, and then your have a functional JeOS VM, which is enough for this setup.

### JeOS installation

Back in our `jellyfish` directory, we first download the VM image (which is also our hard disk)

    $ wget -O jellyfish.cow2 https://download.opensuse.org/distribution/leap/15.2/appliances/openSUSE-Leap-15.2-JeOS.x86_64-kvm-and-xen.qcow2

Then we run `virt-install` to setup and run our `jellyfish` server:

    $ virt-install --name=jellyfish --file=$PWD/jellyfish.qcow2 --vcpus=2 --ram=2048 --os-type=linux --os-variant=opensuse15.2 --boot hd

After a handful of configuration steps, you have a functional system in just some minutes

![Jellyfish setup](/img/jellyfish-jeos.gif)

Now, you have to ensure, you have root ssh access to `jellyfish`. For that, first log in, then find out the ip address using `ip address` (or `ip a` in short):

    # ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
        link/ether 52:54:00:85:0f:7a brd ff:ff:ff:ff:ff:ff
        inet 192.168.122.116/24 brd 192.168.122.255 scope global eth0
           valid_lft forever preferred_lft forever
        inet6 fe80::5054:ff:fe85:f7a/64 scope link 
           valid_lft forever preferred_lft forever

Then on your host system, do

    ssh-copy-id root@jellyfish

Done. Now it's a good time to create a VM snapshot, in case you want to have a fresh system.

Also, add the IP address to `/etc/hosts` otherwise you will need to replace `jellyfish` by the ip address in all following commands.
Add the last line `192.168.122.116    jellyfish` to your `/etc/hosts` file.

    vim /etc/hosts
    
    # IP-Address  Full-Qualified-Hostname  Short-Hostname
    #
    
    127.0.0.1	localhost
    
    # special IPv6 addresses
    ::1             localhost ipv6-localhost ipv6-loopback
    
    fe00::0         ipv6-localnet
    
    ff00::0         ipv6-mcastprefix
    ff02::1         ipv6-allnodes
    ff02::2         ipv6-allrouters
    ff02::3         ipv6-allhosts
    
    ::1             hotdog
    [...]
    
    192.168.122.116    jellyfish

Remember to remove this line afterwards, in case you don't want to keep your `jellyfish` :-)

## Running the playbook

First ensure, that you have root access to `jellyfish`

    ssh root@jellyfish

Back in our `jellyfish` directory, we need to get the roles and the playbook. Let's say

    # Download the ansible repositories
    git clone https://github.com/GeekOops/geekoops-nginx
    git clone https://github.com/GeekOops/geekoops-php-fpm
    
    # Download playbook
    curl -o jellyfish.yml https://geekoops.github.io/posts/20210326-example-webserver/jellyfish.yml
    
    # Create inventory
    echo "jellyfish" > inventory

Now let's run the playbook. `ansible` will then install all packages and configure `nginx` for you

    ansible-playbook -i inventory jellyfish.yml

Now in your browser navigate to [http://jellyfish/phpinfo.php](http://jellyfish/phpinfo.php) and you should see, the output of phpinfo:

![Output of phpinfo in webbrowser](/img/jellyfish-phpinfo.png)

Congratulations. You have successfully deployed a `nginx` + `php-fpm` webserver instance. Time to celebrate!