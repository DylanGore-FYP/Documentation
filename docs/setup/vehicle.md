# Vehicle

<!-- prettier-ignore -->
!!! warning
    This section of the documentation is incomplete! Results may vary.

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

## Ansible Setup

Coming soon!

## Manual Setup

### Basic Raspberry Pi Setup

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

### Download the Code

<!-- prettier-ignore -->
!!! info
    This is a temporary solution, the final instructions will have an easier installation method.

Clone the code from GitHub using git:

```bash
git clone https://github.com/DylanGore-FYP/Car.git
```

Or, download and extract [this](https://github.com/DylanGore-FYP/Car/archive/refs/heads/main.zip) zip file.

### Install `pipenv`

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

**Note:** All instructions after this point assume you are running within the virtual environment unless stated otherwise

### Configuration

All configuration is done within a `config.toml` file. A sample file comes with the repo. Copy it and rename it to use it as a starting point.

```bash
cp config.sample.toml config.toml
```
