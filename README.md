# from root folder of git project
```bash

  docker build  . -t muz/docker-git-acccess-root-folder;

  docker run --name root-folder-git-success muz/docker-git-acccess-root-folder;
```

## Result

success

### Reason

The .git folder was copied into the build

```docker
FROM golang:1.19 as build
COPY . .
RUN echo `git rev-parse --short HEAD` > foo.txt
CMD ["cat", "foo.txt"]
```

# from sub-folder

```bash
  cd subfolder-fail;

  docker build . -t muz/docker-subfolder-git-fail;

  docker run --name subfolder-git-fail muz/docker-subfolder-git-fail;
```
## Result

Failure

### Reason

no access to git, and no access to the .git folder as the build is executed off of root.

```docker
FROM golang:1.19 as build
COPY . .
RUN echo `git rev-parse --short HEAD` > foo.txt
CMD ["cat", "foo.txt"]
```


# from sub-folder with passed in build arguments

```bash
  cd ../subfolder-success;

  docker build --build-arg GIT_HASH=`git rev-parse --short HEAD` . -t muz/docker-subfolder-git-success;

  docker run --name subfolder-git-success muz/docker-subfolder-git-success;
```


## Result

Success

### Reason

Evaluate the value outside the container and pass it into the build.

**ARG GIT_HASH**
**RUN echo ${GIT_HASH} > foo.txt**

```docker
FROM golang:1.19 as build
ARG GIT_HASH
COPY . .
RUN echo ${GIT_HASH} > foo.txt
CMD ["cat", "foo.txt"]
```
