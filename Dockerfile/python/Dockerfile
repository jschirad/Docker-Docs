FROM ubuntu

RUN apt-get update \
	&& apt-get install -y python

RUN echo 1.0 >> /etc/version && apt-get install -y git \
	&& apt-get install -y iputils-ping
### WORKDIR ###
RUN mkdir /datos
WORKDIR /datos
RUN touch fichero.txt

RUN mkdir /datos1
WORKDIR /datos1
RUN touch fichero1.txt

### COPY ###
COPY index.html .
COPY app.log /datos

### ADD ###
#ADD docs docs
#ADD f* /datos/
#ADD f.tar .

### ENV ###
ENV dir=/data dir1=/data1
RUN mkdir $dir && mkdir $dir1

### AGR ###
ARG dir2
RUN mkdir $dir2

### VOLUME ###
ADD paginas /var/www/html
VOLUME ["/var/www/html"]

#CMD ["echo", "Welcome this container"]
#CMD ["/bin/bash"]
CMD echo "Welcome to this container"

#ENTRYPOINT ["/bin/bash"]
