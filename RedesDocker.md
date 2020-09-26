# Redes en Docker

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
