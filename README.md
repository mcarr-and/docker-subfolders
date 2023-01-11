# Repo to show what is available to you in Docker when the build is running

Docker will not always act the same depending on which directory you are in.

You will not always have access to the git information

## from root folder of git project
```bash

  docker build  . -t muz/docker-git-acccess-root-folder;

  docker run --name root-folder-git-success muz/docker-git-acccess-root-folder;
```

### Result

success: echo prints out the git short hash

### Reason

The .git folder was copied into the build

```docker
FROM golang:1.19 as build
COPY . .
RUN echo `git rev-parse --short HEAD` > foo.txt
CMD ["cat", "foo.txt"]
```

## from sub-folder with same Dockerfile contents

```bash
  cd subfolder-fail;

  docker build . -t muz/docker-subfolder-git-fail;

  docker run --name subfolder-git-fail muz/docker-subfolder-git-fail;
```
### Result

Failure: echo returns empty string

### Reason

no access to git, and no access to the .git folder as the build is executed off of root.

```docker
FROM golang:1.19 as build
COPY . .
RUN echo `git rev-parse --short HEAD` > foo.txt
CMD ["cat", "foo.txt"]
```


## from sub-folder while passed in build argument of the git hash

```bash
  cd ../subfolder-success;

  docker build --build-arg GIT_HASH=`git rev-parse --short HEAD` . -t muz/docker-subfolder-git-success;

  docker run --name subfolder-git-success muz/docker-subfolder-git-success;
```


### Result

success: echo prints out the git short hash

### Reason

Evaluate the value outside the container and pass it into the build.

**docker build --build-arg GIT_HASH=`git rev-parse --short HEAD` ... **

GIT_HASH now has a value that is passed into the docker build cycle.


You have to accept the build argument by adding **ARG GIT_HASH**

Then you can use the build argument GIT_HASH by evaluating it with ${GIT_HASH} **RUN echo ${GIT_HASH} > foo.txt**

```docker
FROM golang:1.19 as build
ARG GIT_HASH
COPY . .
RUN echo ${GIT_HASH} > foo.txt
CMD ["cat", "foo.txt"]
```
