# Python Web Page
A python web page using Docker, Visualizer and Redis DB 

## Run and Test
$ git clone https://github.com/kmponis/python-web-page.git
$ cd python-web-page
$ docker-compose up
$ curl <localhost or ip_address>:4000
$ open http:/<localhost or ip_address>:9080 // Check visualiser

## Learning Resources
#### Website: https://www.youtube.com/watch?v=gHlRpZJ768U
$ git branch
$ git checkout -b stable
$ git push origin stable
$ git checkout -b dev
$ git push origin dev

#### Website: https://docs.gitlab.com/ee/ci/docker/using_docker_build.html
$ sudo ssh root@192.168.1.87
$ sudo gitlab-runner register -n --url https://gitlab.com/kmponis/web-page.git --registration-token M_RssmCBWZkK_XKHJ-Li --executor shell --description "My Runner"