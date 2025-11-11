# beingtomgreen.lidarr_docker

A simple ansible role for running [Lidarr](https://lidarr.audio/) via Docker compose using the [LinuxServer.io Lidarr image](https://docs.linuxserver.io/images/docker-lidarr).

For additional information on how to best handle your media paths see information on [wiki.servarr.com](https://wiki.servarr.com/docker-guide#consistent-and-well-planned-paths) and [trash-guides.info](https://trash-guides.info/File-and-Folder-Structure/Hardlinks-and-Instant-Moves/).

## Installation

Given that Galaxy seems to have abandoned roles, I suggest referencing this repository directly in your projects `requirements.yml`:

```yaml
---

roles:
  - name: beingtomgreen.lidarr_docker
    src: https://github.com/BeingTomGreen/ansible-role-lidarr-docker.git

collections: []
```

You can then install it alongside your other requirements as normal:

```bash
ansible-galaxy install -r requirements.yml
```

## Example usage

### Basic usage

```yaml
---

- hosts: lidarr
  become: true
  vars:
    lidarr_docker_env_puid: 5000
    lidarr_docker_env_pgid: 5000

    lidarr_docker_additional_volumes:
        - '/path/to/music:/music'
        - '/path/to/usenet:/usenet'
        - '/path/to/torrents:/torrents'
  roles:
    - role: beingtomgreen.lidarr_docker
```

### Want the kitchen sink?

Seriously, take a look at [`defaults/main.yml`](defaults/main.yml), it's obnoxiously commented, just for you.

```yaml
---

- hosts: lidarr
  become: true
  vars:
    lidarr_docker_cap_add:
      - CAP_WAKE_ALARM
      - CAP_AUDIT_CONTROL
    lidarr_docker_cap_drop:
      - CAP_CHECKPOINT_RESTORE

    lidarr_docker_container_name: 'lidarr_container'

    lidarr_docker_depends_on:
      - 'my-little service'

    lidarr_docker_devices:
      - '/dev/dri:/dev/dri'
      - '/dev/kfd:/dev/kfd'

    lidarr_docker_env_puid: 5000
    lidarr_docker_env_pgid: 5000

    lidarr_docker_env_timezone: 'Europe/London'

    lidarr_docker_extra_environment_vars:
      SOME_OTHER_VAR: 'A_VALUE'

    lidarr_docker_image_tag: '10.10.6'

    lidarr_docker_labels:
      traefik.enable: 'true'
      traefik.http.routers.traefik-https.entrypoints: 'https'

    # Network mode
    lidarr_docker_network_mode: 'service:glutun'

    # Or external networks:
    lidarr_docker_external_networks:
      - 'my-little-network'

    # Or the default ports
    lidarr_docker_ports:
      - '8989:8989'

    lidarr_docker_pull_policy: 'weekly'

    lidarr_docker_restart_policy: 'always'

    lidarr_docker_additional_volumes:
      - '/path/to/music:/music'
      - '/path/to/usenet:/usenet'
      - '/path/to/torrents:/torrents'

    lidarr_docker_base_directory: '/opt/lidarr_docker'

    lidarr_docker_prune_images: True
    lidarr_docker_prune_until: '24h'

  roles:
    - role: beingtomgreen.lidarr_docker
```

## Role Variables

See [`defaults/main.yml`](defaults/main.yml) for more details.

## Requirements

- Docker & docker compose installed on the target host
- ansible_user will also need to be in the docker group

## Dependencies

- community.docker

## License

[MIT](LICENSE)

## Author Information

[Tom Green](https://github.com/BeingTomGreen)
