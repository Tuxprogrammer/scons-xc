# scons-xc Dockerfile
## Build environment for the Microchip MPLAB-XC16 IDE with SConstruct for Bitbucket Pipelines
### Author: Spencer Callicott <tuxprogrammer@gmail.com>

# Downloading and Starting Instance
To download and run this repository, you can begin with:

```bash
docker pull tuxprogrammer/scons-xc:xc16
docker run -it --entrypoint /bin/bash --name xc16_env tuxprogrammer/scons-xc:xc16
```

After docker initializes the container, it will spawn a root shell on the instance. 

# Building pic24lib_all
Next, download the `pic24lib_all` repository and build with the following:

```bash
hg clone https://bitbucket.org/bjones/pic24lib_all/
cd pic24lib_all
/usr/bin/env python3 $(which scons)
```

# Resuming Instance
To quit the interactive session, simply `exit` from the bash instance.

To resume a previous session, locate the instance id with:
```bash
docker container ls --all
```
Which will look similar to the following:
```
CONTAINER ID        IMAGE                         COMMAND      CREATED             STATUS                     PORTS   NAMES
1cfb8e38337c        tuxprogrammer/scons-xc:xc16   "/bin/bash"  5 minutes ago       Exited (0) 4 minutes ago           xc16_env
```

Run the following command to reattach to the session:
```bash
docker start xc16_env && docker attach xc16_env
```

# FAQ
## Docker failed with 'Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock:'`

You have to run all docker commands as superuser. A simple fix for this is to put `alias docker="sudo docker"` in `~/.bashrc` so that docker commands are always executed as superuser.

---

## Please send any questions/suggestions to Spencer Callicott <tuxprogrammer@gmail.com>

## Disclaimer:

>This program is free software: you can redistribute it and/or modify
>it under the terms of the GNU General Public License as published by
>the Free Software Foundation, either version 3 of the License, or
>(at your option) any later version.
>
>This program is distributed in the hope that it will be useful,
>but WITHOUT ANY WARRANTY; without even the implied warranty of
>MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
>GNU General Public License for more details.