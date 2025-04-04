# todo list
- https://www.xrel.to/entertainment-series.html


# StarrComposer

## Introduction

*All Accounts use name `nu03` and password `Sonne-123$` please change this in the settings of the relevant services*
## Sonarr

This is for managint TV series and anime. if a new series is added sonar will scan the attached volumes. 
If the series does not exist sonar will query the conigured indexers and try to find a torrent wich it will pass to the torrent client.
It will monitor the download progress and upon completion it will mark the series as existing.

### Adding indexers

You can either add an indexer like nyaa.si directly or use jackett to manage them and add the Jackett url(add as Torznab)

```bash
NAME="1337X-Torznab"
# note that localhost was replaced with container name cause docker
URL = "http://jackett:9117/api/v2.0/indexers/1337x/results/torznab/"

API_KEY="[insert API key from jackett]"

```

### configuring a torrent client

you can install a torrent client on your host or in a separate docker container.
When adding a connection use the test button to make sure everything is set up correctly.

## Jackett

As far as I can tell Jackett has some advantages:
- Queries are cached wich minimizes indexer requests
- indexers can be configured once and used in multiple apps
- indexers are converted to Torznab format (preferred by Starr Apps)
- you can use Jackett for manual search (good for troubleshooting)



## Torrenting Software

### Transmission

Transmission can set upload to 0 (disable it as it's illegal in CH)
qBit cannot disable Upload only set it to 1 KiB/s

I have found the transmission options tu be clunky and unreliable. Alternatively you can edit the options directly in settings.json
found in the `config/mediarr/transmission` folder

### qBitTorrent

It's just better than Transmission, but you can't set upload to 0.

## Streaming Service

### Jellyfin
basically just make an account,set a password and selecd the `media` folder. all the content iside `media` willl be displayed netflix style.
I choose the option to only allow local connections (localhost) since I will only use this locally. If you want to allow access I suggest you consult the [Servarr wiki](https://wiki.servarr.com/docker-guide).

## Notes
### Links
 using guide from [trash-guides.info](https://trash-guides.info/File-and-Folder-Structure/How-to-set-up/Docker/) 
 and [wiki.servarr.com](https://wiki.servarr.com/docker-guide)

Below i a good guide for a manual installation and configuration of sonarr
what is Sonarr? [ultimate-guide-to-sonarr](https://www.rapidseedbox.com/blog/ultimate-guide-to-sonarr#02)
 
### Folder structure

 ```bash
 data
├── torrents
│  ├── movies
│  ├── music
|  ├── books
│  └── tv
├── usenet
│  ├── movies
│  ├── music
│  ├── books
│  └── tv
└── media
    ├── movies
    ├── music
    ├── books
    └── tv
 ```
Torrenting Software (Traktor, qBit) only need `data/torrents` mounted.
The Starrs need the whole `data` folder mounted.

for testing I used a simpled folder structure

```bash
data
|-- media
|--Downloads
```

Traktor uses `data/downloads and completed files are renamed and moved  `data/media` by Sonarr

## FAQ
Q: My series are downloaded but they are not moved to the `media` folder.
A: check the Logs for a Permission Error. sonarr needs to have write permissions for the media folder.
this should be taken care of by the compose file, but you should check the permissions just to be sure.

```bash
#find container id
docker ps
#open terminal inside the container
docker exec -it [continer id]  sh
#check folder permissions
 ls -ld /[path]
```




![https://www.rapidseedbox.com/wp-content/uploads/image3-18.png](https://www.rapidseedbox.com/wp-content/uploads/image3-18.png)