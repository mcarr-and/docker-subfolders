FROM golang:1.19 as build
ARG GIT_HASH
COPY . .
RUN echo ${GIT_HASH} > foo.txt
CMD ["cat", "foo.txt"]
