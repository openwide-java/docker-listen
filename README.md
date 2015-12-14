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

Running
=======

Run the following command :

    sudo <python> docker-listen.py

[pex](https://github.com/pantsbuild/pex) enables one-line install / run :

    sudo pex dpath docker-py -- docker-listen.py

**WARN** : /etc/dnsmasq.d/docker-* will be cleared each time process is started.

**WARN** : sudo is required so that docker-listen can force dnsmasq to reload hosts
(with a SIGHUP process signal)

Running as a service
====================

Copy the file ``pstart/docker-listen.conf`` in ``/etc/init``.

Configure the path to docker-listen in ``/etc/init/docker-listen.conf`` and start the service with ``service docker-listen start``.

Configuration
=============

    sudo <python> docker-listen.py

Options can be loaded from a python .ini configuration file provided with `-c`
command line option.

Example :

    [docker-listen]
    hosts_domain_name=docker.openwide.fr
    hosts_dir=/etc/dnsmasq.d
    docker_url=unix://var/run/docker.sock
    sighup_enabled=yes
    sighup_process_name=dnsmasq
    log_level=WARN
