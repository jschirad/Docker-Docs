# Redes en Docker

### Clase 1
	
	- Un contenedor puede tener aplicaciones que necesiten ser accedidas desde afuera del contenedor, por ejemplo Apache o Tomcat.
	- Por defecto los puertos de un contenedor son privados y no pueden ser accedidos.
	- Debemos hacerlos publicos y mapearlos con un puerto del host.

### Clase 2

	Vamos a utilizar a continuacion una imagen de Nginx. Mas precisamente la version nginx:lastes.
	Dentro de la documentacion de Docker Hub podemos ver la configuracion de cada imagen, donde podemos ver que el puerto por defecto en este caso es el 80.
	
	> docker pull nginx

	Comprobamos con > docker images que tenemos la imagen descargada.
	
	Si queremos correr el contenedor y exponerlo, podemos utilizar el flag (-P) el cual expondra el puerto de la imagen. Nos asignara un puerto aleatorio para conectarnos por nuestro host principal. Ejecutamos lo siguiente.

	> docker run -d -P nginx
	> docker ps

	Aqui podremos ver el puerto que fue asignado para conectarnos a nuestro servicio.

	En el caso de querer asignar un puerto personalizado, ejecutamos: 

	> docker run -d -p 8080:80 nginx

	Para acceder al servicio nos dirigimos a localhost:8080.

### Clase 13

	Enlazar un contenedor Mysql y un contenedor Wordpress, utilizando una red personalizada.

	> docker run -d --name mysql_wp --rm --network red1 -e MYSQL_ROOT_PASSWORD=secret mysql 
	> docker run -d --name wp --rm --network red1 -e WORDPRESS_DB_HOST=mysql_wp - WORDPRESS_DB_PASSWORD=secret -p 8080:80 wordpress   

	De esta manera tendremos los dos contenedores corriendo en la misma red.
	Podemos entrar al contenedor de mysql para ver si creo la base de datos para wordpress.

	> docker exec -it mysql_wp bash
	> bash: myqsl -u root -p
	> password: secret

	Dentro del cliente de mysql podemos ejecutar los siguientes comandos para ver las tablas creadas.

	> show database
	> use wordpress;
	> show tables;

	Si nos dirigimos a localhost:8080 podremos ver el instalador de Wordpress.
