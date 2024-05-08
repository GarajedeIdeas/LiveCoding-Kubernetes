# 0 Hola Kubernetes
## Temas: Namespaces, resource quotas, primer despliegue, servicio, right-sizing, replicas-autodescubrimiento, escalado horizontal
## Objectivo: Desplegar y acceder a nuestra primera carga de trabajo en Kubernetes
- Arranca minikube ***minikube start***

`$ minikube start`
- Just for fun, juega con kubectl, la línea de comandos de kubernetes

`$ kubectl get nodes`
`$ kubectl describe node minikube`
`$ kubectl get events --all-namespaces`

- Accede al directorio de este reto
`$ cd Retos/0 Hola Kubernetes`

- Crea el namespace para este reto y explóralo
`$ kubectl apply -f namespace.yaml`

`$ kubectl describe namespace 0-hola-kubernetes`

- Cambia el contexto a el nuevo namespace y comprueba. Con ello, a partir de ahora todas las operaciones se harán por defecto sobre el mismo
`$ kubectl config current-context`

`$ kubectl config set-context minikube --namespace=0-hola-kubernetes`

`$ kubectl get pods`

- Define las quotas para este namespace

`$ kubectl apply -f resourcequotas.yaml`

- Despliega tu primera carga de trabajo con ***kubectl apply***

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`



- Como usamos Minikube, puedes exponer y acceder a tu servicio con ***kubectl apply***

`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ kubectl get svc`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

**Nota** - `<Más info>` : <https://minikube.sigs.k8s.io/docs/handbook/accessing/>

