FROM golang:1.19 as build
COPY . .
RUN echo `git rev-parse --short HEAD` > foo.txt
CMD ["cat", "foo.txt"]
