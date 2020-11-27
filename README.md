# ansible-usenet

Installs [`sabnzbd`](https://sabnzbd.org/), [`sonarr`](https://sonarr.tv/), [`radarr`](https://radarr.video/), [`bazarr`](https://www.bazarr.media/) and [`nzbhydra2`](https://github.com/theotherp/nzbhydra2/) as docker containers managed by systemd.

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
| usenet_nzbhydra2_version | text | no | latest | nzbhydra2's Docker image version |
| usenet_docker_working_directory | absolute path | no | /opt/usenet/docker | Docker working directory |
| usenet_sabnzbd_config_volume_directory               | absolute path | no | /srv/usenet/sabnzbd/config               | Location where sabnzbd's config will be stored |
| usenet_sonarr_config_volume_directory                | absolute path | no | /srv/usenet/sonarr/config                | Location where sonarr's config will be stored |
| usenet_radarr_config_volume_directory                | absolute path | no | /srv/usenet/radarr/config                | Location where radarr's config will be stored |
| usenet_bazarr_config_volume_directory                | absolute path | no | /srv/usenet/bazarr/config                | Location where bazarr's config will be stored |
| usenet_nzbhydra2_config_volume_directory             | absolute path | no | /srv/usenet/nzbhydra2/movies             | Location where nzbhydra2's config will be stored |
| usenet_downloads_volume_directory                    | absolute path | no | /srv/usenet/sabnzbd/downloads            | Location where downloads will be stored |
| usenet_incomplete_downloads_volume_directory         | absolute path | no | /srv/usenet/sabnzbd/incomplete-downloads | Location where incomplete downloads will be stored |
| usenet_tv_volume_directory                           | absolute path | no | /srv/usenet/tv                           | Location where tv will be stored |
| usenet_movies_volume_directory                       | absolute path | no | /srv/usenet/movies                       | Location where movies will be stored |
| usenet_sabnzbd_network_interface                     | network address | no | 0.0.0.0 | Bound network interface for sabnzbd's web-interface |
| usenet_sabnzbd_http_port                             | network port    | no | 8080    | Network port for sabnzbd's http web-interface |
| usenet_sabnzbd_https_port                            | network port    | no | 9090    | Network port for sabnzbd's httpd web-interface |
| usenet_sonarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for sonarr's web-interface |
| usenet_sonarr_http_port                              | network port    | no | 8989    | Network port for sonarr's http web-interface |
| usenet_radarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for radarr's web-interface |
| usenet_radarr_http_port                              | network port    | no | 7878    | Network port for radarr's http web-interface |
| usenet_bazarr_network_interface                      | network address | no | 0.0.0.0 | Bound network interface for bazarr's web-interface |
| usenet_bazarr_http_port                              | network port    | no | 6767    | Network port for bazarr's http web-interface |
| usenet_nzbhydra2_network_interface                   | network address | no | 0.0.0.0 | Bound network interface for nzbhydra2's web-interface |
| usenet_nzbhydra2_http_port                           | network port    | no | 5076    | Network port for nzbhydra2's http web-interface |
| usenet_user_id                                       | user id         | no | 666     | User id the services are running with |
| usenet_group_id                                      | group id        | no | 666     | Group id the services are running with |
| usenet_use_vpn                                       | boolean         | no | false   | Specifies if `sabnzbd` and `nzbhydra2` are using a vpn container connection |
| usenet_vpn_container_name                            | text            | yes, if `usenet_use_vpn` is true | The name of the used container which provides the vpn connection |

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
      usenet_nzbhydra2_version: version-v3.5.1
      usenet_sabnzbd_network_interface: 0.0.0.0
      usenet_sabnzbd_http_port: 8080
      usenet_sabnzbd_https_port: 9090
      usenet_sonarr_network_interface: 0.0.0.0
      usenet_sonarr_http_port: 8989
      usenet_radarr_network_interface: 0.0.0.0
      usenet_radarr_http_port: 7878
      usenet_bazarr_network_interface: 0.0.0.0
      usenet_bazarr_http_port: 6767
      usenet_nzbhydra2_network_interface: 0.0.0.0
      usenet_nzbhydra2_http_port: 5076
      usenet_sabnzbd_config_volume_directory: /srv/usenet/config/sabnzbd
      usenet_sonarr_config_volume_directory: /srv/usenet/config/sonarr
      usenet_radarr_config_volume_directory: /srv/usenet/config/radarr
      usenet_bazarr_config_volume_directory: /srv/usenet/config/bazarr
      usenet_nzbhydra2_config_volume_directory: /srv/usenet/config/nzbhydra2
      usenet_downloads_volume_directory: /srv/usenet/downloads
      usenet_incomplete_downloads_volume_directory: /srv/usenet/incomplete-downloads
      usenet_tv_volume_directory: /srv/usenet/tv
      usenet_movies_volume_directory: /srv/usenet/movies
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
molecule test --scenario-name docker-default && molecule test --scenario-name docker-all-parameters && molecule test -s docker-with-vpn && molecule test -s docker-with-and-all-parameters
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
* [nzbhydra2 @ Docker hub](https://hub.docker.com/r/linuxserver/nzbhydra2)
