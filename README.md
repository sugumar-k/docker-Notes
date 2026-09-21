# docker-Notes:

2. DOCKER COMMANDS
	docker +double space =to know the list of commands used in docker.
	docker double tab will show list of commands 
	docker commnad --help will show what operations we can perform using that command.
	we can give docker command operation or docker-command-operation	
	Ex:docker system df:(or) docker-system-df
#docker -v:
	to know the version of the docker.
#docker system df:
	will show the disk usage.
#docker system prune:
	used to remove unused containers and images.
#docker stats:
	used to get real time system usages.
#docker search imageName:
    is used to search public/private images from dockerhub or docker registry
#docker images:
    will list the show the list of images inside the host
#docker pull:
    used to clone the image to host (Ex:docker pull imageName)
#docker create & docker run: 
	both commands are used to create container from an image but run command will start the container create command will not do that.
	Ex:docker run --name mysqldb mysql:latest
	docker run --name containerName imageName:
	Note it will start the container on the same shell so you cant perform any actions on container 
#docker run --name containerName -d imageName:
	use to start the container on background.
#docker ps :
	will show the status of running  containers.
#docker ps -a :
	will show the stopped containers as well
#docker stop:
	used to stop running container.Ex(docker stop containerName)
#docker start:
	used to start the container.Ex(docker start containerName)
#docker rm:
	used to remove the container Ex (docker rm containerName).
#docker rmi:
	used to remove images. Ex(docker rmi imageName).
#docker exec:
	to interact/perform tasks with the containers we have to log in to the containers shell.
	#docker exec -it containerName /bin/sh (or) /bin/bash
	you can perform tasks without login to interactive terminal
	Ex: docker exec containerName task (ocker exec containerName uname-a)
#docker cp:
	docker copy is used to transfer files between docker host and docker container works similar to scp
	docker cp fileName containerName:url docker cp test.txt container1:/tmp (from host to container)
	docker cp containerName:url/fileName /hostdirectoryUrl/ docker cp container1:/tmp/test.txt /tmp
#docker logs:
	used to get logs for the container Ex:(docker logs containerName)
#docker login:
    used to login to public or private registry account.(Ex:docker login -u userName)

	
# 3.DOCKER IMAGES #
    docker image is readonly,blue print for a container.
    it contains multiple layers,each argument or task will created as a layer
#docker search:
    docker search is used o search images from a public or private registry (Ex:docker search mysql).
#docker images:
    this command will list the images present in the docker host(local).
#docker pull:
    this command is used to pull images from public/private registry. (Ex:docker pull mysqld).
#docker inspect:
    this command is used to know full details about the docker image (Ex: docker insect mysql).
#docker history:
    this command is used to get the history of changes made in the image. (Ex: docker history mysql)
#docker save:
    this command is used to save image backup into a tar (Ex:docker save mysql >mysql.tar).
#docker load:
    this command is used to restore image from a backup tar (Ex:docker load -i mysql.tar).
#docker rmi:
    used to remove image from the docker host(local) (Ex:docker rmi mysql).
#docker commit:
    used to create image from a container.(Ex:docker commit new-mysql mysql).
    we can make changes inside the container by using interactive terminal and create a image ,
    when we create a container using that image we can see the changes made in the container reflectedin new continer.
#docker push:
    used to push local image to a public or private registry (Ex:docker push sugumar/custom-mysql).
#docker tag:
    used to rename the docker image.
    
    
#4. Docker Image Creation Using Docker File #


    
#5. Docker Networks:#

#Network Type:
	1.bridge network(default networks an i will be assigned to container can access from docker host and outside also).
	2.none network (--network==none :used only on testing env,cant access from outside and through network.)
	3.host network (only accessed from host server, cant access from outside. --network=host )
	using bridge network access over the network we have to use port forwarding else we can only access from docker host.
	
#6 docker volumes:
	three types:bind,general Volume,tmpfs.
	1.bind_Volume or file_system_Volume:
		data is saved in a normal file system of the docker host.
		Ex: docker run -d --name container_Name image_Name -v src:dest
		src:dest: 
		src= docker host file system Directory Path
		dest:container directory path.
	2.general volume:
		data is save is docker area.
		first we have to create volume Ex(docker volume create appVolumenName).
		then assign it to source 
		Ex:docker run -d --name container_name image_name --mount source=appVloumeName, destination=directoryPathInside Container.
		Or
		Ex docker run -d --name container_Name -v appVolumeName:/var/lib/mysql imageName.
	  (in both bind Volume and general volume data gets saved in docker host even after removing the container.same data can be shared by multiple containers.)
	3.tmpfs:
  default volume data gets removed once container stopped.
  data is stored in docker host ram.
  data is temporary.
	
    
    














