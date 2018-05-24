# Build base image

cd Kubernetes/image-build/mongodb  
source build.sh

docker image ls  
```shell
REPOSITORY     TAG     IMAGE ID         CREATED        SIZE  
mongodb      3.6.5    b500b08fbea1   15 seconds ago    523MB
```

