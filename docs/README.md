# Deploying PhaseNet Service on AWS EC2 instance using Docker

## Local machine
```
chmod 400 eqw_phasenet.pem
ssh -i "eqw_phasenet.pem" ec2-user@ec2-34-216-250-65.us-west-2.compute.amazonaws.com
```

## Remote machine
```bash
sudo dnf update
sudo dnf install docker
sudo systemctl start docker
sudo systemctl enable docker #enable on boot
sudo usermod -aG docker $USER
## Log out and log back in

mkdir phasenet_service
cd phasenet_service

docker build -t phasenet-server .
docker run -d -p 7860:7860 --name phasenet-server phasenet-server

```

## Troubleshooting
```bash

docker logs phasenet-server

## Remove all stopped containers
docker rm $(docker ps -a -q)  

## To remove all images that are not in use by a container
docker system prune --all


## Get inside the docker container
docker exec -it phasenet-server /bin/bash

uvicorn --app-dir eqnet app:app --reload --host 0.0.0.0 --port 7860

```

## Application
http://<your-ec2-public-ip>:7860
http://ec2-34-216-250-65.us-west-2.compute.amazonaws.com:7860

