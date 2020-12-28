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
* When ok, merge develop into master and activate master on HA server   
http://192.168.64.10/link/65#bkmrk-all-actions-could-be
 
All actions could be triggered from my laptop, although step 4 can also be done on the HA server after you had a visual look in the HA  (log files, Lovelace, notifications) after the restart with the develop branch
