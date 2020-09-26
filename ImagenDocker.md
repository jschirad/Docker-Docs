## Imagenes en Docker / Dockerfiles

### Clase 1

	Las imagenes de docker estan formadas por varias capas solo de lectura.

	Ver estructura de un contenedor.


### Clase 2

	Modificar un contenedor.

	> docker run -it --name ubuntu1 ubuntu bash

	> wget http://www.google.com

	Este comando de normal no funcionaria por que no tenemos instalado el comando wget.

	Si instalamos el comando wget, el contenedor habra sufrido un cambio y este sumara a su contenido el comando que descargamos. En este caso wget.

	> docker stop [contenedor]

	Si salimos del conetedor. Este se detendra.

	> docker start -i ubuntu1

	Si volvemos a iniciar el contenedor, podremos seguir utilizando el comando wget.

	> docker diff ubuntu1

	Con este comando podremos ver los cambios que tuvo el contenedor. (A) Add (C) Change (D) Delete.

### Clase 3

	Docker commit. Creando una imagen.

	En este caso queremos crear un nuevo contenedor que tenga el comando wget ya instalado.

	> docker commit ubuntu1 mi_ubuntu
	> docker commit [contenedor principal] [contenedor copia]

	De esta manera podemos ver la nueva imagen

	> docker images

	Podemos comprobar asi que se creo una imagen llamada mi_ubuntu. En este caso si corremos la imagen, el contenedor podra correr el comando wget pre instalado.

	En principio no es una practica recomendada. Pero en ocasiones puede servirnos.


### Clase 4

	Dockerfile.

	Archivo de configuración. El contenedor ser crea a partir de los comandos que encontramos en los dockerfiles.

	> docker image history [imagen]

	Podemos los cambios que fue sufriendo la imagen.

	RUN : comando para correr comandos dentro del contenedor. Por cada run se genera una capa, lo mejor es utilizar '&&' para enlazar comandos.

	CMD : Directiva que nos permite indicar el comando por defecto del contenedor. Una vez que inicia el contenedor, este ejecutara el comando que indique el CMD.

	Es recomendable usar el formato -json junto al CMD.

	ENTRYPOINT : Funciona de manera similar a CMD con la diferencia que al comando que ejecuta el CMD lo podemos modificar a la hora de correr el contenedor. En el caso de terminar la imagen con Entrypoint no podemos modificar el comando que ejecutara el contenedor. Solo el ultimo es valido.

	WORKDIR : Nos permite determinar el directorio de trabajo para las directivas siguientes. Podemos tener multiples WORKDIR.

	COPY / ADD : Con estos comandos podemos copiar o agregar contenido a los directorios de mi contenedor.

	ENV : Genera variables de entorno dentro del contenedor.

	ARG : Para poner variables de entorno, con la diferencia que a la hora de construir el contenedor puedo cambair el valor de la variable de entorno.

	EXPOSE : Exponer puertos del contenedor.

	VOLUME : Crea un volumen en la dirección que le asignemos. El volumen sera un directorio externo al contenedor.  Este puede ser compartido con otro contenedor.
