# Docker
    // List all containers
    $ docker ps
    
    // List all images
    $ docker images 


## Basic Dockerfile
    # stage: 1
    # FROM node:9.3.0-alpine
    FROM node:8 as react-build
    WORKDIR /app
    COPY . ./
    RUN npm i
    RUN npm run build

    # Stage 2 - the production environment
    # FROM nginx:alpine
    # COPY nginx.conf /etc/nginx/conf.d/default.conf
    # COPY --from=react-build /app/build /usr/share/nginx/html
    # EXPOSE 80
    # CMD ["nginx", "-g", "daemon off;"]
    
## Run CRA App (Windows)
    docker run -p 3000:3000 <app-name> npm start

# Docker - How to cleanup (unused) resources

Once in a while, you may need to cleanup resources (containers, volumes, images, networks) ...
    
## delete volumes
    
    // see: https://github.com/chadoe/docker-cleanup-volumes
    
    $ docker volume rm $(docker volume ls -qf dangling=true)
    $ docker volume ls -qf dangling=true | xargs -r docker volume rm
    
## delete networks

    $ docker network ls  
    $ docker network ls | grep "bridge"   
    $ docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
    
## remove docker images
    
    // see: http://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images
    
    $ docker images
    $ docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
    
    $ docker images | grep "none"
    $ docker rmi $(docker images | grep "none" | awk '/ / { print $3 }')

## remove docker containers

    // see: http://stackoverflow.com/questions/32723111/how-to-remove-old-and-unused-docker-images
    
    $ docker ps
    $ docker ps -a
    $ docker rm $(docker ps -qa --no-trunc --filter "status=exited")
    
## Resize disk space for docker vm
    
    $ docker-machine create --driver virtualbox --virtualbox-disk-size "40000" default
    
