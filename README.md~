# Performance profiling tool
## Goal
In this project, we are going to use cAdvisor to monitor Docker containers. Furthermore, we will push data into InfluxDB and finally display it through Grafana.

- cAdvisor: Google Monitoring Container: provides container users an understanding of the resource usage and performance characteristics of their running containers.
   
- InfluxDB: Time series database. It's useful for recording metrics, events, and performing analytics.

- Grafana: Realtime designing tool: we will use it to visualize the recorded data. Grafana is easily configurable and will let you choose what to render.
   
In order to monitor docker containers, we are going to wrap different compiled versions of ffmpeg library within docker containers and then, stress that library to increase the load in term of memory and cpu usage.

This stress tool generates command lines that target FFMpeg Library. It uses the novelty search algorithm to generate efficiently test data passed as parameters to command lines.

More details can be found [here]
[here]:https://github.com/mboussaa/script-generator-for-ffmpeg

We are going to compile ffmpeg library with at least, two different optimization options (e.g. -O1, 02...) Each version will be wrapped into a docker container. Then, we will monitor both versions in term of non functional properties.

##Installation :
###InfluxDB Installation :
Start InfluxDB inside a new container.

    docker run  
    -d
    --name=influxdb 
    -p 8083:8083  
    -p 8086:8086  
    --expose 8090  
    --expose 8099  
    tutum/influxdb

For our case, the HOST_IP is 10.0.2.15. We will continue to use this IP in the rest of this tutorial but you should replace it with your local HOST_IP (use ifconfig to catch your IP)

So then, navigate now to InfluxDB administration page at:

    http://10.0.2.15:8083

Login with root/root and create a new database called "cadvisorDB".

###cAdvisor Installation :
Now start cAdvisor with the extra parameters storage_driver, storage_driver_host and storage_driver_db.

    docker run     
    --volume=/:/rootfs:ro     
    --volume=/var/run:/var/run:rw     
    --volume=/sys:/sys:ro     
    --volume=/var/lib/docker/:/var/lib/docker:ro     
    --publish=8080:8080     
    --detach=true     
    --name=cadvisor     
    google/cadvisor:latest  
    --logtostderr   
    -storage_driver=influxdb    
    -storage_driver_host=10.0.2.15:8086
    -storage_driver_db=cadvisorDB 
   
You should specify your host ip for the storage_driver_host. As well, you have to put the name of the data base you have created "cadvisorDB".

If you wan to access to cAdvisor Web UI, please navigate to :

    http://10.0.2.15:8080 #(Assuming that 10.0.2.15 is your host ip)

###Grafana Installation :
Start another container running Grafana.

    docker run
    -d
    -p 80:80 
    --name=grafana  
    -e HTTP_USER=root   
    -e HTTP_PASS=root    
    -e INFLUXDB_HOST=10.0.2.15    
    -e INFLUXDB_PORT=8086 
    -e INFLUXDB_NAME=cadvisorDB
    -e INFLUXDB_USER=root
    -e INFLUXDB_PASS=root     
    tutum/grafana

In this step, you have to specify the DB name you have already created as well as, your host ip.
   
You can access to Grafana Web UI through :
   
    http://10.0.2.15:80

So far, we have created 3 containers :
  - cAdvisor container
  - InfluxDB container
  - Grafana Container

Execute "docker ps" to see running containers.

###Generate test data using Novelty Search :
To generate test data using Novelty Search within a docker container: create a new image from a dockerfile. To do so, go to novelty_generator folder and execute :

	docker build -t novelty_generator .

Within this image, we will clone the maven project of novelty generator and execute it.

Now, we have to create a new container from this image. We will copy media files and generated script from the maven project to the host machine (/tmp). Therefore, the stress containers can use these files to stress ffmpeg library (data will be always shared using -v Mount Volume)

	docker run -i -v /tmp:/tmp  --name=container_novelty -t novelty_generator /bin/bash -c "cp -r /script-generator-for-ffmpeg/ffmpeg/* /tmp/"

So now, our /tmp host folder is containing all our media files and test data produced by our novelty generator.


###Generating stress containers :

Now, we have to create a stress container for ffmpeg in order to

(1) gather data from cAdvisor

(2) save them in InfluxDB

(3) show statistical analyzes through Grafana.

To do so, we are going to create two other containers (you can create more) from ffmpeg with two different compilation optimizations. Then, we are going to execute a huge number of ffmpeg command lines on these containers in order to increase the load.

Go through O0 and O1 repositories and type the following commands in order to create docker images from docker files:

    In O0:
    docker build -t version_o0 .
    In O1:
    docker build -t version_o1 .

Now we have to start the two containers from both images (versiono0 et versiono1).

