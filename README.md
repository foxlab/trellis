# FoxLab Workflow

## Requirements

Make sure all dependencies have been installed before moving on:

* [Ansible](http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-pip) 2.0.2
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) >= 4.3.10
* [Vagrant](https://releases.hashicorp.com/vagrant/1.8.1/) 1.8.1
* [vagrant-bindfs](https://github.com/gael-ian/vagrant-bindfs#installation) >= 0.3.1 (Windows users may skip this)
* [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager#installation)

## Installation

1. Create a new project directory: `$ mkdir example && cd example`
2. Clone Trellis: `$ git clone --depth=1 git@github.com:foxlab/trellis.git && rm -rf trellis/.git`
3. Clone Bedrock: `$ git clone --depth=1 git@github.com:foxlab/bedrock.git site && rm -rf site/.git`
4. Install the Ansible Galaxy roles: `$ cd trellis && ansible-galaxy install -r requirements.yml`

## Local development setup

1. Configure your WordPress sites in `group_vars/development/wordpress_sites.yml` and in `group_vars/development/vault.yml`
2. Run `vagrant up`

## Add and start new Wordpress site dev

1. Configure your WordPress sites in `group_vars/development/wordpress_sites.yml` and in `group_vars/development/vault.yml`
2. Clone Bedrock: `$ git clone --depth=1 git@github.com:foxlab/bedrock.git site && rm -rf site/.git`
3. Navigate to the trellis directory
4. Run `vagrant reload --provision`
5. Navigate to the site directory
6. Run `composer update`
7. Navigate to the theme directory
8. Run `npm install`
9. Run `bower install`
10. Run `gulp`

## Using BrowserSync

To use BrowserSync during `gulp watch` you need to update `devUrl` at the bottom of `assets/manifest.json` to reflect your local development hostname.

```json
...
  "config": {
    "devUrl": "http://project-name.dev"
  }
...
```

# Trellis

[![Build Status](https://travis-ci.org/roots/trellis.svg)](https://travis-ci.org/roots/trellis)

Ansible playbooks for setting up a LEMP stack for WordPress.

- Local development environment with Vagrant
- High-performance production servers
- One-command deploys for your [Bedrock](https://roots.io/bedrock/)-based WordPress sites

## What's included

Trellis will configure a server with the following and more:

* Ubuntu 16.04 Xenial LTS
* Nginx (with optional FastCGI micro-caching)
* PHP 7.0
* MariaDB (a drop-in MySQL replacement)
* SSL support (scores an A+ on the [Qualys SSL Labs Test](https://www.ssllabs.com/ssltest/))
* Let's Encrypt integration for free SSL certificates
* HTTP/2 support (requires SSL)
* Composer
* WP-CLI
* sSMTP (mail delivery)
* MailHog
* Memcached
* Fail2ban
* ferm

## Requirements

Make sure all dependencies have been installed before moving on:

* [Ansible](http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-pip) 2.0.2
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) >= 4.3.10
* [Vagrant](https://www.vagrantup.com/downloads.html) >= 1.8.5
* [vagrant-bindfs](https://github.com/gael-ian/vagrant-bindfs#installation) >= 0.3.1 (Windows users may skip this)
* [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager#installation)

## Installation

The recommended directory structure for a Trellis project looks like:

```shell
example.com/      # → Root folder for the project
├── trellis/      # → Your clone of this repository
└── site/         # → A Bedrock-based WordPress site
    └── web/
        ├── app/  # → WordPress content directory (themes, plugins, etc.)
        └── wp/   # → WordPress core (don't touch!)
```

See a complete working example in the [roots-example-project.com repo](https://github.com/roots/roots-example-project.com).

1. Create a new project directory: `$ mkdir example.com && cd example.com`
2. Clone Trellis: `$ git clone --depth=1 git@github.com:roots/trellis.git && rm -rf trellis/.git`
3. Clone Bedrock: `$ git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git`
4. Install the Ansible Galaxy roles: `$ cd trellis && ansible-galaxy install -r requirements.yml`

Windows user? [Read the Windows docs](https://roots.io/trellis/docs/windows/) for slightly different installation instructions. VirtualBox is known to have poor performance in Windows — use VMware or [see some possible solutions](https://discourse.roots.io/t/virtualbox-performance-in-windows/3932).

## Documentation

Trellis documentation is available at [https://roots.io/trellis/docs/](https://roots.io/trellis/docs/).

## Local development setup

1. Configure your WordPress sites in `group_vars/development/wordpress_sites.yml` and in `group_vars/development/vault.yml`
2. Run `vagrant up`

[Read the local development docs](https://roots.io/trellis/docs/local-development-setup/) for more information.

## Remote server setup (staging/production)

A base Ubuntu 16.04 server is required for setting up remote servers.

1. Configure your WordPress sites in `group_vars/<environment>/wordpress_sites.yml` and in `group_vars/<environment>/vault.yml` (see the [Vault docs](https://roots.io/trellis/docs/vault/) for how to encrypt files containing passwords)
2. Add your server IP/hostnames to `hosts/<environment>`
3. Specify public SSH keys for `users` in `group_vars/all/users.yml` (see the [SSH Keys docs](https://roots.io/trellis/docs/ssh-keys/))
4. Run `ansible-playbook server.yml -e env=<environment>` to provision the server

[Read the remote server docs](https://roots.io/trellis/docs/remote-server-setup/) for more information.

## Deploying to remote servers

1. Add the `repo` (Git URL) of your Bedrock WordPress project in the corresponding `group_vars/<environment>/wordpress_sites.yml` file
2. Set the `branch` you want to deploy
3. Run `./deploy.sh <environment> <site name>`
4. To rollback a deploy, run `ansible-playbook rollback.yml -e "site=<site name> env=<environment>"`

[Read the deploys docs](https://roots.io/trellis/docs/deploys/) for more information.

## Contributing

Contributions are welcome from everyone. We have [contributing guidelines](https://github.com/roots/guidelines/blob/master/CONTRIBUTING.md) to help you get started.

## Community

Keep track of development and community news.

* Participate on the [Roots Discourse](https://discourse.roots.io/)
* Follow [@rootswp on Twitter](https://twitter.com/rootswp)
* Read and subscribe to the [Roots Blog](https://roots.io/blog/)
* Subscribe to the [Roots Newsletter](https://roots.io/subscribe/)
* Listen to the [Roots Radio podcast](https://roots.io/podcast/)
