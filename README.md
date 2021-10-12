# Omega Manager Docker
A docker based version of Omega Manager - a DayZ Server Manager  
(Currently only works with DayZ Experimental.)
## Prerequisites  
You will need to following things for this setup:
1. A linux based system with docker, docker-compose and git installed. (Click [here](https://docs.docker.com/get-docker/) for more details on how to install docker.)
2. A Steam account that owns DayZ
3. A Steam API Key. (Click [here](https://docs.cftools.cloud/omegamanager) and look at the section "Steam API Key" for more details on how to obtain it.)  
## Installation
Execute the following steps to get your Omega Manager Docker environment up and ready.
1. Clone this repository:  
```shell
cd ~
git clone https://github.com/Blenderpics/Omega-Manager-Docker.git
cd Omega-Manager-Docker
```  
2. Create the necessary directory structure and make sure the permissions are setup correctly:
```shell
mkdir container/omega/conf
mkdir container/omega/data
chown 1000:1000 container/omega/conf # Omega Manager will be executed by UID 1000 in the container, so this is important
chown 1000:1000 container/omega/data # Omega Manager will be executed by UID 1000 in the container, so this is important
chown 755 container/omega/conf
chown 755 container/omega/data
```  
3. Build the docker container  
```shell
docker-compose build --no-cache
```
4. Execute the setup and follow the prompts (and be sure to select "y" when asked "Do you want to use DayZ Experimental?":  
```shell
docker-compose run omega Setup # Follow the instructions and enter your steam credentials + api key
```  
## Running Omega Manager
After you installed Omega Manager, you can start it by executing `docker-compose up -d`.  
Shortly after starting Omega Manager it's webserver will be reachable at `https://localhost:8443`. (Ignore the SSL cert warning.)  
You will be able to spawn your first DayZ server from there.  
If you want to see Omega Managers log output, execute `docker-compose logs -f`. (Press CTRL+C to cancel.)  
To shutdown Omega Manager, execute `docker-compose down`.  
To update Omega Manager, run `docker-compose build --no-cache && docker-compose down && docker-compose up -d`.  
In case you need to edit the DayZ mission files or change some mod settings, you can find your DayZ server instances at `container/omega/data/servers`.
## Disclaimer
The DayZ server for linux is still considered experimental.  
You will most likely experience some weird quirks specific to the linux variant of the DayZ server.  
Omega Manager for linux is also still considered experimental. Stability is not guaranteed.  
  
Omega Manager itself is developed by CFTools™ Software UG (haftungsbeschränkt).  
See https://cftools.com/ to learn more.
