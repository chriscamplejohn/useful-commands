# Intro
These are useful commands for interacting with docker. They assume you are on a machine running docker.

### Stop All Containers
```docker stop $(docker ps -q)```

### Remove All Containers
```docker rm $(docker ps -aq)\n```