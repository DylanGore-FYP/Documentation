# Vehicle

## Requirements

- Raspberry Pi
- Micro SD Card
- OBD-II Adapter
- LCD Display (Optional)
- GPS Adapter/HAT

## Preparing the Raspberry Pi

### Install the Operating System

Flash an SD Card with the latest version of Raspberry Pi OS. The recommended way to do this is to use the [Raspberry Pi Imager](https://www.raspberrypi.org/software/) but something like [balenaEtcher](https://www.balena.io/etcher/) can be used if preferred.

This guide assumes that that standard version of Raspberry Pi OS is being used (the one with the desktop environment) but feel free to use Raspberry Pi OS Lite if no display is needed.

<!-- prettier-ignore -->
!!! info "Other Operating Systems"
    While this guide assumes the use of Raspberry Pi OS, the instructions should be very similar for other operating systems that can run on a Raspberry Pi such as Ubuntu.

### Configure Headless Operation

_This step is optional if using a display but makes things a bit easier to configure later._

Plug the SD card into the computer and open the boot partition.

Add a file called `ssh` (with no file extension) to enable the SSH server.

Add a file called `wpa_supplicant.conf` to pre-configure your Wi-Fi network(s). Below is a sample file that has two networks with a different priority, if only one network is needed, remove the second network block. Don't forget to update the country with the [ISO 3166-1 two letter country code](https://en.wikipedia.org/wiki/ISO_3166-1) for your location.

```json
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=IE

network={
    ssid="Network 1"
    psk="PASSWORD"
    priority=1
}

network={
    ssid="Network 2"
    psk="PASSWORD"
    priority=2
}
```

See [this guide](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) on headless setup from the Raspberry Pi website for more information.

If you are running Ubuntu as your operating system of choice, please follow the [official setup guide](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#3-wifi-or-ethernet) as the headless setup is different when using Ubuntu.

## Basic Raspberry Pi Setup

SSH into the Raspberry Pi. Depending on your network it may be accessible over the `raspberrrypi.local` hostname. If not, check the DHCP leases on the router for the IP address or scan the network using a scanning tool such as [nmap](https://nmap.org/).

```bash
ssh pi@raspberrypi.local
```

<!-- prettier-ignore -->
!!! info "Default Password"
    The default password for the `pi` user in Raspberry Pi OS is `raspberry`.

Update the default password:

```bash
passwd pi
```

Update the software on the Raspberry Pi to the latest version.

```bash
sudo apt update
sudo apt upgrade
```

If you need to change any basic settings (hostname, Wi-Fi, language, etc.), this can be done using `raspi-config`:

```bash
sudo raspi-config
```

You may need to reboot at this point.

```bash
sudo reboot now
```

## Ansible Setup

### Ansible Requirements

- [Ansible](https://www.ansible.com/)
- [Python 3](https://python.org)
- [Git](https://git-scm.com/)

### Download the Playbook

To start, clone the playbook from GitHub:

```bash
git clone https://github.com/DylanGore-FYP/ansible-playbook-vehicle-setup.git
```

and change into that directory:

```bash
cd ansible-playbook-vehicle-setup
```

### Update configuration files

The playbook requires that a `config.toml` file is place in the `templates` directory. An example of this file can be found below or in the [DylanGore-FYP/Car](https://github.com/DylanGore-FYP/Car/blob/main/config.sample.toml) repository.

```toml
[obd]
enabled = true
# How often to poll in seconds
poll_interval = 60

[gps]
enabled = false

[vehicle]
friendly_name = ''
manufacturer = ''
model = ''
driver = ''

[plugins.output.mqtt]
enabled = true
host = '127.0.0.1'
port = 1883
username = ''
password = ''
ssl = false
retain = false
pub_qos = 0
sub_qos = 0
base_topic = 'vehicles/vehicle_id'
```

Next, copy the `vars/vars.sample.yml` file to `vars/vars.yml` and edit the values accordingly.

```bash
cp vars/vars.sample.yml vars/vars.yml
```

## Running the playbook

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

### Download the Code

Clone the code from GitHub using git:

```bash
git clone https://github.com/DylanGore-FYP/Car.git
```

Or, download and extract [this](https://github.com/DylanGore-FYP/Car/archive/refs/heads/main.zip) zip file.

### Install the dependencies

```bash
sudo pip3 install obd paho-mqtt toml gps eel
```

<!-- ### Install `pipenv`

This project uses `pipenv` to manage dependencies. To install it, run:

```bash
pip3 install pipenv
```

On Debian-based systems, `pipenv` can also be installed using `apt`:

```bash
apt install pipenv
```

Once `pipenv` is installed, `cd` into the downloaded code and run:

```bash
pipenv install
```

This will install all of the required dependencies in a virtual environment. To access the virtual environment, run:

```bash
pipenv shell
```

**Note:** All instructions after this point assume you are running within the virtual environment unless stated otherwise -->

### Configuration

All configuration is done within a `config.toml` file. A sample file comes with the repo. Copy it and rename it to use it as a starting point.

```bash
cp config.sample.toml config.toml
```

### Running the Code

From the location you downloaded the code to, run:

```bash
python3 -m car
```

### Optional Steps

#### Disable the Cursor

If you want to hide the cursor, first, install `unclutter`

```bash
sudo apt install unclutter
```

Add `unclutter` to the LXDE `autostart` file so it runs on login:

```bash
sudo nano /etc/xdg/libfm/libfm.conf
```

and add the following line to the end:

```bash
@unclutter -idle 0
```

#### Enable single click

As a quality of life improvement when using a touchscreen, you may want to enable single click, meaning that desktop shortcuts only require a single click/tap to open rather than a double click/tap.

Edit `/etc/xdg/libfm/libfm.conf`:

```bash
sudo nano /etc/xdg/libfm/libfm.conf
```

Where it says:

```bash
single-click=0
```

Replace the `0` with a `1`.
