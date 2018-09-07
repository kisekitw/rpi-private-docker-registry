# rpi-private-docker-registry

## Construct Private Registry on Raspberry Pi 3+

1. Pull and run image from docker hub

~~~Docker Pull
docker run -d -p 5000:5000 --restart=always  -v /opt/data/registry:/var/lib/registry cblomart/rpi-registry
~~~

2. Mount local(on Raspberry Pi 3+) folder to container

~~~Docker instruction
-v localfolder:containerfolder
~~~

3. Modify /etc/default/docker, **clientside as well.**

~~~Docker
DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4 --insecure-registry [host ip]:[host port]"
~~~

4. **Maybe needed!!**
Create and Modify daemon.json

~~~script
sudo touch /etc/docker/daemon.js
sudo nano /etc/docker/daemon.js
~~~

Add folowing script

~~~script
{ "insecure-registries":["host ip:host port"] }
~~~

Then restart docker service

~~~script
sudo service docker restart

~~~


## Dockerize Python Program

1. Constructing Dockerfile for python-3.5 environment on Raspberry Pi 3+

~~~Dockerfile
FROM resin/raspberrypi3-python:3.5

RUN pip install modbus_tk

ADD . /app

CMD [ "python3", "./app/tcp_master.py" ]

# Testing
#RUN python -c "from __future__ import print_function"
#RUN python -c "import modbus_tk"
#RUN python -c "import modbus_tk.defines as cst"
#RUN python -c "from modbus_tk import modbus_tcp, hooks"
#RUN python -c "import logging"
~~~

2. Build Dockerfile

~~~CMD
docker build -t modbus_master .
~~~

3. Tag image to private registry

4. Push image to private registry

## Clientside
1. Pull image from private registry

2. **Maybe needed!!**
Create and Modify daemon.json

~~~script
sudo touch /etc/docker/daemon.js
sudo nano /etc/docker/daemon.js
~~~

Add folowing script

~~~script
{ "insecure-registries":["host ip:host port"] }
~~~

Then restart docker service

~~~script
sudo service docker restart

~~~

## Modify Registry IP
1. Modify /etc/default/docker

~~~Docker
DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4 --insecure-registry **[host ip]:[host port]**"
~~~



