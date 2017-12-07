# BigObject Edge Database Demonstration for Sliding Table and Hot Data Fast Access

[BigObject](http://www.bigobject.io) (BO) is a very light and efficient database with small footprint. 
You can even run it on [Raspberry Pi](https://www.raspberrypi.org/) or [PINE64](https://www.pine64.org/). 
The demonstration shows a scenario that you can use BigObject + [Node-RED](https://nodered.org/) + [Mosquitto](https://mosquitto.org/) to make a naive alerting system with a regular cell phone and a pine64. 
This demonstration utilizes two unique features: 

1. **Sliding Table**: A sliding table is a table that keeps only the records inserted in the most recent time period T. Any records stay in the table longer than T is considered expired and removed immediately from the table. A sliding table is a long-lasting and maintenance-free table that operates very efficiently and effectively in terms of memory and disk space. People do not have to worry about deleting old data. Data kept in a sliding table is recent and so-called hot data.
2. **Hot Data Fast Access**:  Users can access hot data by specifying a recent period for efficiency. BigObject is optimized for performing it efficiently with using timestamp.

https://youtu.be/D0ZQSypXZcc

## Prerequisite
### PINE64 
1. A pine64 board with 
	1. OS: [Xenial Minimal](http://wiki.pine64.org/index.php/Pine_A64_Software_Release#Xenial_Minimal_Image)
	1. git
	1. curl
	1. Docker >= 1.10.0
	1. Docker Compose >= 1.6.0 
		1. You can install these by executing ``` sudo apt install git curl docker.io docker-compose ```
1. An android phone with the app, [Sample MQTT Publisher](https://play.google.com/store/apps/details?id=com.hoop.accelerometer) 
### Raspberry Pi 
1. A Raspberry Pi board with
	1. OS: [RASPBIAN STRETCH LITE](https://www.raspberrypi.org/downloads/raspbian/)
	1. git
	1. curl
	1. Docker >= 1.10.0
		1. Raspbian only provides docker 1.8, which doesn't support docker-compose 2.0. Please install docker-ce from Docker's apt repository. [Reference](https://docs.docker.com/engine/installation/linux/docker-ce/debian/)
	1. Docker Compose >= 1.6.0 
		1. You can install these by executing ``` sudo apt install git curl docker-compose ```
1. An android phone with the app, [Sample MQTT Publisher](https://play.google.com/store/apps/details?id=com.hoop.accelerometer) 

## How to setup this demo
1. Get the source package from github to the pine64
	``` 
	git clone https://github.com/bigobject-inc/edge-database-demo.git
	```
1. Get the ip address, *pin64_ip*, of the pine64 board. e.g. 192.168.1.123 
1. Run this command on the pine64. It may take a while because of downloading docker images.  
	1. PINE64
	``` 
	sudo mkdir -p -m777 /srv/bigobject/node-red-data; sudo docker-compose -f edge-database-demo/compose/docker-compose.yml up -d 
	```
	2. Raspberry Pi
	``` 
	sudo mkdir -p -m777 /srv/bigobject/node-red-data; sudo docker-compose -f edge-database-demo/compose/docker-compose-armhf.yml up -d
	```

1. Import the flow, edge-demo-flow.json, via the following command or use the Node-RED's GUI
	``` 
	curl -v -d "@edge-database-demo/edge-demo-flow.json" --header "Content-type: application/json" http://localhost:1880/flows 
	```
1. Open the app in the cellphone and set the hostname as *pine64_ip* and port as 1883
1. Press **Connect**, and keep the cellphone awake. 
1. Open the dashboard, http://*pine64_ip*:1880/ui/, in your browser. 
1. Rotate your cellphone and you will see the changes in the dashboard
1. Have fun!

![screenshot1](https://raw.githubusercontent.com/bigobject-inc/edge-database-demo/master/images/screenshot1.png)

## How to stop this demo 
Run this command 
1. PINE64
	``` 
	sudo docker-compose -f edge-database-demo/compose/docker-compose.yml stop
	```
2. Raspberry Pi
	``` 
	sudo docker-compose -f edge-database-demo/compose/docker-compose-armhf.yml stop
	```

## How to remove this demo
1. Use docker-compose to purge running containers
	1. PINE64
	``` 
	sudo docker-compose -f edge-database-demo/compose/docker-compose.yml down -v
	```
	2. Raspberry Pi
	``` 
	sudo docker-compose -f edge-database-demo/compose/docker-compose-armhf.yml down -v
	```

1. If you want to remove bo & node-red data, run
	``` 
	sudo rm -rf /srv/bigobject/iot-data /srv/bigobject/lua /srv/bigobject/node-red-data 
	```

1. If you want to remove generated and downloaded docker images, run 
	1. PINE64
	``` 
	sudo docker rmi compose_mqtt_broker:latest bigobject/node-red:arm64 bigobject/bigobject-edge:arm64 buildpack-deps:jessie-curl
	```
	2. Raspberry Pi
	``` 
	sudo docker rmi compose_mqtt_broker:latest bigobject/node-red:armhf bigobject/bigobject-edge:armhf buildpack-deps:jessie-curl
	```
## FAQ
### What is Z-score?
*In statistics, the Z-score is the signed number of standard deviations by which the value of an observation or data point is above the mean value of what is being observed or measured.* - [wikipedia](https://en.wikipedia.org/wiki/Standard_score)

In this demonstration, the Z-score is defined as ![Z-score](https://raw.githubusercontent.com/bigobject-inc/edge-database-demo/master/images/z-score.png). 

The text showed Y-axis acceleration and Z-score will also turn red when the z-score is larger than 1. E.g. 
![screenshot2](https://raw.githubusercontent.com/bigobject-inc/edge-database-demo/master/images/screenshot2.png)

### How to install docker-ce on Raspbian Stretch Lite
The following steps are tested on a fresh installed Raspbian Stretch Lite. It's an excerpt from [Docker's official site](https://docs.docker.com/engine/installation/linux/docker-ce/debian/). It's highly recommended read the official site first before using these. 
```
sudo apt-get update
sudo apt-get install -y \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
echo "deb [arch=armhf] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
sudo apt-get update
sudo apt-get install -y docker-ce
```

