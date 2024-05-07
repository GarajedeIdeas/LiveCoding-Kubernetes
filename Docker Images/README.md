# Construir y probar las cuatro imágenes Docker que utilizaremos:
- Abre la línea de comandos que más te guste, en el Live Coding uso Windows Developer PowerShell que no es la que más me gusta, pero ahí está
- Construye las cuatro imágenes ejecutando ***docker build*** y súbelas a tu repositorio Docker con ***docker tag*** y ***docker push***

`$ cd LiveCoding-Kubernetes/Docker Images/v0.0`

`$ docker build -t hola-kubernetes/basica:0.0 .`

`docker image tag hola-kubernetes/basica:0.0 marisamartinserrano/hola-kubernetes-basica:0.0`

`docker image push marisamartinserrano/hola-kubernetes-basica:0.0`

`$ cd LiveCoding-Kubernetes/Docker Images/v1.1`

`$ docker build -t hola-kubernetes/basica:1.1 .`

`docker image tag hola-kubernetes/basica:1.1 marisamartinserrano/hola-kubernetes-basica:1.1`

`docker image push marisamartinserrano/hola-kubernetes-basica:1.1`

`$ cd LiveCoding-Kubernetes/Docker Images/BLUE`

`$ docker build -t hola-kubernetes/blue:blue .`

`docker image tag hola-kubernetes/blue:blue marisamartinserrano/hola-kubernetes-blue:blue`

`docker image push marisamartinserrano/hola-kubernetes-blue:blue`

`$ cd LiveCoding-Kubernetes/Docker Images/GREEN`

`$ docker build -t hola-kubernetes/green:green .`

`docker image tag hola-kubernetes/green:green marisamartinserrano/hola-kubernetes-green:green`

`docker image push marisamartinserrano/hola-kubernetes-green:green`

- Lista las imágenes que acabas de crear con con ***docker image ls***

`$ docker image ls`

![alt tag](./Screenshots/docker-image-ls.png)

- Sube las imágenes al repositorio de Docker (o al que prefieras) ***docker push***
docker image tag hola-kubernetes/basica:0.0 marisamartinserrano/hola-kubernetes/basica:0.0
docker push marisamartinserrano/hola-kubernetes/basica:0.0

### Nota
- Si quieres probar alguna imagen por ejemplo la blue, puedes ejecutar su contenedor con ***docker run***:

`$ docker run -p 8080:80 --name blue_blue -d hola-kubernetes/blue:blue`

`$ docker container ls`

![alt tag](./Screenshots/docker-container-ls.png)

- Ábrela en tu navegador escribiendo localhost:8080. Sí, es así de azul :D

![alt tag](./Screenshots/localhost8080.png)
- Recuerda borrar el contenedor (pero no la imagen!)

`$ docker rm -f blue_blue`
