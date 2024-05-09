# 0 Hola Kubernetes
## Temas: Namespaces, resource quotas, primer despliegue, servicio, right-sizing, replicas-autodescubrimiento, escalado horizontal
## Objectivo: Desplegar y acceder a nuestra primera carga de trabajo en Kubernetes
### Arranca y juega
- Arranca minikube ***minikube start***

`$ minikube start`
- Just for fun, juega con kubectl, la línea de comandos de kubernetes

`$ kubectl get nodes`
`$ kubectl describe node minikube`
`$ kubectl get events --all-namespaces`

### 1. Namespaces & Resouces quotas
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




### 2. Hola Kubernetes!
- Despliega tu primera carga de trabajo con ***kubectl apply***

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`



- Como usamos Minikube, puedes exponer y acceder a tu servicio con ***kubectl apply***

`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

**Nota** - `<Más info>` : <https://minikube.sigs.k8s.io/docs/handbook/accessing/>




### 3. Autodescubrimiento y balanceado
- En el fichero deployment.yaml, actualiza el numero de replicas a 3, esto es   replicas: 3 # numero de pods
- Actualiza el despliegue

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`

- Comprueba que el numero de replicas es 3

`$ kubectl get pods --namespace=0-hola-kubernetes`

NAME                                        READY   STATUS    RESTARTS   AGE
hola-kubernetes-basica-00-5f5958676-4x5pw   1/1     Running   0          14s
hola-kubernetes-basica-00-5f5958676-626c7   1/1     Running   0          14s
hola-kubernetes-basica-00-5f5958676-8twg4   1/1     Running   0          14s

- Cada una de esas replicas tiene una IP interna que se presenta en el front de mi carga de trabajo
- - Como usamos Minikube, puedes exponer y acceder a tu servicio con ***kubectl apply***

`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

**Nota** - `<Más info>` : <https://minikube.sigs.k8s.io/docs/handbook/accessing/>
- El comando anterior te devolvera un host:port
- Abrelo en multiples tabs o navegadores, incluyendo la ventana de incógnito
- Verás como la ip del servidor toma 3 valores distintos, siendo 3 el número de réplicas dado que Kubernetes está balanceando mis peticiones entre las 3 réplicas disponibles


### 4. Self-healing
- Elimina una de las replicas (pods) manualmente

`$ kubectl get pods`

NAME                                         READY   STATUS    RESTARTS   AGE
hola-kubernetes-basica-00-586bcd79bf-55r79   1/1     Running   0          4m2s
hola-kubernetes-basica-00-586bcd79bf-b48r5   1/1     Running   0          41s
hola-kubernetes-basica-00-586bcd79bf-w4ddf   1/1     Running   0          41s
`$ kubectl delete pod hola-kubernetes-basica-00-586bcd79bf-w4ddf`

- Comprobar que Kubernetes ha creado una réplica nueva para remplazar la que hemos borrado manualmente
- Para mantener el número de réplicas al indicado (3)
`$ kubectl get pods`

NAME                                         READY   STATUS              RESTARTS   AGE
hola-kubernetes-basica-00-586bcd79bf-55r79   1/1     Running             0          7m11s
hola-kubernetes-basica-00-586bcd79bf-9f6w6   0/1     ContainerCreating   0          9s
hola-kubernetes-basica-00-586bcd79bf-b48r5   1/1     Running             0          3m50s


### 5. Escalado
#### 5.1 Vertical
- En el fichero deployment.yaml, edita los recursos de CPU a 2
        resources:
          requests:
            memory: "64Mi"
            cpu: "2"
          limits:
            memory: "128Mi"
            cpu: "2"
