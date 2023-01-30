# Docker Apps Config

There are Docker Compose configs, guides and notes for myself about various self-hosted applications. This is primarily a personal reference though it could be used as a starting point for others.

- [Bitwarden](#bitwarden)
    - [Initial Setup](#initial-setup)
    - [Usage](#usage)
    - [Backup \& Restore](#backup--restore)
- [ddclient](#ddclient)
    - [Initial Setup](#initial-setup-1)
    - [Usage](#usage-1)
    - [Backup \& Restore](#backup--restore-1)
- [Resilio Sync](#resilio-sync)
    - [Initial Setup](#initial-setup-2)
    - [Usage](#usage-2)
    - [Backup \& Restore](#backup--restore-2)
- [SimpleHelp](#simplehelp)
    - [Initial Setup](#initial-setup-3)
    - [Usage](#usage-3)
    - [Backup \& Restore](#backup--restore-3)


## Bitwarden
Bitwarden has an [official self hosting guide](https://bitwarden.com/help/install-on-premise-linux/). However, my own setup steps are as follows:

### Initial Setup
```bash
# Create a directory for Bitwarden data to live.
mkdir -p ~/docker-apps/bitwarden && cd ~/docker-apps/bitwarden
# Add primary user to Docker group to be able to use Docker Compose.
sudo usermod -aG docker $(whoami)
# Download Bitwarden's setup script.
curl -Lso bitwarden.sh https://go.btwrdn.co/bw-sh
# Install Bitwarden
./bitwarden.sh install
```

Once running, a path needs to be made such that:
```
https://bitwarden.justincampbell.dev:443/ -> http://localhost:8082
```

#### Replace Default Settings
##### Ports
```
nano bwdata/config.yml
```
```
http_port: 8081
https_port:
```

##### SMTP Details
```
nano bwdata/env/global.override.env
```
```
globalSettings__mail__replyToEmail=no-reply@bitwarden.justincampbell.dev
globalSettings__mail__smtp__host=smtp.gmail.com
globalSettings__mail__smtp__username=SEE BITWARDEN `Bitwarden SMTP`
globalSettings__mail__smtp__password=SEE BITWARDEN `Bitwarden SMTP`
globalSettings__mail__smtp__ssl=true
globalSettings__mail__smtp__port=587
```

#### Rebuild settings.
```
./bitwarden.sh rebuild
```

### Usage
| Action | Command |
| - | - |
| Start | `./bitwarden.sh start` |
| Stop | `./bitwarden.sh stop` |
| Restart | `./bitwarden.sh restart` |

### Backup & Restore
- Export passwords from Bitwarden web as a CSV
- Reinstall Bitwarden using initial setup instructions
- Create a new account in Bitwarden
- Import passwords from CSV

## ddclient
ddclient has some [configuration and usage docs.](https://ddclient.net/)
### Initial Setup
Replace login details in `config/ddclient/config/ddclient.conf` with details in Bitwarden and run:
```bash
docker compose up -d
```
### Usage
| Action | Command |
| - | - |
| Start | `docker compose up -d` |
| Stop | `docker compose down` |
| Restart | `docker compose down && docker compose up -d` |

Additionaly, the config file holds links to relevant boards talking about configuration.

### Backup & Restore
There is no persistent data to backup. Only the config file is needed.

## Resilio Sync
### Initial Setup
Confirm paths in the volumes section of `docker-compose.yml` exist and the PUID/GUID match existing permissions then run:
```bash
docker compose up -d
```

Once running, a path needs to be made such that:
```
https://sync.justincampbell.dev:443/ -> http://localhost:9999
```

### Usage
| Action | Command |
| - | - |
| Start | `docker compose up -d` |
| Stop | `docker compose down` |
| Restart | `docker compose down && docker compose up -d` |

If things start to appear as `root` without PUID/GUID being set to `root`, `chown -R USER:GROUP /PATH-TO-DATA/` may help.

### Backup & Restore
Backup host volume data and recreate the container.

## SimpleHelp
### Initial Setup
Create the `configuration` folder by unzipping a previous backup and running:
```
docker compose up -d
```
Once running, paths needs to be made such that:
```
https://simplehelp.justincampbell.dev:443/ -> http://localhost:8082
http://justin.graces.com:80/ -> http://localhost:8082
```

### Usage
| Action | Command |
| - | - |
| Start | `docker compose up -d` |
| Stop | `docker compose down` |
| Restart | `docker compose down && docker compose up -d` |

### Backup & Restore
Backup configuration directory and recreate container.