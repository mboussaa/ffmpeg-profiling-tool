# Performance profiling tool
## Goal
In this project, we are going to use cAdvisor to monitor Docker containers. Furthermore, we will push the data into InfluxDB and finally display it through Grafana.

- cAdvisor: Google Monitoring Container: provides container users an understanding of the resource usage and performance characteristics of their running containers.
   
- InfluxDB: Time series database. It's useful for recording metrics, events, and performing analytics.

- Grafana: Realtime designing tool: we will use it to visualize the recorded data. Grafana is easily configurable and will let you choose what to render.
   
In order to monitor docker containers, we are going to wrap different compiled versions of ffmpeg library within docker containers and then, stress that library to increase the load in term of memory and cpu usage.

This stress tool generates command lines that target FFMpeg Library. It uses the novelty search to generate efficiently test data passed as parameters to the command lines.

More details can be find [here]
[here]:https://github.com/mboussaa/script-generator-for-ffmpeg

We are going to compile ffmpeg library with at least, two different optimization options (e.g. -O1, 02...) each one will be wrapped in a docker container. Then, we will monitor both versions in term of non functional properties.

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

###Generating stress containers :
 
Now, we have to create a stress container for ffmpeg in order to

(1) gather data from cAdvisor

(2) save them in InfluxDB

(3) show statistical analyzes through Grafana.

To do so, we are going to create two other containers (you can create more) from ffmpeg with two different compilation optimizations. Then, we are going to execute a huge number of ffmpeg command lines on that containers in order to increase the load.

Go through O0 and O1 repositories and type the following commands in order to create docker images from docker files:

    In O1:
    docker build -t versiono0 .
    In O2:
    docker build -t versiono1 .

Now we have to start the two containers from both images (versiono0 et versiono1).

Before running the following commands, copy the content of tmp folder into your root tmp folder. This folder contains the media files and the script to run.

    docker run -i --name=containero0 -v /tmp:/tmp -t versiono0 /bin/bash -c "chmod +x /tmp/ffmpegScript.sh && /tmp/ffmpegScript.sh"

    docker run -i --name=containero1 -v /tmp:/tmp -t versiono1 /bin/bash -c "chmod +x /tmp/ffmpegScript.sh && /tmp/ffmpegScript.sh"

NB 1: You can create more than two images using other docker files within O2, 03, Ofast repositories.

NB 2: Each container should be run in a different terminal

    docker ps # to see the running containers, they should be 5

###Monitoring and test :

Finally, once containers start running, we can monitor and measure some non-functional metrics.

First, we have to be sure that cAdvisor is dumping data to the influx data base. Go to :

    http://10.0.2.15:8083

and execute a sample query to explore data. For example:

    select container_name, derivate(cpu_cumulative_usage) from stats where docker_name="containero0" and time>now()-15m group by time(2s)

In fact, this query expresses the CPU utilized per container "containero0" within last 15 minutes. Results are grouped by time(2s).

CPU usage is a cumulative metric so it is an ever-increasing integer. The mean of it won't be very interesting. We utilized a derivative value that allows us to show the difference between data points and thus see how much CPU was used in that time period.

cAdvisor also outputs in number of cores. The result will be in billionths of a core so if we divide by 1,000,000,000, we get the result in whole cores. To get the percentage of CPU Usage, we will divide over the number of cores on the machine (your machine). For example:

    select container_name, ((derivate(cpu_cumulative_usage)/1000000000)/4) from stats where docker_name="containero0" and time>now()-15m group by time(2s)

This will be the percentage used over that 2s interval.

You can find more details about measuring CPU usage in cadvisor github: [issue #679]
[issue #679]:https://github.com/google/cadvisor/issues/679

However, the memory consumption is an instantaneous metric. It's value will be useful without a derivative but a mean value within an interval of 2 sec would be significant. You can use the following example to design the memory consumption:

    select container_name, mean(memory_usage) from stats where container_name="containero0" and time>now()-15m group by time(2s)

Some stats and data must be displayed on the screen.

To test Grafana and set some metrics, go to :
 
    http://10.0.2.15:80

Start a new graph and edit it. Navigate to the metric tab and try to set the same query used above. You have to see then, a graph showing the cpu cumulative usage within latest 15min.

You can play with it now !! Try to compare the cpu cumulative usage between the two versions of ffmpeg...

   










