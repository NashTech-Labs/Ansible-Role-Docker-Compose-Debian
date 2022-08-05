Ansible Role to install Docker with Docker Compose on a Debian Platform
=========
[![CI](https://github.com/geerlingguy/ansible-role-docker/workflows/CI/badge.svg?event=push)](https://github.com/geerlingguy/ansible-role-docker/actions?query=workflow%3ACI)

This template is an Ansible Role that installs Docker and Docker Compose  on Linux with Debian distros.

Requirements
------------
* Python  : ``` sudo apt install python3 ```
* Ansible : you refer to [Ansible Installation](./InstallAnsible.md) for steps

* Ansible Prerequisites:

  * **One Ansible Control Node:** The Ansible control node is the machine we’ll use to connect to and control the Ansible hosts over SSH. Your Ansible control node can either be your local machine or a server dedicated to running Ansible, though this guide assumes your control node is an Ubuntu 20.04 system. Make sure the control node has:

    * A **non-root user** with sudo privileges. To set this up, you can follow Steps 2 and 3 of our [Initial Server Setup](./SetUpServer.md). However, please note that if you’re using a remote server as your Ansible Control node, you should follow every step of this guide. Doing so will configure a firewall on the server with ufw and enable external access to your non-root user profile, both of which will help keep the remote server secure.

    * An **SSH keypair** associated with this user. To set this up, you can follow Step 1 of [Set Up SSH Keys on Ubuntu](./SetUpSSHkey.md).

  * One or more **Ansible Hosts**: An Ansible host is any machine that your Ansible control node is configured to automate. This guide assumes your Ansible hosts are remote Ubuntu 20.04 servers. Make sure each Ansible host has:

    * The Ansible control node’s SSH public key added to the authorized_keys of a system user. This user can be either root or a regular user with sudo privileges. To set this up, you can follow Step 2 of [Set Up SSH Keys on Ubuntu](./SetUpSSHkey.md).


Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # Edition can be one of: 'ce' (Community Edition) or 'ee' (Enterprise Edition).
    docker_edition: 'ce'
    docker_packages:
        - "docker-{{ docker_edition }}"
        - "docker-{{ docker_edition }}-cli"
        - "docker-{{ docker_edition }}-rootless-extras"
    docker_packages_state: present

The `docker_edition` should be either `ce` (Community Edition) or `ee` (Enterprise Edition). 
You can also specify a specific version of Docker to install using the distribution-specific format: 
Red Hat/CentOS: `docker-{{ docker_edition }}-<VERSION>` (Note: you have to add this to all packages);
Debian/Ubuntu: `docker-{{ docker_edition }}=<VERSION>` (Note: you have to add this to all packages).

You can control whether the package is installed, uninstalled, or at the latest version by setting `docker_package_state` to `present`, `absent`, or `latest`, respectively. Note that the Docker daemon will be automatically restarted if the Docker package is updated. This is a side effect of flushing all handlers (running any of the handlers that have been notified by this and any other role up to this point in the play).

    docker_service_manage: true
    docker_service_state: started
    docker_service_enabled: true
    docker_restart_handler_state: restarted

Variables to control the state of the `docker` service, and whether it should start on boot. If you're installing Docker inside a Docker container without systemd or sysvinit, you should set `docker_service_manage` to `false`.

    docker_install_compose_plugin: false
    docker_compose_package: docker-compose-plugin
    docker_compose_package_state: present

Docker Compose Plugin installation options. These differ from the below in that docker-compose is installed as a docker plugin (and used with `docker compose`) instead of a standalone binary.

    docker_install_compose: true
    docker_compose_version: "1.26.0"
    docker_compose_arch: x86_64
    docker_compose_path: /usr/local/bin/docker-compose

Docker Compose installation options.

    docker_repo_url: https://download.docker.com/linux

The main Docker repo URL, common between Debian and RHEL systems.

    docker_apt_release_channel: stable
    docker_apt_arch: amd64
    docker_apt_repository: "deb [arch={{ docker_apt_arch }}] {{ docker_repo_url }}/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} {{ docker_apt_release_channel }}"
    docker_apt_ignore_key_error: True
    docker_apt_gpg_key: "{{ docker_repo_url }}/{{ ansible_distribution | lower }}/gpg"

You can switch the channel to `nightly` if you want to use the Nightly release.

You can change `docker_apt_gpg_key` to a different url if you are behind a firewall or provide a trustworthy mirror.
Usually in combination with changing `docker_apt_repository` as well.


    docker_users:
      - user1
      - user2

A list of system users to be added to the `docker` group (so they can use Docker on the server).

    docker_daemon_options:
      storage-driver: "devicemapper"
      log-opts:
        max-size: "100m"

Custom `dockerd` options can be configured through this dictionary representing the json file `/etc/docker/daemon.json`.


## Example Playbook

```yaml
- hosts: all
  roles:
    - docker_compose
```