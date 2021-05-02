# Cloud - Single Server Setup

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

[Setup Firebase :material-firebase:](external-firebase-authentication.md){ .md-button .md-button--primary }

Once you have a `serviceAccountKey.json` file, place it in the Playbook `templates/` directory.

### Variable Configuration

The variable names should be self-explanatory, please create `vars/vars.yml` based on the example file found in the `vars/` directory.

### Configuration Files

You must also include two custom configuration files, one for Alert Manager and one for configuring MQTT login credentials. There are samples for both in the `templates/` directory. Please create both an `mqtt_logins` file and an `alertmanager.yml` file. The playbook will fail if these files are not present.

### Running the Playbook

Before running the playbook itself, you must download the required roles. This can be done by running:

```bash
ansible-galaxy install -r requirements.yml -p roles --force
```

To run the playbook, run one of the following commands depending on how authentication is configured. If in doubt, run the password authentication command.

Run the playbook using SSH Key authentication:

```bash
ansible-playbook -i inventory playbook.yml
```

Run the playbook using password authentication:

```bash
ansible-playbook -i inventory playbook.yml --ask-pass
```

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

## Next Steps

From here you can deploy the various services as Docker containers. Please see the Ansible role linked above and/or the individual component repositories for examples.
