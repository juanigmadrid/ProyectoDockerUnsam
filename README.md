# Pasos de instalación.

- Clonamos el repositorio en nuestro equipo:
> $ git clone https://github.com/juanigmadrid/ProyectoDockerUnsam.git
- Crear network para proxy:
``$ docker network create proxy-network``
- Correr docker-compose para levantar el proxy:
``$ 

# Gitlab Container.

Aquí definiremos un contenedor de GitLab utilizando la última imagen oficial y definiendo su configuración en un archivo de docker-compose.

## .env

El archivo .env contiene las variables de entorno para el contenedor.
- **HOST**: se utiliza para indicar el hostname que se utilizará para que el reverse proxy rediriga las peticiones a este contenedor.
- **GITLAB_HOME**: aquí se define la ruta local en la que queremos que gitlab guarde sus configuraciones.
- **SSH_PORT**: definimos el puerto que queremos usar para el uso de git por SSH, ya que tenemos instalado un ssh server en el puerto 22 del equipo host, deberemos utilizar uno diferente para gitlab.

## Opciones definidas en docker-compose.yml

- **image**: Aquí le indicamos que utilice la última versión de la imagen oficial de GitLab.
- **restart**: Le indicamos que queremos que el contenedor se inicie automáticamente junto con docker, y que también se reinicie automáticamente en caso de algún fallo o interrupción
- **hostname**: Aquí colocamos el fqdn a utilizar. Es una opción requerida según la documentación oficial de GitLab.
- **networks**: Debido a que tenemos varios contenedores en el mismo servidor que deben escuchar por el puerto 80 y 443, utilizaremos un contenedor que hará de reverse proxy para redirigir las escuchas de estos puertos a los contenedores correspondientes segun el hostname indicado. Para esto necesitamos que ambos contenedores estén en la misma red interna de docker, por lo que creamos una red "proxy-network" en docker con el comando *docker network create proxy-network* para utilizar en nuestros contenedores.
- **environment**: Aquí definimos más variables de entorno con configuraciones específicas para gitlab. Entre ellas indicamos la URL externa que utilizaremos, definimos el puerto de escucha de SSH y le aclaramos al motor nginx que escuche en el puerto 80 ya que los certificados TLS los administra el proxy reverso.
- **ports**: Redirigimos previamente seleccionado del host al 22 del container para utilizar el SSH en GitLab.
- **volumes**: Utilizamos bind mounts para redirigir carpetas locales al contenedor.
