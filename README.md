# Sistema de Monitoreo de Almacén Automatizado con Kubernetes

---

## Descripción Generalue hice por favor:


Este repositorio contiene la implementación de un sistema de monitoreo en tiempo real para un almacén automatizado, desarrollado como práctica académica para demostrar el uso de Kubernetes en la orquestación de aplicaciones desacopladas.

El sistema simula la generación de datos provenientes de un sensor IoT, los almacena en una base de datos NoSQL y permite su visualización mediante una interfaz web, cumpliendo con requisitos de persistencia, seguridad y resiliencia ante fallos.

---

## Objetivo del Proyecto

El objetivo principal del proyecto es aplicar conceptos fundamentales de Kubernetes, tales como:

- Despliegue de aplicaciones contenerizadas.
- Comunicación entre componentes desacoplados.
- Persistencia de datos mediante volúmenes.
- Uso de Secrets para manejo de credenciales.
- Recuperación automática ante fallos de Pods.

---

## Componentes del Sistema

El sistema está compuesto por los siguientes componentes:

### 1. Capa de Datos (Persistencia)

Se utiliza MongoDB como base de datos NoSQL para almacenar las lecturas generadas por el sensor.  
La base de datos se encuentra protegida mediante autenticación y configurada con un volumen persistente para evitar la pérdida de información ante reinicios o fallos del Pod.

### 2. Microservicio Productor (Sensor)

El microservicio productor simula un sensor IoT que genera datos de forma periódica.  
Cada lectura contiene un identificador de sensor, un valor numérico aleatorio y una marca de tiempo, y se inserta automáticamente en la base de datos.

### 3. Microservicio Cliente (Visualización)

Para la visualización de los datos se utiliza Mongo Express, una herramienta web existente que permite inspeccionar el contenido de la base de datos sin necesidad de desarrollar un frontend propio.

---

## Arquitectura

La arquitectura del sistema sigue un modelo desacoplado, donde cada componente cumple una función específica:

- El sensor productor genera y envía datos.
- La base de datos MongoDB actúa como capa de persistencia.
- El cliente de visualización permite consultar los datos almacenados.

Todos los componentes son gestionados por Kubernetes, el cual se encarga de la creación, supervisión y recuperación automática de los Pods.

---

## Persistencia y Resiliencia

El sistema está diseñado para garantizar la continuidad de la información:

- Los datos se almacenan en un volumen persistente asociado a MongoDB.
- En caso de que el Pod de la base de datos sea eliminado o falle, Kubernetes lo recrea automáticamente.
- Al reiniciarse el Pod, los datos previamente almacenados permanecen disponibles.
- El cliente de visualización se reconecta sin intervención manual.

---

## Seguridad

La seguridad básica del sistema se implementa mediante:

- Uso de Secrets de Kubernetes para almacenar credenciales.
- Eliminación de conexiones anónimas a la base de datos.
- Inyección segura de variables de entorno en los contenedores.

---

## Organización del Repositorio

El repositorio se encuentra organizado de la siguiente manera:

- Archivos YAML: contienen los manifiestos de Kubernetes para cada componente.
- `reportaje.md`: documento técnico con las decisiones de diseño y justificación de tecnologías.
- `README.md`: descripción general del proyecto y su arquitectura.
- Archivo PDF: reporte final con evidencias de funcionamiento y capturas de pantalla.

---
## Cómo Utilizar y Probar el Proyecto

### Requisitos Previos

- Docker Desktop en ejecución  
- Minikube  
- kubectl configurado  

---

### 1. Iniciar el Clúster de Kubernetes

minikube start --driver=docker
kubectl config use-context minikube
Verificar que el clúster esté activo:
kubectl get nodes

## 2. Desplegar el Proyecto
Desde la carpeta raíz del repositorio:
kubectl apply -f.
Verificar que los recursos estén creados correctamente:
kubectl get pods
kubectl get svc
kubectl get pvc

## 3. Verificar el Funcionamiento del Sensor
Comprobar que el sensor genera datos cada 3 segundos:
kubectl logs deploy/sensor-almacen -f
En la consola se deben observar mensajes en formato JSON con los valores generados.

## 4. Acceder al Cliente de Visualización
En un entorno con Minikube:
minikube service servicio-visualizacion
En la interfaz web:
Base de datos: almacen_db
Colección: lecturas_sensor

## 5. Verificar la Persistencia del Volumen
Ejecutar el siguiente comando:
kubectl get pvc
El volumen debe mostrarse en estado Bound.

## 6. Prueba de Resiliencia
Eliminar manualmente el Pod de MongoDB:
kubectl delete pod -l app=bd-almacen
Observar cómo Kubernetes lo recrea automáticamente:
kubectl get pods -w

## Finalmente, verificar que:
El volumen persistente sigue en estado Bound
Los datos continúan visibles en el cliente de visualización

---

## Consideraciones Finales

Este proyecto demuestra una implementación sencilla y funcional de un sistema distribuido utilizando Kubernetes, cumpliendo con los requerimientos planteados y manteniendo una arquitectura clara, escalable y fácil de explicar en un entorno académico.

