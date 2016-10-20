ansible-nginx for Ubuntu
============

Ansible role for installing basic nginx confguration.

## Automatic nginx configuration optimisation.

The default configuration file `/etc/nginx/nginx.conf` will be updated to reflect resources available on the target system.

```nginx
user {{ nginx.user|default('www-data') }};
worker_processes {{ ansible_processor_vcpus }};
```

`user` will be set to the typical user of `www-data` and can be overwritten.

Number of `worker_processes` will be configured based on the number of CPU cores available using Ansible system facts.

# Gzip

On by default, because **performance**.

## sites-enabled and sites-available folders

**As part of the playbook, these folders will be removed.** These folders are relics of the Apache configuration process and has no real place in nginx land.

Place all of your configurations in `/etc/nignx/conf.d` and give them `.conf` suffix.

## HTML5 Boilerplate Nginx Server Configs

Default: `{{ h5bp_install }}` defaults to `True`.

This playbook uses `Nginx Server Configs` by the amazing folks at [HTML5 Boilerplate](https://github.com/h5bp).

Their handy configuration files can be found in `/etc/nginx/h5bp` folder.

To add their basic mix of configurations, add the following line to your configuration file.

```nginx
{
    ...
    include h5bp/basic.conf;
    ...
}
```

## Overwriting Variables

The `vars/main.yml` file should contain your list of packages you want to install in order to override defaults found in `defaults/main.yml`. Additionally, you can overwrite the variables as part of your playbook.

```yml
---
...
nginx_ppa: nginx/stable

# Setting this to `yes` will stop
# installation of /etc/nginx/conf.d/default.conf
nginx_install_default: yes
doc_root: /usr/share/nginx/html
default_domain: 127.0.0.1

www_user: www-data
www_group: www-data
...
```

## Adding additional site configurations

When setting up site configurations, place them in following path `/etc/nginx/conf.d/` and make sure to restart nginx `sudo service nginx restart`.
