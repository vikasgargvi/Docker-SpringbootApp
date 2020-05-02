# Docker-SpringbootApp

docker-compose file pull the 4 images of centos on which spring boot application in deployed

# Image 1 (Eureka server PORT 8888) :
                This image run jar file. This is used to discover all the service availabe on the network.
# Image 2 (ZUUL Proxy server PORT 8762) :
                This image also run jar file which act as a gateway for all the all the micro-service availabe on eureka server and fetch                   details of services and help to intract serveics with the Internet. PORT 
# Image 3 and 4 :
                These two service (servic1 PORT 8000, service2 PORT 8001) are for demo purpose just to show on which ip address and port they are running.
             
Here DOCKER part comes in, All the services already isolates by zuul proxy server, but by usigng docker in this project I povide further isolation by containerize all the service on a user-defined bridge network.

Docker Features used:
- Created all the images using dockerfile with EntryPoints.
- push all the images on docker hub
- use docker-compose.yml file for :
      - create user-defined bridge network
      - create seperate volumes for all
      - port mapping for (eureka server and zuul proxy server only)
      - run docker container
      
# How to Use:
I'm running docker on VM RHEL 8, RHEL 8 is host machine for docker
1. start firewalld
2. docker-compose up
Wait until all the services is started successfully
Now you can run from your host machine
url: localhost:8888 (for Eureka server, can check if the services is registered or not )

If all the service registered successfully then, 
localhost:8762/service1/   (for accessing service1)
localhost:8762/service2/   (for accessing service2)

Here interesting this is, 
service1 is running on port 8000 but we are accessing it on port 8762, because zuul proxy server doing this so we not need to specify ip addr and port for every service, zuul is doing for us.


# For Accessing outside your host machine
3. stop firewalld

wait for few seconds
then,
from your windows/macOS:

http://ipAddrOfRHEL8:8762/service1/
http://ipAddrOfRHEL8:8762/service2/

Here,
<ip addr of RHEL8> : request first goes to your RHEL8 VM machine
port 8762: then req is forwarded to zuul server
/service1: then service1 micro-service will be invoked

hence you will get your output
