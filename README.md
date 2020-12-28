# ha_updater
Remote Home Assistant Configuration Updater

This project aims to use a remote development system (Visual Studio Code) for updating Home Assistant configurations files. Still it must be possible to make changes in the Home Assistant Lovelace interface, i.e. scene changes that are more convenient to be done in the interface.

Changes to will be managed by git, to enable history tracking and easy undo in case of failure. My git repository is hosted on my NAS server, but any compatible git system can be used. This project uses two branches: 

* master - the active Home Assistant configuration on the Home Assistant server
* develop - the branch used to edit the configuration

The update cycle of my Home Assistant configuration have the following phases

1. Edit home assistant configuration in Visual Studio Code on develop branch
1. Commit and push development branch to git repository
1. Activate develop branch on HA server, validate and restart HA server
1. Check Home Assistant server if restart did succeed (check log files, notifications and UI)
1. When ok, merge develop into master and activate master on HA server   

All actions could be triggered from the remote development system, although some intermediate steps could also be initiated on the HA server itself or from the Lovelace interface.

The scripts are made based on my specific setup:

* A docker based setup (https://www.home-assistant.io/docs/installation/docker/)
* git is configured to be accessed through SSH, using public keys for authentication

# Requirements
* following software packages need to be installed, on both your development pc and the Home Assistant server:
  * jq
  * head
  * getopt
  * curl 
  * git
  * bash
  
(All these packages seem to be preinstalled in the standard docker homeassistant images)
* It assumed that you already have a git repository to host the updates (create a master and develop branch)
* .HA_VERSION should be in you .gitignore. This way the scripts can detect if you are running on HA server or the development system
* Activate the master branch on the Home Assistent server, activate the development branch on your development system

# Docker
Because of my docker setup, a few special considerations were needed:

* volume mount the homeassistant users .ssh folder into the container, to be able to use the public key authentication when you trigger commands from the Lovelace UI. Here is mine docker-compose.yaml:
```
version: '3'
services:
  homeassistant:
    container_name: home-assistant
    image: homeassistant/raspberrypi4-homeassistant:stable
    volumes:
      - /home/homeassistant/.homeassistant:/config
      - /var/lib/dehydrated/certs/my-host-name:/certs:ro
      - /home/homeassistant/.ssh:/root/.ssh:ro
    environment:
      - TZ=Europe/Amsterdam
    restart: always
    network_mode: host
```
* git user and email need to be setup locally to have it both available within the container as on the HA host itself
```
git config --local user.name "username"
git config --local user.email "email"
```



