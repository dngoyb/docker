FROM golang

MAINTAINER  Ngoy Banza <dngoyb@gmail.com>

RUN apt-get update -y && apt-get upgrade -y \
	&& apt-get install git wget bzip2 -y

RUN adduser --disabled-login --gecos 'Gogs' git
USER git

WORKDIR /home/git

EXPOSE 3000

#Download and untar Go source code 

RUN mkdir -p local \
	&& wget https://dl.google.com/go/go1.11.linux-amd64.tar.gz \
	&& tar -C /home/git/local -xzf *.tar.gz

#Set environments

ENV GOROOT=/home/git/local/go
ENV GOPATH=/home/git/go
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

#Install Go 
#Build from develop branch

RUN mkdir -p  $GOPATH/src/github.com/gogs \
	&& cd $GOPATH/src/github.com/gogs \
	&& git clone --depth=1 -b develop https://github.com/gogs/gogs \
	&& cd gogs \
	&& go build

#adding sql tags

RUN go get -u -tags "sqlite" github.com/gogs/gogs \
	&& cd $GOPATH/src/github.com/gogs/gogs \
	&& go build -tags "sqlite"

#Git user sudo access
USER root

WORKDIR $GOPATH/src/github.com/gogs/gogs
CMD ["./gogs", "web"]

#docker build -t gogs .
#docker run -it --rm --name yes gogs
