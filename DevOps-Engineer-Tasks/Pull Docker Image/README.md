## Login to the respective mentioned server in the task using ssh then switch to root user

```
ssh username@server

sudo su -
```

## Pull busybox image from dockerhub using docker pull

```
docker pull busybox:musl
```
#### Output:

```
musl: Pulling from library/busybox
9bdc5982aa6f: Pull complete 
Digest: sha256:e611393eb57a903f0efff7328506e991abb84ca9794c519a4e1f42483a582732
Status: Downloaded newer image for busybox:musl
docker.io/library/busybox:musl
```

## List all images:
```
docker images
```

#### Output:

```
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
busybox      musl      f2e900173e30   10 days ago   1.4MB
```

## Tag the pulled image using docker image tag 
```
docker image tag busybox:musl busybox:blog
```

## List all images to validate the task:

```
docker images
```

#### Output:

```
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
busybox      blog      f2e900173e30   10 days ago   1.4MB
busybox      musl      f2e900173e30   10 days ago   1.4MB
```