Before running the following commands, copy the content of tmp folder into your root tmp folder. This folder contains the media files and the script to run.

	docker run -i 
	--name=container_o0 
	-v /tmp:/tmp 
	-t version_o0 
	/bin/bash 
	-c "chmod +x /tmp/ffmpegScript.sh && /tmp/ffmpegScript.sh"

	docker run -i 
	--name=container_o1 
	-v /tmp:/tmp 
	-t version_o1 
	/bin/bash 
	-c "chmod +x /tmp/ffmpegScript.sh && /tmp/ffmpegScript.sh"

NB 1: You can create more than two images using other docker files within O2, O3, Ofast folders.

NB 2: Each container should be run in a different terminal

    docker ps # to see the running containers, they should be 5

###Monitoring and profiling :
####From InfluxDB :
Finally, once containers start running, we can monitor and measure some non-functional metrics.

First, we have to be sure that cAdvisor is dumping data to the influx data base. Go to :

    http://10.0.2.15:8083

and execute a sample query to explore data. For example:

	select container_name, derivative(cpu_cumulative_usage)  
	from stats  
	where container_name='containero0' and time>now()-15m  
	group by time(2s)

In fact, this query expresses the CPU utilized per container "containero0" within last 15 minutes. Results are grouped by time(2s).

CPU usage is a cumulative metric so it is an ever-increasing integer. The mean of it won't be very interesting. We utilized a derivative value that allows us to show the difference between data points and thus see how much CPU was used in that time period.

cAdvisor also outputs in number of cores. The result will be in billionths of a core so if we divide by 1,000,000,000, we get the result in whole cores. To get the percentage of CPU Usage, we will divide over the number of cores on the machine (your machine). For example:

	select container_name, (derivative(cpu_cumulative_usage)/1000000000)/4
	from stats 
	where container_name='containero0' and time>now()-15m 
	group by time(2s)

This will be the percentage used over that 2s interval.

You can find more details about measuring CPU usage in cadvisor github: [issue #679]
[issue #679]:https://github.com/google/cadvisor/issues/679

However, the memory consumption is an instantaneous metric. It's value will be useful without a derivative but a mean value within an interval of 2 sec would be significant. You can use the following example to calculate the memory consumption:

	select container_name, mean(memory_usage) 
	from stats 
	where container_name='containero0' and time>now()-15m 
	group by time(2s)

Some stats and data must be displayed on the screen.
####From Grafana :
To test Grafana and set some metrics, go to :
 
    http://10.0.2.15:80

Start a new graph and edit it. Navigate to the metric tab and try to set the same query used above. You have to see then, a graph showing the cpu cumulative usage within latest 15min.

You can play with it now !! Try to compare the cpu cumulative usage between the two versions of ffmpeg...

####From Shipyard :
Shipyard is a management tool for Docker servers. It allows you to see which containers each of your servers are running, in order to start or stop existing containers or create new ones.

Shipyard is useful to deploy many applications/containers on your Docker host using the GUI.

Once you've set up Shipyard on your server you can access it using a graphic interface, a command-line interface, or an API. 

Once you have Docker running, it is quite easy to install Shipyard because it ships as Docker images. All you need to do is pull the images from the Docker registry and run the necessary containers. First we will create a data volume container to hold Shipyard's database data. This container won't do anything by itself; it is a convenient label for the location of all of Shipyard's data.
   
	docker create --name shipyard-rethinkdb-data shipyard/rethinkdb


Now that the data volume container is created, we can launch the database server for Shipyard and link them together.

	docker run -it -d --name shipyard-rethinkdb --restart=always --volumes-from shipyard-rethinkdb-data -p 127.0.0.1:49153:8080 -p 127.0.0.1:49154:28015 -p 127.0.0.1:29015:29015 shipyard/rethinkdb

This launches a container running RethinkDB, a distributed database, and makes sure it can only be accessed locally on the server itself. If you try to visit http://10.0.2.15:49153 in your browser, you shouldn't see anything.

Now that Shipyard's database is up, we can run Shipyard itself by launching another container and linking it to the database.

	docker run -it -p 8080:8080 -d --restart=always --name shipyard --link shipyard-rethinkdb:rethinkdb shipyard/shipyard

We can now access our running Shipyard instance using port 8080.

Next, we'll take a look at Shipyard's graphic interface. To access it, open http://10.0.2.15:8080 in your browser. This should show you the login screen. Use the username admin and the new password shipyard.

Once you're logged in, Shipyard will display the Engines tab and warn you that there are no engines in your Shipyard cluster yet. An engine is a Docker host capable of running containers. Here we will add each Docker server that you want to manage with Shipyard.

Add at the end of /etc/default/docker the following command to configure Docker to listen to requests port (4243 for example).

	DOCKER_OPTS="-H tcp://your_server_ip:4243 -H unix:///var/run/docker.sock"

and restart docker: 

	sudo service docker restart

Now that your Docker host is properly configured, we can add it to Shipyard as an engine. Access your Shipyard GUI and go to the Engines tab. Click on the + Add button.

Add a Name for your docker host, a label, a CPU and memory limit and the following address :
	
	http://10.0.2.15:4243

Shipyard will now connect to your Docker host, verify the connection, and add it as an engine.






