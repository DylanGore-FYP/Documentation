# Cloud - Single Server Setup

<!-- prettier-ignore -->
!!! warning
    This section of the documentation is incomplete! Results may vary.

## Requirements

- 1 Virtual Machine/Bare-metal server

## Ansible Setup

### Download the Playbook from GitHub

This can either be done by cloning the Git repository or downloading it as a zip file.

#### Clone the Repository

```bash
git clone https://github.com/DylanGore-FYP/ansible-playbook-docker-compose-deploy.git
```

#### Direct Download

```bash
wget https://github.com/DylanGore-FYP/ansible-playbook-docker-compose-deploy/archive/refs/heads/main.zip
```

Unzip the file:

```bash
unzip main.zip
```

### Add the Firebase Admin Credentials File

If you have already setup Firebase, you should have a `serviceAccountKey.json` file if not, you should setup Firebase before continuing.

[Setup Firebase :material-firebase:](/setup/external-firebase-authentication){ .md-button .md-button--primary }

Once you have a `serviceAccountKey.json` file, place it in the Playbook `templates/` directory.

### Variable Configuration

Coming Soon!

### Running the Playbook

Coming Soon!

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
