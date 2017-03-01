[![](https://images.microbadger.com/badges/image/thomx/duplicati.svg)](https://microbadger.com/images/thomx/duplicati "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/thomx/duplicati.svg)](https://microbadger.com/images/thomx/duplicati "Get your own version badge on microbadger.com") [![GitHub license](https://img.shields.io/badge/license-AGPL-blue.svg)](https://raw.githubusercontent.com/Thom-x/docker-duplicati/master/LICENSE)

# Duplicati #
Duplicati is a backup client that securely stores encrypted, incremental, compressed backups on cloud storage services and remote file servers. It works with Amazon S3, Windows Live SkyDrive, Google Drive (Google Docs), Rackspace Cloud Files or WebDAV, SSH, FTP (and many more). Duplicati is open source and free.

Duplicati has built-in AES-256 encryption and backups can be signed using GNU Privacy Guard. A built-in scheduler makes sure that backups are always up-to-date. Last but not least, Duplicati provides various options and tweaks like filters, deletion rules, transfer and bandwidth options to run backups for specific purposes.

Duplicati is licensed under LGPL and available for Windows and Linux (.NET 2.0+ or Mono required). The Duplicati project was inspired by duplicity. Duplicati and duplicity are similar but not compatible. Duplicati is available in English, Spanish, French, German, Danish, Portugese, Italian, and Chinese.

### Duplicati Features ###
* Duplicati uses AES-256 encryption (or GNU Privacy Guard) to secure all data before it is uploaded.
* Duplicati uploads a full backup initially and stores smaller, incremental updates afterwards to save bandwidth and storage space.
* A scheduler keeps backups up-to-date automatically.
* Encrypted backup files are transferred to targets like FTP, Cloudfiles, WebDAV, SSH (SFTP), Amazon S3 and others.
* Duplicati allows backups of folders, document types like e.g. documents or images, or custom filter rules. 
* Duplicati is available as application with an easy-to-use user interface and as command line tool.
* Duplicati can make proper backups of opened or locked files using the Volume Snapshot Service (VSS) under Windows or the Logical Volume Manager (LVM) under Linux. This allows Duplicati to back up the Microsoft Outlook PST file while Outlook is running.


### Start duplicati with web interface ###
To start with the web interface, run the following command:
```bash
docker create \
	--name=duplicati \
	-e PUID=<UID> -e PGID=<GID> \
	-e DUPLICATI_PASS=duplicatiPass \
	-e TZ=<timezone> \
	-v </path/to/config>:/home/duplicati/.config/Duplicati/ \
	-v </path/to/data>:/data \
	-p 8200:8200 \
	thomx/duplicati
```

## Parameters

The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.


* `-v /home/duplicati/.config/Duplicati` - Duplicati config file.
* `-v /data/xyz` - Source backup folder goes here. Add as many as needed e.g. `/data/movies`, `/data/tv`, etc.
* `-e PGID=` for for GroupID - see below for explanation
* `-e PUID=` for for UserID - see below for explanation
* `-e TZ` - for timezone information *eg Europe/London, etc*
* `-e DUPLICATI_PASS` - Webapp password.

It is based on ubuntu xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it duplicati /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" <sup>TM</sup>.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <duplicati>
    uid=1001(duplicati) gid=1001(duplicati) groups=1001(duplicati)
```

## Setting up the application
Webui can be found at `<your-ip>:8200`

## Info

* Shell access whilst the container is running: `docker exec -it duplicati /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f duplicati`
