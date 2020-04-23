# Smart Home demo application using Spring Boot, Websockets and Active MQ
Websocket client-server example app, with ActiveMQ message broker. 
This is the code repo for the dzone article: <Url here>

## Description 
In our scenario, all the smart devices have a persistent connection to a server. The server is responsible for sending commands to specific devices, such as turning on the living room lights, or enabling the alarm. It can also receive information from devices. For example there can be a temperature sensor that takes readings every minute, or an oven that sends alerts if the temperature is too high. Finally the server may also issue commands to all devices, such as turn on/off.

![Image of Microservices](/screenshots/websockets-2.png)

Each microservice (MS) is written in Java 13, using the Spring Boot framework. The communication with the clients is handled by the Device Management MS. The Control MS exposes the REST API, and communicates with the Device Mgmt MS using an Active MQ Artemis message broker. For incoming traffic routing, service discovery and load balancing we are using Spring Cloud Gateway and Eureka.

## Prerequisites

* Java 11
* Docker

## How to install

From root directory type:

> mvn package -DskipTests

To build the docker images run:

> docker-compose build

## How to run

To dun with docker, after creating the images, navigate to root directory and type:

> docker-compose up

This will bring up the server and one client 

## How to test

* Monitor the services discovered by Eureka by visiting: http://localhost:8761
* The ActiveMQ artemis comes with a management page running at: http://localhost:8161
* You can send POST requests to http://localhost:8000/control-service/device 

```
curl --location --request POST 'http://localhost:8000/control-service/device' \
--header 'Content-Type: application/json' \
--data-raw '{
	"destination": "lights_living_room",
	"command": "turn_on",
	"args": {
	}
}'
```

Then you can monitor the device client / cdm service logs to see the transmitted messages
