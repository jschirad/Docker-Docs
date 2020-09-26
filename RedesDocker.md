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

### Clase 6

	> docker network create --help

	Con este comando podemos acceder a un manual de ayuda para crear nuestra red personalizada. Por defecto si creamos una red personalizada, sera una red de tipo bridge.

	> docker network create red1

	> docker network ls

	Podemos apreciar que tendremos una nueva red llamada red1 que sera de tipo bridge.

	> docker network create --subnet=192.168.0.0/16 red2

	De esta manera creamos una nueva red que estara sujeta al rango que asociamos. De igual manera sera de tipo bridge.


### Clase 7

	Asociar contenedores a una Red.

	> docker network ls

	> docker run -it  --name ubuntuA --network red1 ubuntu

	> docker ps

	De esta manera tendremos un contenedor con un Ubuntu corriendo y ademas estara asociado a la red 'red1'.

	> docker run -d --name nginx2 --network red1 nginx

	En este caso tendremos el contenedor de ubunutu y nginx corriendo en la misma red.

	Si entramos al contenedor de ubuntu y ejecutamos un ping $ip al contenedor de nginx podemos ver que existe una conexion entre ellos.

	> docker network connect red2 ubuntuA

	Con este comando podemos asociar un contenedor a una red personalizada en caliente. Asi tambien podemos desconectarlo.

	> docker network disconnect red2 ubuntuA

### Clase 8

	Creacion de Redes y asociarlas a contenedores.

	> docker network ls

	> docker network create net1

	> docker run -d -p 27017:27017 --network net1 --name mongo1 mongo

	> docker ps

	> docker network inspect net1

	Aqui podremos ver que el contenedor de mongo esta asociado a la red net1.

	Desde otro terminal.

	> docker run -d -p 27018:27017 --name mongo2 --network net1 mongo

	> docker ps

	> docker network inspect net1

	Podemos apreciar que dentro de la red tenemos dos contenedores de mongo asociados.

	Asi mismo podemos hacer ping de un contenedor a otro para comprobar la conexion entre ellos.

### Clase 9

### Clase 10

### Clase 11

### Clase 12

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


### Clase 14

### Clase 15
