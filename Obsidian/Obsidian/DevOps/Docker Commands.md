
### Docker pull ``nginxdemos/hello:0.3`
pull a docker image from the docker store (specified version 0.3)

____

### docker run `nginxdemos/hello`
Run an image to start a container

### docker run -d `nginxdemos/hello`
Run an image to start a container <span style="color:rgb(107, 255, 174)">in detached mode</span>

### docker run -d --name peterscontainer nginxdemos/hello
Run an image to start a container <span style="color:rgb(107, 255, 174)">in detached mode</span> <span style="color:rgb(255, 0, 0)">with custom name</span> 

### docker run -d -p 5001:80 nginxdemos/hello
Run the image at <span style="color:rgb(255, 0, 0)">port 5001</span>

____

### docker ps
list running containers

### docker ps -a
list <span style="color:rgb(255, 0, 0)">all</span> containers

____

### docker stop 574949

____

### docker logs 4234
<span style="color:rgb(255, 0, 0)">Show</span> the logs of the container with name starting with 4234

### docker logs 4234 -f
<span style="color:rgb(255, 0, 0)">Follow</span> the logs of the container with name starting with 4234

____

### docker rm 2042
Remove the container

### docker rm 2042 -f
Remove the container, <span style="color:rgb(255, 0, 0)">even if it's working</span> 

____

### docker rmi nginxdemos/hello:0.3
Remove image

____

### docker images
Show all docker images  

____

### docker exec -it c52 /bin/sh

___

### docker run -it -p 8080:8080 -v %cd%:/app -w /app node:22.0.0 npm run serve

____

composerize -> docker run commands to docker compose file