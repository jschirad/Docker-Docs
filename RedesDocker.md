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


### Clase 3

	> docker network ls

	Este comando nos listara las redes que tenemos disponibles para trabajar.
	
	Aqui encontramos 3 tipos de redes para utilizar.

	> bridge : Red privada. Puede conectarse con el exterior, que en este caso seria la maquina nuestra. Nivel local. Red que utilizan los contenedores de manera predeterminada.

	> host : Los contenedores que pertenecen a esta red, no pueden verse entre si. Se pueden conectar a la maquina principal. Mas que nada puede ser utilizada para contenedores independientes.

	> none/null : Para contenedores que no tiene ninguna red asociada.

	Para saber a que red pertenece un contenedor podemos ejecutar :

	> docker inspect [contenedor] 

	Dentro de las propiedades NetworkSettings podemos apreciar a la red que pertenece el contenedor.


### Clase 4

	> docker network ls : Vemos las redes que tenemos disponibles.

	> docker network inspect bridge : Inspeccionamos la red.

	Aqui podemos apreciar el rango de ips que seran asociadas a los contenedores.

	Tambien podemos apreciar los contenedores que estan asociados a la red actualmente, en el apartado de containers.
	
	
### Clase 5

	Configurar Puertos

	Usamos una imagen de MongoDB. Vamos a averiguar los puertos por los que escucha. Podemos usar el comando inspect.

	Por supuesto, lo mas rapido es comprobar la documentacion de la imagen en Docker Hub.

	En este caso comprobamos que escucha por el puerto 27017.

	Creamos un contenedor llamado mongo1 que escuche por el mismo puerto en el host.

	> docker run -d -p 27017:27017 --name mongo1 mongo

	Comprobamos que funcione y los puertos por los que escucha.

	> docker ps

	Tambien podemos usar el comando PORT.

	> docker port mongo1
	
	Ahora comprobaremos las redes de docker.

	> docker network inspect bridge 
	
	En el apartado de contenedores podemos ver la direccion ip asociada a nuestro contenedor mongo1.
	
	Podemos probar que llegamos al contenedor con un ping.

	> ping $ip_mongo1



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
