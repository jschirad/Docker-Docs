FROM ubuntu:14.04

# Añadimos la clave PGP de POstgreSQL para verificacion.
# Deberia coincidir con https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

# Añadimos el repositorio de Postgre.
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

# Actualizamos repositorios
# instalamos python y postgreSQL
RUN apt-get update && apt-get -y -q install python-software-properties software-properties-common \
	&& apt-get -y -q install postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

# Nos cambiamos al usuario postggres, que se ha creado al instalar postgres
USER postgresql

# Creamos un usuario y su base de datos.
RUN /etc/init.d/postgresql start \
	&& psql --command "CREATE USER pguser WITH SUPERUSER PASSWORD 'secret';" \
	&& createdb -O pguser pgdb

# Cambiamos al usuario root
USER root

# Permitimos la conexion remota
RUN echo "host all all 0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf

# Exponemos el puerto de la base de datos
EXPOSE 5432

# creamos un directorio y asignamos permisos al usuario postgres
RUN mkdir -p /var/run/postgresql && chown -R postgres /var/run/postgresql

# Creamos los volumenes necesarios
VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

# Indicamos el comando a ejecutar al crear el contenedor
# Basicamente arrancar postgres con la configuracion adecuada.
CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]
