# ansible-usenet

Installs [`sabnzbd`](https://sabnzbd.org/), [`sonarr`](https://sonarr.tv/), [`radarr`](https://radarr.video/) and [`bazarr`](https://www.bazarr.media/) as docker containers managed by systemd.

## System requirements

* Docker
* docker-compose
* Systemd

## Role requirements

* (none, so far)

## Tasks

* Template docker-compose file environment
* Create volume paths for docker containers
* Setup systemd unit file
* Start/Restart systemd service

## Role parameters

| Variable      | Type | Mandatory? | Default | Description           |
|---------------|------|------------|---------|-----------------------|
| usenet_sabnzbd_version | text | no | latest | sabnzbd's Docker image version |
| usenet_sonarr_version  | text | no | latest | sonarr's Docker image version |
| usenet_radarr_version  | text | no | latest | radarr's Docker image version |
| usenet_bazarr_version  | text | no | latest | bazarr's Docker image version |
| usenet_docker_working_directory | absolute path | no | /opt/usenet/docker | Docker working directory |
| usenet_sabnzbd_config_volume_directory | absolute path | no | /srv/usenet/sabnzbd/config | sabnzbd config volume directory |
| usenet_sabnzbd_downloads_volume_directory | absolute path | no | /srv/usenet/sabnzbd/downloads | sabnzbd downloads volume directory |
| usenet_sabnzbd_incomplete_downloads_volume_directory | absolute path | no | /srv/usenet/sabnzbd/incomplete-downloads | sabnzbd incomplete downloads volume directory |
| usenet_sonarr_config_volume_directory                | absolute path | no | /srv/usenet/sonarr/config | sonarr config volume directory |
| usenet_sonarr_tv_volume_directory                    | absolute path | no | /srv/usenet/sonarr/tv | sonarr tv volume directory |
| usenet_sonarr_downloads_volume_directory             | absolute path | no | /srv/usenet/sonarr/downloads | sonarr downloads volume directory |
| usenet_radarr_config_volume_directory                | absolute path | no | /srv/usenet/radarr/config | radarr config volume directory |
| usenet_radarr_movies_volume_directory                | absolute path | no | /srv/usenet/radarr/movies | radarr movies volume directory |
| usenet_radarr_downloads_volume_directory             | absolute path | no | /srv/usenet/radarr/downloads | radarr downloads volume directory |
| usenet_bazarr_config_volume_directory                | absolute path | no | /srv/usenet/bazarr/config    | bazarr config volume directory |
| usenet_bazarr_tv_volume_directory                    | absolute path | no | /srv/usenet/bazarr/tv        | bazarr tv volume directory |
| usenet_bazarr_movies_volume_directory                | absolute path | no | /srv/usenet/bazarr/movies    | bazarr movies volume directory |
| usenet_sabnzbd_network_interface                     | network address | no | 0.0.0.0 | Bound network interface for sabnzbd's web-interface |
| usenet_sabnzbd_http_port                             | network port    | no | 8080    | Network port for sabnzbd's http web-interface |
| usenet_sabnzbd_https_port                            | network port    | no | 9090    | Network port for sabnzbd's httpd web-interface |
| usenet_sonarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for sonarr's web-interface |
| usenet_sonarr_http_port                              | network port    | no | 8989    | Network port for sonarr's http web-interface |
| usenet_radarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for radarr's web-interface |
| usenet_radarr_http_port                              | network port    | no | 7878    | Network port for radarr's http web-interface |
| usenet_bazarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for bazarr's web-interface |
| usenet_bazarr_http_port                              | network port    | no | 6767    | Network port for bazarr's http web-interface |
| usenet_user_id                                       | user id         | no | 666     | User id the services are running with |
| usenet_group_id                                      | group id        | no | 666     | Group id the services are running with |

## Example Playbook

### Add to `requirements.yml`:

```yaml
- name: install-usenet
  src: https://github.com/borisskert/ansible_usenet.git
  scm: git
```

### Example `playbook.yml`:

```yaml
- hosts: servers
  roles:
    - role: install-usenet
      usenet_sabnzbd_version: version-3.1.1
      usenet_sonarr_version: version-2.0.0.5344
      usenet_radarr_version: version-3.0.0.4127
      usenet_bazarr_version: version-v0.9.0.6
      usenet_sabnzbd_network_interface: 0.0.0.0
      usenet_sabnzbd_http_port: 8080
      usenet_sabnzbd_https_port: 9090
      usenet_sonarr_network_interface: 0.0.0.0
      usenet_sonarr_http_port: 8989
      usenet_radarr_network_interface: 0.0.0.0
      usenet_radarr_http_port: 7878
      usenet_bazarr_network_interface: 0.0.0.0
      usenet_bazarr_http_port: 6767
      usenet_sabnzbd_config_volume_directory: /srv/usenet/sabnzbd/config
      usenet_sabnzbd_downloads_volume_directory: /srv/usenet/sabnzbd/downloads
      usenet_sabnzbd_incomplete_downloads_volume_directory: /srv/usenet/sabnzbd/incomplete-downloads
      usenet_sonarr_config_volume_directory: /srv/usenet/sonarr/config
      usenet_sonarr_tv_volume_directory: /srv/usenet/sonarr/tv
      usenet_sonarr_downloads_volume_directory: /srv/usenet/sonarr/downloads
      usenet_radarr_config_volume_directory: /srv/usenet/radarr/config
      usenet_radarr_movies_volume_directory: /srv/usenet/radarr/movies
      usenet_radarr_downloads_volume_directory: /srv/usenet/radarr/downloads
      usenet_bazarr_config_volume_directory: /srv/usenet/bazarr/config
      usenet_bazarr_tv_volume_directory: /srv/usenet/bazarr/tv
      usenet_bazarr_movies_volume_directory: /srv/usenet/bazarr/movies
      usenet_user_id: 666
      usenet_group_id: 666
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
molecule test --scenario-name docker-default && molecule test --scenario-name docker-all-parameters
```

### Run within Vagrant

```shell script
molecule test --scenario-name vagrant-default && molecule test --scenario-name vagrant-all-parameters
```

I recommend to use [pyenv](https://github.com/pyenv/pyenv) for local testing.
Within the Github Actions pipeline I use [my own molecule Docker image](https://github.com/borisskert/docker-molecule).

## License

MIT

## Author Information

* [borisskert](https://github.com/borisskert)

## Further links

* [sabnzbd @ Docker hub](https://hub.docker.com/r/linuxserver/sabnzbd)
* [sonarr @ Docker hub](https://hub.docker.com/r/linuxserver/sonarr)
* [radarr @ Docker hub](https://hub.docker.com/r/linuxserver/radarr)
* [bazarr @ Docker hub](https://hub.docker.com/r/linuxserver/bazarr)
