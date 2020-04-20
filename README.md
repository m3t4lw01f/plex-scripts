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

# Testing the configuration
* Run `touch ${HOME}/localmedia/file` and you should notice a file get created in that location, and it should be  visible at `${HOME]/media` due to the merger-fs file system.
* Run `./uploader` and wait for it to finish. You should now see that `file` is gone from `${HOME}/localmedia` and exists in `${HOME}/remotemedia` but still also visible in `${HOME}/media`.

If the above worked, continue by setting up cron to keep things mounted and updated:
`*/1 * * * * /bin/bash /home/user/bin/check.mount >> /home/user/logs/check.mount.log`  
`0 6  * * * /bin/bash /home/user/bin/uploader >> /home/user/logs/uploader_morning.log`  

The first checks the mount every minute and re-mounts if it has failed. The second uploads everything that's in localmedia to your cloud storage.  

From here, configure all of your applications to use `${HOME}/media` as their directory and it should continue to work as if everything is local to you, and transparently keep everything uploaded to your Google Drive.
