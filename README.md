# ITI - Docker Lab 2 🐋

## Part 1: Run a container using ngins image, and mount a directory from your host into the Docker container, example: /home/samy/nginx:/home/nginx (bind mount)
```bash 
   1- create Bind Mount Directory 
      -> mkdir nginx_BindMount
      -> cd nginx_BindMount
      -> pwd

    2- Run a container using nginx image
      -> docker run -d --name nginx_BindMount -v /root/nginx_bindMount:/user/share/nginx/html nginx
      -> docker exec -it nginx_BindMount bash

    3- Echo any contentto show when curl ip-address
      -> cd /user/share/nginx/html
      -> echo "Welcome from bind mount "
      -> exit
      -> docker inspect -f '{{.NetworkSetting.IPAddress}}' nginx_BindMount   
      -> 172.17.0.2
      -> curl 172.17.0.2
```

## Part2:
### 1.Create 2 docker network (net-1 & net-2)
```bash
   -> docker network create network_1
   -> docker network create network_2
```
### 2. Run 2 new containers using nginx:alpine image, and attach the net-1 to them

```bash
    -> docker run -d --name nginx_net1 --net network_1 nginx:alpine
    -> docker run -d --name nginx_net2 --net network_1 nginx:alpine

```
### 3. Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
     -> docker run -d --name nginx_net3 --net network_2 nginx:alpine
```
### 4. Inspect the 3 containers to know their IPs and write them aside.
```bash
     -> docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net1
     -> 172.18.0.2

     -> docker inspect -f '{{.NetworkSettings.Networks.network_1.IPAddress}}' nginx_net2
     -> 172.18.0.3

     -> docker inspect -f '{{.NetworkSettings.Networks.network_2.IPAddress}}' nginx_net3
     -> 172.20.0.2
```
### 5. Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
   -> docker exec -it nginx_net1 bash
   -> Use ping 
   -> ping 172.20.0.2
The notice:
The ping fails as the two containers are in different networks resulting in no response in the terminal.
```
###  6. Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)

```bash
 -> docker exec -it nginx_net1 bash 
 -> curl 172.18.0.2
 -> ping 172.18.0.3
The notice:
The Ping succeeds and returns a response because the two containers are in the same network.
```

## Part3:
### 1. Explain the difference between Docker volumes and Bind MountActivate Windows Go to Settings to activate Windo

```bash
Docker volume:

=> Docker volume is the recommended method for storing data created and utilized by Docker containers is to use volumes.
=> Docker volumes may be interacted with using CLIs and APIs.
=> All you need is the volume name to mount it.
=> In /var/lib/docker/volumes, the volumes are stored.

Bind volume:

=> Bind mount has existed from Docker’s early versions. Comparatively speaking, bind mounts are less useful than volumes.
=> Bind mounts cannot be accessed by CLI commands. You may still work instantly with them on the host system.
=> When using bind mounts for mounting, a route to the host computer must be supplied.
=> On the host computer, a bind mount can be located anywhere.
    
```
