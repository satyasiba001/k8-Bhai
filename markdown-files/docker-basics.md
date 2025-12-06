docker pull/run redis:4.0
docker run -p 80:5000 kodecloud/sample-app:4.0
docker inspect <container-name/id>
docker logs <container-name/id>



./app.sh  ==> It takes an input and show "Hello and welcome sibu"
If you want to dockerise the application it will not show you an interactive shell, for that to happen you have to use the i flag.

docker run -i kodecloud/sample-prompt-docker ==> It will take you to an interactive mode.
sibu
Hello and welcome sibu

docker run ubuntu sleep 100
docker attach <container-id>
docker run timer


### Notes:

1. Remember, the underlying host where Docker is installed is called Docker host or Docker engine. When we run a containerized web application, it runs and we are able to see that the server is running.But how does a user access my application? As you can see, my application is listening on port 5,000. I could access my application by using port 5,000. But what IP do I use to access it from a web browser? There are two options available. One is to use the IP of the docker container. Every docker container gets an IP assigned by default.In this case, it is 172.17.0.2. But remember, that this is an internal IP and is only accessible within the Docker host. If you open a browser from within the Docker host, you can go to http://172.17.0.1:5000 to access the IP address. But since this is an internal IP, users outside of the docker host cannot access it using this IP. For this, we could use the IP of the docker host, which is 192.168.15.1 But for that to work, you must have mapped the port inside the Docker container to a free port on the Docker host. For example, if I want the users to access my application through Port 80 on my Docker host, I could map Port 80 of local host to port 5,000 on the Docker container, using the -P parameter in my run command like this. The user can access my application by going to the URL http://192.168.1.5:80/. All traffic on port 80 on my Docker host will get routed to port 5,000 inside the Docker container. This way, you can run multiple instances of your application and map them to different ports on the Docker host or run instances of different applications on different ports. For example, in this case, I'm running an instance of MYSQL that runs a database on my host and listens on the default MYSQL port, which happens to be 3306 or another instance of MYSQL on another port 8306. You can run as many applications like this and map them to as many ports as you want. Of course, you cannot map to the same port on the Docker host more than once. We will discuss more about port mapping and networking of containers in the network lecture later on.

2. Let's now look at how data is persisted in a Docker container. For example, let's say you were to run a MYSQL container. When databases and tables are created, the data files are stored in location MYSQL inside the Docker container. Remember, the docker container has its own isolated file system and any changes to any files happen within the container. Let's assume you dump a lot of data into the database. What happens if you were to delete the MYSQL container and remove it? As soon as you do that, the container along with all the data inside it gets blown away. Meaning all your data is gone. If you would like to persist data, you would want to map a directory outside the container on the Docker host to a directory inside the container. In this case, I create a directory called /opt/datadir and map that to /var/lib/mysql inside the Docker container, using the -v option and specifying the directory on the Docker host, followed by a colon and the directory inside the Docker container. This way, when Docker container runs, it will implicitly mount the external directory to a folder inside the Docker container. This way, all your data will now be stored in the external volume at /opt/data directory, and thus will remain even if you delete the Docker container. 

     ```docker run -v /opt/datadir:/var/lib/mysql mysql```
