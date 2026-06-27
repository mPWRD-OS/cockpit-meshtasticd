# cockpit-meshtasticd

A [Cockpit](https://cockpit-project.org/) page for managing a
[meshtasticd](https://meshtastic.org/docs/hardware/devices/linux-native-hardware/)
installation: service status, logs, and YAML config editing.

Cockpit is a web-based server administration interface for Linux that runs on
port 9090. This module adds a **Meshtasticd** entry to its menu.

## Install Cockpit (if not already installed)

**Debian / Ubuntu / Armbian:**

    sudo apt update
    sudo apt install cockpit

**Fedora / RHEL / CentOS:**

    sudo dnf install cockpit

Then start it:

    sudo systemctl enable --now cockpit.socket

Open `https://<host>:9090` in your browser and log in with a system user.

## Install the module

### Pre-built tarball (no build tools required)

Download the latest `cockpit-meshtasticd-*.tar.gz` from
[Releases](https://github.com/chris-casper/cockpit-meshtasticd/releases), then:

    sudo mkdir -p /usr/local/share/cockpit/meshtasticd
    sudo tar -xzf cockpit-meshtasticd-*.tar.gz -C /usr/local/share/cockpit/meshtasticd

### From source

Requires Node.js and npm.

    git clone https://github.com/chris-casper/cockpit-meshtasticd.git
    cd cockpit-meshtasticd
    make
    sudo mkdir -p /usr/local/share/cockpit
    sudo ln -sfn "$(pwd)/dist" /usr/local/share/cockpit/meshtasticd

Reload Cockpit (Ctrl+Shift+R). The **Meshtasticd** entry will appear in the menu.

## Permissions

Editing YAML files requires root. In Cockpit, click **Limited access** at the
top of the page and switch to **Administrative access** before saving.

## Configuring the meshtasticd instance

Defaults (systemd unit `meshtasticd`, config at `/etc/meshtasticd/config.yaml`,
radio configs in `/etc/meshtasticd/config.d/`) work for stock installs.

To point at a different instance, edit `src/settings.ts` and rebuild with
`make`. Each entry in `INSTANCES` specifies a unit name, main config path,
and per-radio config directory.

## Tabs

- **Overview** — service installed / running status
- **Logs** — recent `journalctl -u <unit>` output
- **Config** — edit `config.yaml` (writes `.cockpit_backup` on save)
- **Radio Config** — edit any YAML in the radio config directory

## License

LGPL-2.1-or-later