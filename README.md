# Assignment: Geoserver Intro
## Worth: 40 points
## Due: 7 Days

## Objective: Run geoserver in a docker container with sample data

In this assignment you will setup geoserver to run in a docker container on your local machine and configure the docker container to use your local disk as a volume to serve the geoserver sample data. 

## Background
GeoServer is a Java-based software server that allows users to view and edit geospatial data. (About GeoServer)[http://geoserver.org/about/] Using open standards set forth by the Open Geospatial Consortium (OGC), GeoServer allows for great flexibility in map creation and data sharing. GeoServer is written in Java and is provided freely and with its source code uner the GNU General Public License v2.0.

In a typical scenario, GeoServer would be run as a server on dedicated server hardware, a VM, or a container in a container management service. For the purposes of this class, you will run GeoServer in a docker container on your workstation. This has a number of benefits:
1) It is a clean copy of GeoServer for your exclusive use
2) As a container, the environment will be consistent every time
3) No need to provision dedicated server hardware to run idle when not in use
4) Gain familiarity with docker

## Prerequisites
1) Docker is installed

## Assignment

### 1) Pull the geoserver docker container.

There are a number of geoserver docker containers which have been created and published on docker hub. The one will be using is `kartoza/geoserver:2.15.2`. Visit https://hub.docker.com and search for `geoserver`. The container you will use should be one of the most popular. Take a minute to look at the number of Downloads, Stars, and the date it was last updated. As of September, 2019, `kartoza/geoserver` had 76 Stars compared to the 20 stars for the next two. Explore this docker container by clicking on it in the list of other geoserver containers. It will take you to https://hub.docker.com/r/kartoza/geoserver where you can read instructions on usage, look at its Tags, see the source Dockerfile, and other details about its build history.

Pull the image as in the instructions to download the latest version:

```
docker pull kartoza/geoserver:2.18.0
```

Run the docker container to give yourself a quick test that you have a working container:

```
docker run -p 8080:8080 kartoza/geoserver:2.18.0
```

This will run geoserver in a container and expose the port `8080` from the docker container as `8080` on your host system (i.e., your Desktop or laptop). It will take a minute for geoserver to start up. Watch the terminal output for a message indicating that Geoserver is running; it will look like this: 
```
10-Sep-2019 05:38:34.042 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in 37964 ms
```
. Now open a browser and visit http://localhost:8080/geoserver. If you see something other than the GeoServer Welcome page, then something went wrong.

### 2) Configure geoserver to store data on the host rather than the container.

We want to use a local directory to serve sample data through geoserver but in order to do that, we need to do a little housecleaning and prep first.

### 3) Kill the running container from step 1). 
To kill the running container, look at the running docker processes:
```
docker ps
```
Copy the CONTAINER ID for the geoserver IMAGE and use it to `rm` the container:
```
docker rm -f CONTAINER_ID
```
Use `docker ps` to see its status and make sure it is not running.

### 4) Acquire the sample data. 
Sample data is included in the source repository for geoserver, which is at https://github.com/geoserver/geoserver. Clone this repository to get a copy of the geoserver source code and the sample data. 

Create a directory to use as the geoserver data directory. This will contain data and configuration for geoserver and should persist between runs of the container. If you use a shared network path then it will run the same from another machine with the same drive mapping (e.g., `G:\geoserver-data`). 

In the cloned repo, find the directory `/data/release`. Copy the contents of the directory to your geoserver data directory you created in the previous step.

### 5) Run kartoza/geoserver with a local volume
Finally, run the docker container with the volume switch mapping your local directory to the geoserver data directory on the container. The important switch here is `-v $HOME/geoserver_data:/opt/geoserver/data_dir` which maps the directory, `$HOME/geoserver_data` on your local computer to the `/opt/geoserver/data_dir` in the container, which is where geoserver looks to find its configuration and data files. Your path will likely be different. If you wanted to use a shared network drive named `G:\geoserver_data`, for example, it might look like: `-v G:/geoserver_data:/opt/geoserver/data_dir`

This is the command I would use on a mac:

```
docker run -v $HOME/geoserver-data:/opt/geoserver/data_dir -p 8080:8080 kartoza/geoserver:2.15.2
```

### 6) Test it out
Visit http://localhost:8080/geoserver and make sure it is running. Login as admin:
- default username: `admin`
- default password: `geoserver`

Verify that on the Welcome screen you see that there are `19 Layers` in the Main content area. 

If everything is working, clean up by deleting the local geoserver cloned repo.

_Deliverable: Take a screenshot of your GeoServer Welcome screen showing the 19 Layers. Name it `screenshot.png` and add it to a new GitHub branch named `solution`. 

### 7. Turn in your work via GitHub Pull Request. 

Submit a *Pull request* to merge your `solution` branch with the `master` branch. _Do not merge it yourself_

