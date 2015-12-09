# docker plexpass

This is a Dockerfile to set up (https://plex.tv/ "Plex Media Server") - (https://plex.tv/)

This is for the paid for plexpass version

Build from docker file

```
git clone git@github.com:timhaak/timhaak/docker-plexpass.git
cd timhaak/docker-plexpass
docker build -t timhaak/plexpass .
```

You can also obtain it via:

```
docker pull timhaak/plexpass
```

---
Instructions to run:

```
docker rm -f plex
docker run --restart=always -d --name plex -h *your_host_name* -v /*your_config_location*:/config -v /*your_videos_location*:/data -p 32400:32400  timhaak/plexpass
```
or for auto detection to work add --net="host". Though be aware this more insecure and not best practice with docker images.

The only reason for doing it is to allow Avahi to work (As it uses broadcasts will not cross network boundries). If anyone knows of a better method please shout and we can get it added.

See https://docs.docker.com/articles/networking/#how-docker-networks-a-container

```
docker rm -f plex
docker run --restart=always -d --name plex --net="host" -h *your_host_name* -v /*your_config_location*:/config -v /*your_videos_location*:/data -p 32400:32400  timhaak/plexpass
```

The first time it runs, it will initialize the config directory and terminate. (This most likely won't happen if you've used the --net="host")

You will need to modify the auto-generated config file to allow connections from your local IP range (This should not be needed if you've used the --net="host"). This can be done by modifying the file:

*your_config_location*/Library/Application Support/Plex Media Server/Preferences.xml

and adding ```allowedNetworks="192.168.1.0/255.255.255.0" ``` as a parameter in the <Preferences ...> section. (Or what ever your local range is)

Start the docker instance again and it will stay as a daemon and listen on port 32400.

Browse to: ```http://*ipaddress*:32400/web``` to run through the setup wizard.

