# ProyectoDockerUnsam
Entorno de desarrollo corriendo en containers con docker-compose.

## Crear docker network
Tenemos que crear una red de docker, ya que como el contenedor de gitlab-runner necesita datos del gitlab local, no podremos correrlos en simultaneo en un solo compose.
``
docker network create gitlab_network
``