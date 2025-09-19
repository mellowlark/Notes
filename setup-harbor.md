Add a project
Tag an image with hostname:8080/project/image-name[:TAG]
edit /etc/docker/daemon.json
add { "insecure-registries":["hostname:8080"] }
reload docker service sudo systemctl reload docker
docker login (enter user\password when prompted)
push hostname:8080/project/image-name[:TAG]
