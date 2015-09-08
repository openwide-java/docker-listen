Intro
=====

docker-listen provides automatic dnsmasq hosts configuration update on docker
run and stop events.

Installation
============

docker-listen relies on following dependencies :
 * dpath
 * docker-py

You can install these dependencies by your regular packaging system.

Otherwise, you can use virtualenv and pip to prepare an isolated python env :

    virtualenv docker-listen-pyenv
    ./docker-listen-pyenv/bin/pip install dpath
    ./docker-listen-pyenv/bin/pip install docker-py

`<python>` refers to an interpreter loaded with needed dépendencies.

Dnsmasq configuration
=====================

You need to modify dnsmasq configuration to load the hosts declared by
docker-listen.

In your /etc/dnsmasq.conf, /etc/dnsmasq.d/docker.conf or 
/etc/NetworkManager/dnsmasq.d/docker.conf, add an hosts_dir configuration

    addn-hosts=/etc/dnsmasq.d/docker-hosts

Running
=======

Run the following command :

    sudo <python> docker-listen.py

[pex](https://github.com/pantsbuild/pex) enables one-line install / run :

    sudo pex dpath docker-py -- docker-listen.py

**WARN** : /etc/dnsmasq.d/docker-hosts will be cleared (all files deleted) each
time process is started.

**WARN** : sudo is required so that docker-listen can force dnsmasq to reload hosts
(with a SIGHUP process signal)

Configuration
=============

    sudo <python> docker-listen.py

Options can be loaded from a python .ini configuration file provided with `-c`
command line option.

Example :

    [docker-listen]
    hosts_dir=/etc/dnsmasq.d/docker-hosts
    docker_url=unix://var/run/docker.sock
    sighup_enabled=yes
    sighup_process_name=dnsmasq
    log_level=WARN