- Actualiza el despliegue

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`

- Revisa el estado del despliegue

`$ kubectl describe deployment hola-kubernetes-basica-00`

- En la parte final de "Events" obtendremos un mensaje tal que
16h         Warning   FailedCreate                   replicaset/hola-kubernetes-basica-00-644c7d9689     Error creating: pods "hola-kubernetes-basica-00-644c7d9689-ht9fm" is forbidden: exceeded quota: mem-cpu-demo, requested: limits.cpu=2,requests.cpu=2, used: limits.cpu=0,requests.cpu=0, limited: limits.cpu=500m,requests.cpu=250m

#### 5.2 Horizontal
- Ya lo hemos visto, se trata de aumentar el número de réplicas en el deployment.yaml

#### 5.3 Horizontal Pod Autoscaler (HPA)
- Revisa y despliega el fichero hpa.yaml
`$ kubectl apply -f .\hpa.yaml --namespace=0-hola-kubernetes`

- Investiga el nuevo hpa con ***kubectl get*** y ***describe***
  
`$ kubectl get hpa`
NAME                        REFERENCE                              TARGETS        MINPODS   MAXPODS   REPLICAS   AGE
hola-kubernetes-basica-00   Deployment/hola-kubernetes-basica-00   <unknown>/1%   1         3         0          8s

`$ kubectl describe hpa hola-kubernetes-basica-00`

Name:                     hola-kubernetes-basica-00
Namespace:                0-hola-kubernetes
Labels:                   <none>
Annotations:              autoscaling.alpha.kubernetes.io/conditions:
                            [{"type":"AbleToScale","status":"True","lastTransitionTime":"2024-05-08T17:45:46Z","reason":"SucceededGetScale","message":"the HPA control...
CreationTimestamp:        Wed, 08 May 2024 19:45:31 +0200
Reference:                Deployment/hola-kubernetes-basica-00
Target CPU utilization:   1%
Current CPU utilization:  <unknown>%
Min replicas:             1
Max replicas:             3
Deployment pods:          3 current / 0 desired
Events:
  Type     Reason                        Age   From                       Message
  ----     ------                        ----  ----                       -------
  Warning  FailedGetResourceMetric       14s   horizontal-pod-autoscaler  failed to get cpu utilization: did not receive metrics for targeted pods (pods might be unready)
  Warning  FailedComputeMetricsReplicas  14s   horizontal-pod-autoscaler  invalid metrics (1 invalid out of 1), first error is: failed to get cpu resource metric value: failed to get cpu utilization: did not receive metrics for targeted pods (pods might be unready)

- Fíjate en el "<unknown>" en el atributo "TARGETS" (comando get) y en "failed to get cpu utilization" (comando describe).
- Esto sucede porque para medir el uso de recursos necesitamos desplegar el adon de Metrics Server. Lo desplegamos
  
`$  minikube addons enable metrics-server`

- Esperamos a que esté listo y revisamos con ***kubectl describe hpa*** que el hpa ya recoge las métricas
- 
`$ kubectl describe hpa hola-kubernetes-basica-00`

Normal   SuccessfulRescale             2m2s                 horizontal-pod-autoscaler  New size: 1; reason: All metrics below target

- Aquí vemos que ha bajad las réplicas a 1 porque no ha carga suficiente
  
### 6. Rollout & Rollback
- Para hacerlo mas visual, borra el despliegue anterior

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`

- Edita deployment.yaml para cambiar la imagen a image: marisamartinserrano/hola-kubernetes-blue:blue
- Despliega con la nueva imagen

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`

- Comprueba visualmente que el servicio desplegado es el azul

`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

- El comando anterior te devolvera un host:port
- Copialo en tu navegador para acceder a tu carga de trabajo y comprobar que es azul 

- Comprueba en que version estamos
  
`$ kubectl rollout history deployment/hola-kubernetes-basica-00`

deployment.apps/hola-kubernetes-basica-00
REVISION  CHANGE-CAUSE
1         <none>

- Edita deployment.yaml para cambiar la imagen a image: marisamartinserrano/hola-kubernetes-green:green
- Despliega con la nueva imagen

`$ kubectl apply -f .\deployment.yaml --namespace=0-hola-kubernetes`


- Comprueba visualmente que la carga de trabajo desplegado es el VERDE
  
`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

- El comando anterior te devolvera un host:port
- Copialo en tu navegador para acceder a tu carga de trabajo y comprobar que es VERDE 

- Ahora imagina que tus usuarios odian el VERDE y tienes que volver a la version AZUL
- Comprueba la lista de versiones

`$ kubectl rollout history deployment/hola-kubernetes-basica-00`

deployment.apps/hola-kubernetes-basica-00
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
- Cambia a la deseada

`$ kubectl rollout undo --to-revision=1 deployment/hola-kubernetes-basica-00`

- Comprueba visualmente que se ha vuelto a desplegar a carga de trabajo AZUL

`$ kubectl expose deployment hola-kubernetes-basica-00 --type=NodePort --port=80 --namespace=0-hola-kubernetes`

`$ minikube service hola-kubernetes-basica-00 --url --namespace=0-hola-kubernetes`

- El comando anterior te devolvera un host:port
- Copialo en tu navegador para acceder a tu carga de trabajo y comprobar que vuelve a ser AZUL como les gusta a tus usuarios
