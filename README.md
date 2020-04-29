<h1>Docker Media component</h1>

This docker-compose project was created to move on an easy way the different application that i use for manage the media repositories.<br>

Components: 
 - Radarr (Movies)
 - Sonarr (Series)
 - Bazarr (Subtittles)
 - Tautulli (Plex monitoring)
 - Transmission (Torrent manager)
 - Netdata (Hardware monitoring)

<br>

The configuration for this project is based on the following file path:
 ```
 -mnt
 |-media
  |-appdata
  |-downloads
  |-movies
  |-tv_shows
 ```
|Folder        | Description                                  |
|--------------|:--------------------------------------------:|
| **appdata**  |is saved all the applications configurations  |
|**downloads** |is the downloads folder for transmission      |
|**movies**    |the location to store the movies              |
|**tv_shows**  |The series location                           |


By default, all the information is stored on the /mnt/media folder, but you can create syms lynks to use your locations, or update the docker-compose.yml with your custom folders.<br>

<h3>Docker env file</h3>

The docker-compose was configured to work with the .env configurations, that is a file where you define some constants attributes used on all the yml document. 

``` yaml

   HOST=192.168.0.XXX
   PUID=1000
   PGID=1000
   DOCKER_PGID=99X
   TR_USER=xxx
   TR_PASS=xxx
   TZ=America/Bogota
```

As you can see there are some properties required by each container.

|Attribute | Description                                                                                                      |
|----------|:----------------------------------------------------------------------------------------------------------------:|
|HOST      |Is network ip where remains your docker server (ie. 192.168.0.12)                                                 |
|PUID      |Some apps like radarr or sonarr move and create files, this is the user to use for that                           |
|PGID      |This is the group to use when the files are created                                                               |
|DOCKER_PGID | This variable is used for netdata, to retrieve the names of the docker containers, instead of the docker uuid  |
|TR_USER | This is the user for transmission                                                                                  |
|TR_PASS | This is the password defined for transmission                                                                      |
|TZ      | Your timezone, for some containers, better have all with the same region                                           |


Useful commands: </br>
- PUID and PGID can be found using ``` id $user ``` on a linux shell.
- DOCKER_PGID can be retrieved with the command ```grep docker /etc/group | cut -d ':' -f 3  ``` only works when you have docker installed directly on your system (don't work when you have docker installed using snap).


Useful steps:


FSTAB configuration for NTFS disks
- Configure the system to mount disk on startup.

  UUID=055577F83C6CFCAA /media/lordvault/055577F83C6CFCAA   ntfs   defaults  0  2<br>
  UUID=69B2BC0B12B958CC /media/lordvault/69B2BC0B12B958CC   ntfs   defaults  0  2


Create syms lynks.
- ln -s /media/lordvault/055577F83C6CFCAA/media/ /mnt/media/
- ln -s source_file symbolic_link
