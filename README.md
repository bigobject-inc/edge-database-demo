# BigObject Edge Database Demonstration

[BigObject](http://www.bigobject.io) (BO) is a very light and efficient database with small footprint. 
You can even run it on [PINE64](https://www.pine64.org/). 
The demonstration shows a scenario that you can use BigObject + [Node-RED](https://nodered.org/) + [Mosquitto](https://mosquitto.org/) to make a naive alerting system with a regular cell phone and a pine64. 

## Prerequisite
1. A pine64 board. 
	1. OS: [Xenial Minimal](http://wiki.pine64.org/index.php/Pine_A64_Software_Release#Xenial_Minimal_Image)
	1. Docker >= 1.10.0
	1. Docker Compose >= 1.6.0 
1. An android phone with the app, [Sample MQTT Publisher](https://play.google.com/store/apps/details?id=com.hoop.accelerometer) 

## How to setup this demo
1. Get the source package from github
1. Get the ip address, *pin64_ip*, of the pine64 board. e.g. 192.168.1.123 
1. Run this command. It may take a while because of downloading docker images.  
```
cd edge-demo/compose; docker-compose up -d
```
1. Import the flow, edge-demo-flow.json, via the following command or use the Node-RED's GUI
```
curl -X POST -v -d "@edge-demo-flow.json" --header "Content-type: application/json" http://localhost:1880/flows
```
1. Open the app in the cellphone and set the hostname as *pine64_ip* and port as 1883
1. Press **Connect**, and keep the cellphone awake. 
1. Open the dashboard, http://*pine64_ip*:1880/ui/, in your browser. 
1. Move your cellphone and you will see the changes in the dashboard
1. Have fun!



