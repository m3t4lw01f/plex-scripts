# plex-scripts
Scripts for mounting Google Drive on Linux as storage for Plex

# Requirements
mergerfs  
rclone  
fuse

# Other Requirements
Plex must run as the same user as the scripts. On Linux you can do this from the command line:  
`sudo service plexmediaserver stop`  
`sudo chown -R myuser:myuser /var/lib/plexmediaserver`  
`sudo systemctl edit plexmediaserver`  

This is the content to be placed in the editor  
`[Service]`  
`User=myuser`  
`Group=myuser`  

Then:  
`sudo systemctl daemon-reload`  
`sudo service plexmediaserver start`  

# Steps
* `sudo apt install merger-fs`
* Install rclone from rclone.org
* Create a remote in rclone that points at a folder in Google Drive. It should be a 'Drive' remote with access to either the top level of your drive. No advanced configuration is required, as our rclone mount command will configure the mount.  
* Clone the repo to a folder on your computer and make sure the scripts are all executable
* Set the primary remote to what you named the rclone remote and set the folders you wish to use in config.conf
* Create the local folders `${HOME}/media` `${HOME}/localmedia` and `${HOME}/remotemedia` (these are the defaults in the config file)
* On cloud storage, create a folder called `Media` at the top level.
* In that media folder create a file called `remote-check`
* In `${HOME}/localmedia` create a file called `local-check`
* Run `check.mount`.

If the above was successful, you should see the contents of both `${HOME}/localmedia` and `${HOME]/remotemedia` at `${HOME}/media`. If not, figure that out before proceeding setting up your other tools.
