# PTP

PTP made easy using a Docker container. This repo uses PTP4L daemon as-is from http://git.code.sf.net/p/linuxptp/code. 


**Pre-Reqs**

- You need the latest Docker version installed.


**Building the Docker Container for PTP4L**

- In the ptp folder execute: 

sudo docker build -f ./Dockerfile.ptp -t ptp4l:1.0 .


**Running the Docker Container for 'PTP as a Client'**

- Start in the repo ptp folder


- Disable Ubuntu 20.04 time sync service"

sudo systemctl stop systemd-timesyncd <br>
sudo systemctl status systemd-timesyncd

- Start PTP as a Client

If you DO NOT have an 802.1AS network switch execute: <br>
sudo docker run -d --privileged -it --rm -v `pwd`/default.cfg/:/configs/default.cfg -v `pwd`/start-ptp-client.sh:/scripts/start-ptp.sh --net host ptp4l:1.0 bash -c "/scripts/start-ptp.sh"

If you DO have an 802.1AS network switch execute:<br>
docker run -d --privileged -it --rm -v `pwd`/gPTP.cfg/:/configs/default.cfg -v `pwd`/start-ptp-client.sh:/scripts/start-ptp.sh --net host ptp4l:1.0 bash -c "/scripts/start-ptp.sh"


**Running the Docker Container for 'PTP as the Primary (master)'**

- Start in the repo ptp folder


- Disable Ubuntu 20.04 time sync service

sudo systemctl stop systemd-timesyncd <br>
sudo systemctl status systemd-timesyncd

- Start PTP as a Primary

If you do not have a 802.1AS network switch execute:<br>
echo "Start PTP using default config which doesn't require 802.1AS switch
docker run -d --privileged -it --rm -v `pwd`/default.cfg/:/configs/default.cfg -v `pwd`/start-ptp-primary.sh:/scripts/start-ptp-primary.sh --net host ptp4l:1.0 bash -c "/scripts/start-ptp-primary.sh"

If you DO have an  802.1AS network swtich execute:<br>
echo "Start PTP-SW (not all devices support PTP in HW)"
docker run -d --privileged -it --rm -v `pwd`/gPTP.cfg/:/configs/default.cfg -v `pwd`/start-ptp-primary.sh:/scripts/start-ptp-primary.sh --net host ptp4l:1.0 bash -c "/scripts/start-ptp-primary.sh"


**Verify the Docker Container and PTP4L are running and are healthy**

- Get the _running container ID_ using: sudo docker ps
- sudo docker logs _running container ID_
- Results should not show "Waiting for PTP". If this occuring then PTP is not working. Results should show offsets between PHC/system time and GM


