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
