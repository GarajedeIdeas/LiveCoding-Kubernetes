# livecoding-kubernetes
## Construir y probar las cuatro imágenes Docker que utilizaremos:
- Abre la línea de comandos que más te guste, en el Live Coding uso Windows Developer PowerShell
- Construye las cuatro imágenes ejecutando *docker build*
`$ cd LiveCoding-Kubernetes/Docker Images`
`$ docker build -t hola-kubernetes/basica:0.0 source/repos/GarajedeIdeas/LiveCoding-Kubernetes/Docker Images/v0.0`
`$ docker build -t hola-kubernetes/basica:1.1 source/repos/GarajedeIdeas/LiveCoding-Kubernetes/Docker Images/v1.1`
`$ docker build -t hola-kubernetes/blue:blue source/repos/GarajedeIdeas/LiveCoding-Kubernetes/Docker Images/BLUE`
`$ docker build -t hola-kubernetes/green:green source/repos/GarajedeIdeas/LiveCoding-Kubernetes/Docker Images/GREEN`
`$ docker image ls`
### Nota
- Si quieres probar alguna imagen por ejemplo la blue, puedes ejecutar su contenedor con *docker run*:
`$ docker run -p 8080:80 --name blue_blue -d hola-kubernetes/blue:blue`
`$ docker container ls`
- Ábrela en tu navegador escribiendo localhost:8080
- Recuerda borrar el contenedor (pero no la imagen!)
`$ docker rm -f blue_blue`


## Setup
- Laptop OS: Windows 11 Home
- Docker Desktop 4.28.0
- Minikube v1.32.0
- Kubectl v1.21
- Helm: v3.6.0 
- Visual Studio: Microsoft Visual Studio Community 2022 (64-bit) - v17.9.6

## Materiales
- Este repo =)
- `<link>` : <https://github.com>
- `<Presentación de este Live Coding>`: <https://view.genial.ly/6637f68001b64c001572a91a>
- `<Presentación relacionada en la SalmorejoTech 2024 "Microservicios. Contenedores. Kubernetes todo a la vez!">`: <https://view.genial.ly/66184816e948a90013c70712>

## Créditos y méritos:
- `<Creación imágenes Docker>`: <https://github.com/Einsteinish/docker-nginx-hello-world/tree/master>
- `<Logo Kubernetes>`: <https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Kubernetes_logo_without_workmark.svg/2109px-Kubernetes_logo_without_workmark.svg.png>
- `<Creación presentaciones interactiva (puedes hacer juegos, quizzes, escape rooms, presentaciones...)>`: <https://app.genial.ly/>
- `<Sonido en la presentación de este Live Coding>`: <https://pixabay.com/es/sound-effects/relaxing-ocean-waves-high-quality-recorded-177004/>
- `<Generador QR>`: <https://www.codigos-qr.com/generador-de-codigos-qr/

## Muchas muchísimas gracias...
- Ana por toda la ayuda
- Garaje de Ideas por la oportunidad
- A ti por este ratito!