flamejam - a game jam application using Flask
=============================================

[![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/svenstaro/flamejam)](https://hub.docker.com/r/svenstaro/flamejam)
[![license](http://img.shields.io/badge/license-zlib-blue.svg)](https://github.com/svenstaro/flamejam/blob/master/LICENSE)
[![Stars](https://img.shields.io/github/stars/svenstaro/flamejam.svg)](https://github.com/svenstaro/flamejam/stargazers)

Description
-----------
flamejam is a generic game jam application implemented in Python and using the
Flask microframework.
It was initially created as a voting platforms for the [BaconGameJam](http://www.reddit.com/r/BaconGameJam).
However, it is generic and as such it is usable for any other game jam event.

This application is designed to make sure that participants vote on other
entries fairly and evenly.

How to run for development
--------------------------
You are currently expected to run some kind of POSIX system such as Linux. This software has
not been tested on Windows and it would be quite a wonder indeed if it worked there.

1.  Copy the default config from `doc/flamejam.cfg.default` to `flamejam.cfg`
    and configure it to your needs.
2.  Initialize the database using either test data or an admin account.

    Example for an empty database with just an admin account called `peter` and password `hunter2`:

        poetry run flask init-db peter hunter2 peter@example.com

    Example to seed a database with test data:

        poetry run flask seed-db
3.  Then, running the application should be as simple as calling

        make run_debug

How to run in production
------------------------
Docker is the primary supported way to run this software in production. It can
easily be run without Docker but it's very hard for me to universally support
that so if you don't want to use Docker, you should adopt the following
instructions to your own systems. Follow step 1. and 2. from above. Then, run

    docker run -v $PWD/flamejam.cfg:/etc/flamejam/flamejam.cfg -p 8080:8080 svenstaro/flamejam:latest

Support and contact
-------------------
In order to receive help in getting this application to run, it would be best
to ask here on GitHub via the issues system.

Pull requests are welcome.

License
-------
This application, all of its sources and resources are licensed under the zlib license with the
following exceptions:

 - jquery
 - lightbox
 - bootstrap

These exceptions are subject to their own copyrights and licenses. This project only makes use of them.

For the full license text, please see the included LICENSE file.


Run with docker
---------------
Flamejam comes with a prepared Dockerfile in order to run it as a container on your system.
Running flamejam with docker saves you installing all the required dependencies and encapsulates it inside a contained system.

To build the image you need to run as a container you call

    docker build --tag flamejam-image .
    
where `flamejam-image` can be freely chosen to name the image. You need to remember it in order to select it for the container.
Building the container concludes with

    Successfully built 6370906318a8
    Successfully tagged flamejam-image:latest

By running `docker images` you can see the built image.

    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    flamejam-image      latest              6370906318a8        24 seconds ago      156MB
    alpine              3.6                 43773d1dba76        9 months ago        4.03MB

Now you can start the container

    docker run --name flamejam flamejam-image
    
and it is up and running. Executing `docker ps` confirms your running container.

    CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
    16e378d4c10f        flamejam-image      "uwsgi deploy/uwsgi.…"   32 seconds ago      Up 31 seconds       8080/tcp            flamejam
    
In order to continue with setting up flamejam you can login to the system by typing

    docker exec -it flamejam sh

Run with docker-compose
-----------------------
If you don't like the manual docker running or don't care about configuration options, you can install and use `docker-compose` instead.

    docker-compose up --build
    
This uses the provided `docker-compose.yml` configuration. Afterwards you can reach the server in the same way as described above.
Enter http://localhost:8080 to access the frontpage of the application