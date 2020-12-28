# ha_updater
Remote Home Assistant Configuration Updater

This project aims to use a remote development system (Visual Studio Code) for updating Home Assistant configurations files. Still it must be possible to make changes in the Home Assistant Lovelace interface, i.e. scene changes that are more convenient to be done in the interface.

Changes to will be managed by git, to enable history tracking and easy undo in case of failure. My git repository is hosted on my NAS server, but any compatible git system can be used. This project uses two branches: 

* master - the active Home Assistant configuration on the Home Assistant server
* develop - the branch used to edit the configuration

The update cycle of my Home Assistant configuration have the following phases

* Edit home assistant configuration in Visual Studio Code on develop branch
* Commit and push development branch to git repository
* Activate develop branch on HA server, validate and restart HA server
* Check Home Assistant server if restart did succeed (check log files, notifications and UI)
* When ok, merge develop into master and activate master on HA server   

All actions could be triggered from the remote development system, although some intermediate steps could also be initiated on the HA server itself or from the Lovelace interface.

The scripts are made based on my specific setup:

* A docker based setup (https://www.home-assistant.io/docs/installation/docker/)
* git is configured to be accessed through SSH, using public keys for authentication
