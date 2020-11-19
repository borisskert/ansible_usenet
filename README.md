# ansible-usenet

Installs `sabnzbd` and `sonarr` as docker containers managed by systemd.

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
| usenet_docker_working_directory | text | no         | nextcloud | Docker image name (recommended to use the default image)  |
