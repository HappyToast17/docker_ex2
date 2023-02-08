# docker_ex2
Sep22 course assignment

# build an image from dockerfile in rep
docker build .
# create volume
docker volume create --name volume_demo
# run container with map to /data
docker run -it -v volume_demo:/data <image_id>
# enter container and create files
docker exec -it <container_name> /bin/bash
touch file1
touch file2
#exit the container and see if you can locate on host
# run container without privilege to create dir “/datatest” inside the container
docker run --read-only -it <image_id> /bin/bash
# kill all containers
docker stop $(docker ps -aq) && docker rm $(docker ps -aq)
# create docker network “network_demo”
docker network create network_demo
# create container again with  “network_demo”
docker run -it --name container1 -p 8080:80 --network network_demo <image_id>
# create “network_demo” dir and copy Dockerfile inside that dir and change directory to “network_demo” 
mkdir network_demo
cp Dockerfile network_demo/
cd network_demo/
# now run container with “network_demo” that will listen on nginx on port 8000 - do it with volume to the configuration
docker run -it --name container2 -p 8000:80 --network network_demo -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf <image_id>
# make sure ping utility is installed on each container!
apt-get update && apt-get install -y iputils-ping
# check ping connection by container name (docker dns resolver) between the containers
docker exec container1 ping container2
# kill all containers
docker stop $(docker ps -aq) && docker rm $(docker ps -aq)
# remove images
docker rmi  <imgage_id>
