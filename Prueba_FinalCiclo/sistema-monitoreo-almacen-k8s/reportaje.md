# Reporte de la Prueba de final de Ciclo  
## Sistema de Monitoreo de Almacén 


## 1. Introduccion

Este proyecto implementa un sistema de monitoreo en tiempo real para un almacén automatizado, utilizando una arquitectura desacoplada desplegada sobre un clúster de Kubernetes con comandos de minikube.  
El objetivo principal es demostrar el uso de contenedores, persistencia de datos, seguridad básica y resiliencia ante fallos


## 2. Arquitectura del Sistema

El sistema está compuesto por tres componentes principales como se nos pide:

1. **Capa de Datos (Persistencia)**  
   - Base de datos MongoDB con autenticación (usuario y contraseña) y almacenamiento persistente

2. **Microservicio Productor (Sensor)**  
   - Simula un sensor IoT que genera datos aleatorios

3. **Microservicio Cliente (Visualización)**  
   - Interfaz web en Mongo Express para visualizar los datos almacenados

Es importnate saber que mongoBD no se considera un microservicio, sino la capa de persistencia del sistema


## 3. Tecnologías Seleccionadas

 Componente              Tecnología        Justificación 

orquestador           | Kubernetes       | Permite gestionar contenedores como docker-compose
contenedores           | Docker           | La mejor forma que se de empaquetar 
base de datos          | MongoDB          | Base NoSQL que es para documentos, ideal para datos JSON 
persistencia           | PersistentVolume | Garantiza que los datos no se pierdan al reiniciar Pods 
productor (sensor)     | Bash + mongosh   | Solución simple y efectiva para generar datos , que ya viene con mongo
cliente de visualizacion | Mongo Express | Interfaz web lista


## 4. Justificación de MongoDB

Maneje MongoDB porque
- Utilice datos en formato JSON de forma nativa
- Es adecuada para lecturas y escrituras frecuentes
- Permite autenticación mediante usuario y contraseña
- Se integra fácilmente con Kubernetes y volúmenes persistentes


## 5. Seguridad Implementada

- Se utilizó un **Secret de Kubernetes** para almacenar credenciales
- MongoDB no permite conexiones anónimas
- Las credenciales no están escritas directamente en los Deployments


## 6. Persistencia de Datos

- Se configuró un **PersistentVolumeClaim (PVC)**
- El volumen se monta en `/data/db` que ya viene asi en mongoDB
- Los datos permanecen disponibles aunque el Pod sea eliminado


## 7. Resiliencia y Recuperación ante Fallos

Para demostrar la resiliencia del sistema
1. Se eliminaron manualmente los Pods de MongoDB
2. Kubernetes recreo automáticamente el Pod
3. Los datos almacenados seguian disponibles
4. El cliente se reconectó sin intervención de comandos


## 8. Conclusiones

El sistema cumple con todos los requerimientos planteados
- Arquitectura desacoplada
- Persistencia de datos
- Seguridad implementada
- Recuperación automática ante fallos

