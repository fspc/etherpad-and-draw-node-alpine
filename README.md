# etherpad-and-draw-node-alpine

## About
Designed as a plugin for the [Yellow Bike Database](https://github.com/fspc/Yellow-Bike-Database), which is a specialized software for Community Bike Shops, [etherpad-and-draw-node-alpine](https://github.com/fspc/etherpad-and-draw-node-alpine) can be used for any project that wants to run lightweight alpine linux based dockerized containers with etherpad with etherdraw functionality built-in with an easy to setup docker-compose file.

The etherpad Dockerfile and entrypoint.sh is a fork of the work by [tvelocity](https://github.com/tvelocity/dockerfiles/tree/master/etherpad-lite) altered to work with alpine linux and rehashed to work with etherdraw. Tvelocity's own work was inspired by the official Wordpress Dockerfile and johbo's etherpad-lite image. That's how Free Software works!

## How to Use

The recommended way is to run `docker-compose up -d`. Out of the box, there is only one requirement: There must be a running container created from one of the official mysql images like [MariaDB](https://hub.docker.com/_/mariadb/). The default name for the container is `mysql`, but you can change the name in .env to whatever you have named your own mysql container:

`SQL_CONTAINER=my_own_mysql_container_name`

This will bring up two containers on port 9001 (etherpad) and port 9002 (etherdraw). If your are running on localhost, etherdraw should work properly within etherpad. If you are on a network, simply add this line to .env:

`DRAW_HOST=the_name_of_the_domain_name_you_are_using.org`

There are several other environmental variables you can change where VARIABLE=default value:

```
PAD_DATABASE=pad
PAD_PASSWORD=password
DB_HOST=mysql (must be the same value as SQL_CONTAINER)
PAD_TITLE=Online Flip Chart
ADMIN_PASSWORD=admin_password
ADMIN_USER=admin
DRAW_DATABASE=draw
DRAW_PASSWORD=password
DRAW_TITLE=Ether Draw
DRAW_PORT=9002
```

## Advanced environmental changes
Two empty files, `pad_environment` and `draw_environment` are included to add your own environmental variables and values. For instance, I enjoy using [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion), so here is what I add:

###### pad_environment
```
VIRTUAL_HOST=demopad.bikelover.org
LETSENCRYPT_HOST=demopad.bikelover.org
LETSENCRYPT_EMAIL=bike@bikelover.org
```

###### draw_environment
```
VIRTUAL_HOST=demodraw.bikelover.org
LETSENCRYPT_HOST=demodraw.bikelover.org
LETSENCRYPT_EMAIL=bike@bikelover.org
```

## EtherPad Plugins
Here are the default plugins installed by npm for etherdraw:

```
    ep_adminpads ep_align ep_authornames ep_comments_page \
    ep_copy_paste_images ep_define ep_draw ep_font_color ep_font_family \
    ep_font_size ep_headings ep_message_all ep_offline_edit \ 
    ep_page_view ep_print ep_simpletextsearch ep_spellcheck
```                

To change, simply edit [etherpad/Dockerfile](https://github.com/fspc/etherpad-and-draw-node-alpine/blob/master/etherpad/Dockerfile) after `RUN npm install`.

If any of the plugins require configuration, simply edit [etherpad/entrypoint.sh](https://github.com/fspc/etherpad-and-draw-node-alpine/blob/master/etherpad/entrypoint.sh) between `cat <<- EOF > settings.json` and `EOF`. 




