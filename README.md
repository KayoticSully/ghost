## Ghost Dockerfile


This repository contains **Dockerfile** of [Ghost](https://www.ghost.org/) for [Docker](https://www.docker.com/)'s [automated build](https://registry.hub.docker.com/u/dockerfile/ghost/) published to the public [Docker Hub Registry](https://registry.hub.docker.com/).


### Base Docker Image

* [dockerfile/nodejs](http://dockerfile.github.io/#/nodejs)


### Installation

1. Install [Docker](https://www.docker.com/).

2. Download [automated build](https://registry.hub.docker.com/u/dockerfile/ghost/) from public [Docker Hub Registry](https://registry.hub.docker.com/): `docker pull dockerfile/ghost`

   (alternatively, you can build an image from Dockerfile: `docker build -t="dockerfile/ghost" github.com/dockerfile/ghost`)


### Usage

    docker run -d -p 80:2368 dockerfile/ghost

#### Customizing Ghost

    docker run -d -p 80:2368 -v <override-dir>:/ghost-override dockerfile/ghost

where `<override-dir>` is an absolute path of a directory that could contain:

  - `config.js`: custom config file copied from [here](https://github.com/TryGhost/Ghost/blob/master/config.example.js) (you must replace `127.0.0.1` with `0.0.0.0`)
  - `content/data/`: persistent/shared data
  - `content/images/`: persistent/shared images
  - `content/themes/`: more themes

After few seconds, open `http://<host>` for blog or `http://<host>/ghost` for admin page.


#### Running Ghost in development environment with forever

When running Ghost in node development environment

    docker run -d -p 80:2368 -e NODE_ENV=development -e WATCH_DIRECTORY=<dir-to-watch> -v <override-dir>:/ghost-override dockerfile/ghost

Ghost starts with the help of forever https://www.npmjs.com/package/forever and its watchDirectory option.
This lets us develop on a shared `ghost-override` volume and forever watch for file changes in `watchDirectory` and restart our Ghost process without stoping docker.

e.g.
    
    docker run --rm -e NODE_ENV=development -e WATCH_DIRECTORY=content/themes/my_theme -v /home/my_localhost_ghost:/ghost-override -p 2368:2368 dockerfile/ghost
