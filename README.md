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
``[Service]  
User=myuser  
Group=myuser``  

Then:  
`sudo systemctl daemon-reload`  
`sudo service plexmediaserver start`  


