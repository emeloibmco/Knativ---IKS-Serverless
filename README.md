# Knativ---IKS-Serverless
# GUÍA DE INSTALACIÓN DE KNATIVE Y DESPLIEGUE DE APP SERVERLESS NODEJS EN KUBERNETES

_Para el desarrollo de este proyecto se tiene como base una aplicación hello World basada en el lenguaje de programación NodeJs y su posterior despliegue en un **Cluster de Kubernetes** que se encuentra alojado en **IBM Cloud**._

### Indice
1. [Pre-requisitos](#Pre-requisitos-)
2. [Prepación para instalación de Knative en Kubernetes desde la consola web del cluster](#preparación-para-instalación-de-Knative-en-Kubernetes-desde-la-consola-web-del-cluster-)
3. [Instalación de Knative en el cluster de Kubernetes (IKS)](#instalación-de-Knative-en-el-cluster-de-kubernetes-iks-)
4. [Despliegue en Kubernetes de una aplicación HelloWord de nodejs Serverless](#despliegue-en-Kubernetes-de-una-aplicación-helloWord-de-nodejs-serverless-)
5. [Referencias](#Referencias)

## Pre-requisitos 📋

_1. Tener un Cluster de Kubernetes con minimo 3 Pods._

_2. Tener instalado Docker._

_3. Tener CLI de IBM Cloud._

4.[Instale la CLI de IBM Cloud, el plugin de IBM Cloud Kubernetes Service y la CLI de Kubernetes](https://cloud.ibm.com/docs/containers?topic=containers-cs_cli_install&locale=es#cs_cli_install_steps). Asegúrese de instalar una versión de la CLI de kubectl que coincida con la versión de Kubernetes de su clúster.

5.[Cree un clúster estándar con al menos 3 nodos trabajadores con 4 núcleos y 16 GB de memoria (b3c.4x16) o más cada uno](https://cloud.ibm.com/docs/containers?topic=containers-clusters&locale=es#clusters_ui). Además, el clúster y los nodos trabajadores deben ejecutar al menos la versión mínima soportada de Kubernetes, que puede obtener con el mandato ibmcloud ks addon-versions --addon knative.

6.Asegúrese de tener el [rol de servicio Escritor o Gestor de IBM Cloud IAM](https://cloud.ibm.com/docs/containers?topic=containers-users&locale=es#platform) para IBM Cloud Kubernetes Service.

7.[Defina su clúster como destino de la CLI](https://cloud.ibm.com/docs/containers?topic=containers-cs_cli_install&locale=es#cs_cli_configure).

8.Si ya tiene instalado el complemento Istio gestionado, [compruebe que la versión es 1.4 o desinstale Istio](https://cloud.ibm.com/docs/containers?topic=containers-serverless-apps-knative#knative_limitations). Cuando se instala el complemento Knative gestionado, Istio 1.4 se instala con el complemento.


## Preparación para instalación de Knative en Kubernetes desde la consola web del cluster: 🚀

### Haga 'login' a IBM Cloud desde la línea de comando.

_Inicialmente debe acceder a IBM Cloud desde el siguiente link:_

```
https://cloud.ibm.com/
```
### Acceda al cluster de Kubernetes (IKS) desplegado en IBM Cloud 📦


_1.	Despues de haber iniciado sesión en IBM Cloud, debe dirigirse al clúster en el que se va a trabajar._

_Para ingresar al clúster que tengamos aprovisionado en nuestra cuenta de IBM Cloud se deben realizar los siguientes pasos:_

_•	Diríjase al resource list._
_Primero debe dar clic en el **navigation menu** y luego donde dice **Resource list**, como se puede ver en la siguiente imagen:_

<p align="center">
<img width="696" alt="7" src="https://user-images.githubusercontent.com/60987042/76996077-da434b00-691e-11ea-92be-558da48f7d97.PNG">
</p>

_•	Diríjase a la sección de **clústers** y dar clic en el que se desea acceder._

_•	Se da clic en el botón **Actions...** y luego en la sección que dice **Terminal Web**._

![WhatsApp Image 2020-06-09 at 11 30 23 AM](https://user-images.githubusercontent.com/60628267/84174858-bc304700-aa44-11ea-99d7-02065ad676cc.jpeg)

 
_La terminal que se abre al terminar el paso anterior, es una terminal similar a la que maneja un sistema operativo como Ubuntu._

### Instalación de Knative en el cluster de Kubernetes (IKS) 📦

**NOTA: En este punto de la guía debemos estar en la consola web del cluster**
_Para instalar corectamente Knative en el cluster se deben seguir los siguientes pasos:_ 

_1. Habilite el complemento Knative gestionado en el clúster, esto lo puede hacer mediante el siguiente comado:_

```
ibmcloud ks cluster addon enable knative --cluster <cluster_name_or_ID> -y
```
_La salida de este comado debe ser:_

```
Enabling add-on knative for cluster knative...
OK
```

_2. Verifique que Istio se ha instalado correctamente. Todos los pods correspondientes a los servicios de Istio deben estar en el estado **Running**, y para poder verificarlo debemos utilizar el siguiente comando:_

```
kubectl get pods --namespace istio-system
```
_La salida de este comado debe ser:_

```
NAME                                       READY     STATUS      RESTARTS   AGE
istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
```

_3. **Opcional:** si desea utilizar Istio para todas las demás apps en el espacio de nombres **default**, añada la etiqueta **istio-injection=enabled** al espacio de nombres, si desea realizarlo el comado es el siguiente:_


```
kubectl label namespace default istio-injection=enabled
```
_4. Verifique que todos los pods del componente Serving de Knative están en el estado Running. Verifiquelo mediante el siguiente comando:_

```
kubectl get pods --namespace knative-serving
```
_La salida de este comado debe ser:_

```
NAME                          READY     STATUS    RESTARTS   AGE
activator-598b4b7787-ps7mj    2/2       Running   0          21m
autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
controller-7fc84c6584-qlzk2   1/1       Running   0          21m
webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
```

_En este punto de la guía, Knative ya está instalado en el cluster de Kubernetes (IKS), ahora debemos pasar al despliegue de una aplicación._



### Despliegue en Kubernetes de una aplicación HelloWord de nodejs Serverless: 🚀

**NOTA: Este despliegue se debe hacer de una manera local.**

_Para el despliegue de esta aplicación debemos seguir los siguientes pasos:_

_1. Descargar la Imagen Docker que contiene la aplicación a Desplegar:_

```
docker pull ibmcom/kn-helloworld
```

![WhatsApp Image 2020-06-08 at 4 46 41 PM](https://user-images.githubusercontent.com/60628267/84172330-83db3980-aa41-11ea-96be-bb13a7b007ac.jpeg)


_2. Cargue a su Docker personal esta imagen descargada, los comandos son los siguientes:_

```
docker tag <ID_IMAGE> <Docker_User>/kn-helloworld
```
_Luego debe ejecutar el siguiente:_

```
docker push <Docker_User>/kn-helloworld
```

![WhatsApp Image 2020-06-08 at 4 47 41 PM (1)](https://user-images.githubusercontent.com/60628267/84173324-c18c9200-aa42-11ea-9f93-984c770a43a0.jpeg)
 


_3. Cree un nuevo archivo con el nombre **service.yaml** y añada el siguiente contenido:_

```
nano service.yaml
```
**NOTA: Al ejecutar el editor de texto nano puede añadir el siguiente contenido y para Guardarlo y salir, debe presionar la tecla f2, luego debe aceptar los cambios y dar enter.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: knative-example
  namespace: default
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: docker.io/##DOCKERHUB_NAME##/knative-example
```


![WhatsApp Image 2020-06-08 at 4 49 31 PM (1)](https://user-images.githubusercontent.com/60628267/84173463-ef71d680-aa42-11ea-8a63-efdd8cfd73a8.jpeg)


_4. Una vez realizado el paso anterior se genera el servicio de Knative para esta aplicación, el cual se hace mediante el siguiente comando:_

```
kubectl apply -f service.yaml
```

![WhatsApp Image 2020-06-08 at 4 50 26 PM (1)](https://user-images.githubusercontent.com/60628267/84173533-031d3d00-aa43-11ea-8bb8-aa9546be98ec.jpeg)
 
_5. Mediante el siguiente comando, podemos observar cómo el servicio crea los diferentes recursos y entidades:_

```
kubectl get configuration
```

![WhatsApp Image 2020-06-08 at 4 50 55 PM (1)](https://user-images.githubusercontent.com/60628267/84173559-0e706880-aa43-11ea-9b07-519e584d5be8.jpeg)


_6. Ahora podemos verificar que efectivamente el sercicio este desplegado:_

```
kubectl get replicaset
```

![WhatsApp Image 2020-06-08 at 4 51 39 PM (1)](https://user-images.githubusercontent.com/60628267/84173584-19c39400-aa43-11ea-976f-81611f6bcfc6.jpeg)



_7. Ahora vamos a visualizar la ruta de despliegue de nuestra aplicación:_

```
kubectl get route
```

![WhatsApp Image 2020-06-08 at 4 52 09 PM (1)](https://user-images.githubusercontent.com/60628267/84173618-25af5600-aa43-11ea-83e6-c73cc2fce67c.jpeg)


**Nota: Esta ruta es la que debemos llevar a un navegador y ahi podremos ver la aplicación funcionando y ya desplegada**

_8. En este paso podremos ver en que pod fue asignado el despliegue al igual que podremos visualizar todas las aplicaciones que esten corriendo en nuestro Cluster:_

```
kubectl get pods
```
![WhatsApp Image 2020-06-08 at 4 52 57 PM (1)](https://user-images.githubusercontent.com/60628267/84173686-3f509d80-aa43-11ea-8806-2e71e5da7588.jpeg)


_9. Utilizando el link de despliegue en el navegador se podrá visualizar la aplicación funcionando._

![WhatsApp Image 2020-06-08 at 4 44 00 PM (1)](https://user-images.githubusercontent.com/60628267/84173739-50011380-aa43-11ea-8ec1-1c8bf6478003.jpeg)


## Referencias

La documentación en linea de Knative en IBM Cloud, se encuentra en el siguiente enlace:

https://cloud.ibm.com/docs/containers?topic=containers-serverless-apps-knative&locale=es#update-knative-addon

La documentación necesaria para una aplicación Serverless la puede encontrar en el siguiente enlace:

https://serverless-architecture.io/blog/serverless-workloads-on-kubernetes-with-knative/


## Autores ✒️

_Equipo IBM Cloud Tech sales Colombia._
Javier Jiménez
Jhoiner Smith
Felipe Martinez
Jessica Sepulveda

