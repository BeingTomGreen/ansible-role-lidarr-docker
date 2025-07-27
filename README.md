# beingtomgreen.lidarr_docker

A simple role for running Lidarr via Docker compose.

## Installation & usage

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

Now you're free to use it within your project:

```yaml
---

- hosts: lidarr
  become: true
  vars:
    lidarr_docker_environment:
      PUID: '6900'
      PGID: '6900'
      TZ: 'Europe/London'

    lidarr_docker_volumes:
      - "{{ lidarr_docker_base_dir }}/config:/config"
      # The default way
      - '/path/to/music:/music'
      - '/path/to/download-client-downloads:/downloads'
      # Better, for atomic moves/hard links
      # See https://wiki.servarr.com/docker-guide#consistent-and-well-planned-paths
      # See https://trash-guides.info/File-and-Folder-Structure/Hardlinks-and-Instant-Moves/
      # - '/path/to/data:/data'

    lidarr_docker_networks:
      - 'arrrrrr'

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
