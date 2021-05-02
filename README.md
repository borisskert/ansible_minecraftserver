# ansible_minecraftserver

This is an [ansible role](https://docs.ansible.com/) to install a [minecraft server](https://www.minecraft.net/de-de/download/server) as
[docker container](https://www.docker.com/) controlled by [systemd](https://systemd.io/).

I'm using [itzg's docker image](https://github.com/itzg/docker-minecraft-server). Thanks for your great work!

## System requirements

* Docker
* docker-compose
* Systemd

## Role requirements

* (none, so far)

## Tasks

* Template docker-compose file environment
* Create data volume directory for docker container
* Setup systemd unit file
* Start/Restart systemd service

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| minecraftserver_state         | text | no | `'present'` | If `absent` removes the docker configuration and systemd service |
| minecraftserver_image_version | text | no | `'latest'`  | itzg's Docker image version |
| minecraftserver_version       | text | no | `''`        | Minecraft Server version (e.g. `1.16.5`, default: latest version will be downloaded) |
| minecraftserver_interface     | text | no | `0.0.0.0`   | Mapped network interface |
| minecraftserver_port          | text | no | `25565`     | Mapped network port |
| minecraftserver_data_volume_directory | text | no | `/srv/minecraftserver/minecraft-data` | Location of your data volume directory |

## Example Playbook

### Add to `requirements.yml`:

```yaml
- name: install-minecraftserver
  src: https://github.com/borisskert/ansible_minecraftserver.git
  scm: git
```

### Example `playbook.yml`:

```yaml
- hosts: servers
  roles:
    - role: install-minecraftserver
      minecraftserver_image_version: latest
      minecraftserver_version: '1.16.5'
      minecraftserver_interface: 0.0.0.0
      minecraftserver_port: 25565
      minecraftserver_data_volume_directory: /srv/minecraftserver/minecraft-data
```

## Testing

Requirements:

* [Vagrant](https://www.vagrantup.com/)
* [Qemu](https://www.qemu.org/libvirt) and [libvirt](https://libvirt.org/)
* [Ansible](https://docs.ansible.com/)
* [Molecule](https://molecule.readthedocs.io/en/latest/index.html)
* [yamllint](https://yamllint.readthedocs.io/en/stable/#)
* [ansible-lint](https://docs.ansible.com/ansible-lint/)
* [Docker](https://docs.docker.com/)

### Run within docker

```shell script
molecule test && molecule test --scenario-name all-parameters && molecule test --scenario-name state-absent
```

### Run within Vagrant

```shell script
molecule test -s vagrant-default && molecule test -s vagrant-all-parameters
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

## Further links

* [itzg/docker-minecraft-server (Github)](https://github.com/itzg/docker-minecraft-server)
