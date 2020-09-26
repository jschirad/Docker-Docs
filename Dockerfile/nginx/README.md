### Imagen de Nginx corriendo en Alpine

	> docker build -t nginxV1 .
	> docker run -d -p 80:80 -p 443:443 -p 22:22 nginxV1

	> localhost:80 -> http/https
	> localhost:443 -> https
	> ssh root@localhost -p 22:22
