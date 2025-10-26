# Backstage

## Software Catalog
## Tech Docs
## Software Templates

## Deploy Backstage using Docker I
1. Go to google type backstage installation -> https://backstage.io/docs/getting-started/
2. Look for prerequsistes
3. go to terminal and type dockerimages -> take https://hub.docker.com/_/node
4. docker pull node:20-bookworm-slim
5. we do this because we need npx to install backstage
6. create folder  mkdir backstage-app under ~
    the reason we need to do in wsl as if we do it in windows it has performance issues -> https://github.com/docker/for-win/issues/6742
7. find the path /home/tara/backstage-app
8. run this inside the folder 
    docker run --rm -p 3001:3000 -ti -v /home/tara/backstage-app:/app -w /app node:20-bookworm-slim bash  //delete the container once exitet thats why we have --rm, ti is inside the container, -w is working directory
9. Once inside the directory check pwd
10. go back here and check installation https://backstage.io/docs/getting-started/
11. run this npx @backstage/create-app@latest
    1. provide name, hit enter for defailt 
12. Wait around 5 mins
13. once done type ls it should have dir backstage
14. go inside backstage dir and typ yarn dev
        yarn start
15.  http://localhost:3000 - does not work
16. open new terminal do docker ps
17. docker exec -ti 60d07bcc781b bash
17.1 
    apt-get update
    apt-get install curl
18. curl http://localhost:3000/ from inside container and it works
19. from browser a problem
20. Expose it now
    1. GO to the container inside /app/backstage and find app-config.yaml\
    2. run more app-config.yaml
    3. install nano 
        apt-get install nano
    4. nano app-config.yaml
    5. modify app 
        app:
        title: Scaffolded Backstage App
        baseUrl: http://localhost:3000
        listen:
            host: 0.0.0.0
    6. yarn start
    7. go to localhost:3000 in the browser
    8. exit backstage
    9. exit container