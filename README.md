# BigObject Edge Database Demonstration

[BigObject](http://www.bigobject.io) (BO) is a very light and efficient database with small footprint. 
You can even run it on [PINE64](https://www.pine64.org/). 
The demonstration shows a scenario that you can use BigObject + [Node-RED](https://nodered.org/) + [Mosquitto](https://mosquitto.org/) to make a naive alerting system with a regular cell phone and a pine64. 

## Prerequisite
1. A pine64 board. 
	1. OS: [Xenial Minimal](http://wiki.pine64.org/index.php/Pine_A64_Software_Release#Xenial_Minimal_Image)
	1. git
	1. curl
	1. Docker >= 1.10.0
	1. Docker Compose >= 1.6.0 
		1. You can install these by executing ``` sudo apt install docker.io docker-compose ```
1. An android phone with the app, [Sample MQTT Publisher](https://play.google.com/store/apps/details?id=com.hoop.accelerometer) 

## How to setup this demo
1. Get the source package from github to the pine64

	``` git clone https://github.com/bigobject-inc/bo-edge-demo```

1. Get the ip address, *pin64_ip*, of the pine64 board. e.g. 192.168.1.123 
1. Run this command on the pine64. It may take a while because of downloading docker images.  

	``` sudo mkdir -p -m777 /srv/bigobject/node-red-data; pushd bo-edge-demo/compose; sudo docker-compose up -d; popd ```

1. Import the flow, edge-demo-flow.json, via the following command or use the Node-RED's GUI

	``` curl -X POST -v -d "@bo-edge-demo/edge-demo-flow.json" --header "Content-type: application/json" http://localhost:1880/flows ```

1. Open the app in the cellphone and set the hostname as *pine64_ip* and port as 1883
1. Press **Connect**, and keep the cellphone awake. 
1. Open the dashboard, http://*pine64_ip*:1880/ui/, in your browser. 
1. Move your cellphone and you will see the changes in the dashboard
1. Have fun!

## How to remove this demo
1. Use docker-compose to purge running containers
	
	``` pushd bo-edge-demo/compose; sudo docker-compose down -v; popd ```

1. If you want to remove bo & node-red data, run

	``` sudo rm -rf /srv/bigobject/iot-data /srv/bigobject/lua /srv/bigobject/node-red-data ```

1. If you want to remove generated and downloaded docker images, run 

	``` sudo docker rmi compose_mqtt_broker:latest compose_node_red:latest bigobject/bigobject-rpi-arm32:PINE64 node:8.6-slim buildpack-deps:jessie-curl ```



