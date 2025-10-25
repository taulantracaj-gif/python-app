INSTALL PYTHON
INSTALL PIP
code https://github.com/ricardoandre97/python-app 
1. INSTALL REQUIREMENTS 
 pip install -r .\requirements.txt

2. go to src
 python app.py

2.1 wsl -d Ubuntu

3. http://127.0.0.1:5000

4. go to https://hub.docker.com/_/python
    https://hub.docker.com/_/python/tags?name=3.10-alpine
5. Do the docker

6. build the image
  docker build -t python-app:v1 . 
  docker images

7. deploy deploy
  docker run -p 8098:5000 -d python-app:v1 
    http://localhost:8098/api/v1/info

8. go into container
  docker exec -ti 3582869de76c sh
  apk add curl
  curl http://localhost:5000/api/v1/info

9. go to hub.docker.com signup taulant.rracaj@gmail.com
 create a repository -> python-app
 create pat
   1. go to account 
   2. personal access tocken
   3. give it a name python-app
   4. select read and write
   5. save it 
   6. docker login -u tracaj
   7. docker images
   8. docker tag python-app:v1 tracaj/python-app:v1
   9. docker push tracaj/python-app:v1
10. DONE