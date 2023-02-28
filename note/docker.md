is:: [[note]]
from:: [[hacking]]

# Notes
Hints for exploring a docker image

## Inspect image metadata
```docker inspect <image>```

## Look at the history
Gives the commands used to build the image.

```docker history <image>```

## Try to get a shell on it
```docker run -it <image> sh```

If the software on the image throws errors, try to bypass the entrypoint.
```docker run -it --rm  --entrypoint "/bin/bash" <image>```
* the ```--rm``` option deletes the container after exiting. Useful when running an image over and over

## Hacking an image
If the repository is accessible for writing, try to add a payload to get code execution. Inspect the entrypoint to find the best place to put it.

1) Run the image (without --rm). Make changes. Exit. Then find the container name.

```
docker run -it --entrypoint "/bin/bash" <image>
docker container ls -a
```

2) Use the container to create a new image. Push the image into the repository under the ```latest``` tag. This uses a temporary image name called ```hacked-image```. It may be possible to commit directly into ```<target-image>:latest```, but haven't tried yet.

```
docker commit -m "hacked" <container-name> hacked-image
docker image tag hacked-image:latest <target-image>
docker image ls
docker image push <server>/<target-image>
```
