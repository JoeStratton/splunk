# Splunk 
This is a Dockerfile for Splunk - https://www.splunk.com

It is designed to deploy Splunk and auto-install any custom apps you want through a volume. The web interface is exposed on port HTTP/8000, data ingest on TCP/9997, and API on HTTPS/8089. 

If you run the Dockerfile with no arguments you will get a single instance of Splunk 7.2.6 with the user admin:changeme2019. 

# Usage
Single instance with no persistence 

`docker run -d -p 8000:8000 -p 8089:8089 -p 9997:9997 --name splunk atunnecliffe/splunk`

Single instance with indexed data persistence

`docker run -d -p 8000:8000 -p 8089:8089 -p 9997:9997 -v /mnt/mysplunkdata:/opt/splunk/var --name splunk atunnecliffe/splunk`

Splunk that auto-installs all apps from volume. 

`docker run -d -p 8000:8000 -p 8089:8089 -p 9997:9997 -v /splunkconfig/heavyforwarder:/apps --name splunk atunnecliffe/splunk`

Splunk on unRAID with indexed data persistence

`docker run -d -p 8000:8000 -p 8089:8089 -p 9997:9997 --build-arg IS_UNRAID=true -v /mnt/user/appdata/splunk:/opt/splunk/var --name splunk atunnecliffe/splunk`

# Arguments
`DOWNLOAD_URL` 

is a direct link to download the .DEB file of your desired release obtained from the "download via WGET" button on the website. This Dockerfile is compatible with versions newer than 7.1.1. Currently defaults to version 7.2.6. 

`ARG SPLUNK_CLI_ARGS` 

What args do you want Splunk to start with every time it opens? Defaults to `--accept-license --no-prompt`, without both of these Splunk will fail to start automatically as it will be waiting for user input. 

`ADMIN_PASSWORD` 

Sets the default "admin" user account password. Defaults to `changeme2019`. You can change this through the web interface once the container is running. 

`IS_UNRAID` 

Defaults to `false`, set to `true` if you're running on unRAID. This just fixes up permissions of the 'nobody' user to match unRAID so you don't run in to permissions issues when editing data in volumes. 

# Volumes

`/apps`

The contents of this directory is forcefully copied into /opt/splunk/etc/apps on container startup. Use this volume to place all your the apps you want pre-installed. 

`/opt/splunk/var` 

Mount this to gain a persistent install for a standalone instance, or an indexer. This is where indexed data is stored. 

`/data` 

For ingesting whatever data from a "local" source. Could potentially map to the root of your external syslog server.
