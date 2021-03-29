# Cloud - Single Server Setup

<!-- prettier-ignore -->
!!! warning
    This section of the documentation is incomplete! Results may vary.

## Requirements

- 1 Virtual Machine/Bare-metal server

## Ansible Setup

Coming soon!

## Manual Setup

<!-- prettier-ignore -->
!!! info
    This guide is based of the use of Ubuntu 20.04 as the operating system.

### Initial Server Setup

Update the server

```bash
sudo apt update
sudo apt upgrade
```

### Install Docker

Install Docker using the [official installation guide](https://docs.docker.com/engine/install/ubuntu/). Don't forget to follow the [post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/) to create the Docker group and enable it to start on boot.

### Install `docker-compose`

Below are the required parts of the docker-compose installation guide. The full guide is available [here](https://docs.docker.com/compose/install/).

Download the latest stable version of `docker-compose`:

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Make it executable:

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Optionally, you can enable command completion for `docker-compose` by following the guide [here](https://docs.docker.com/compose/completion/).
