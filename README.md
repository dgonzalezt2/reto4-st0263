# ST0263 - Tópicos Especiales de Telemática

## Integrantes:
- David González Tamayo (dgonzalez2@eafit.edu.co)
- Juan Esteban Castro García (jecastrog@eafit.edu.co)
- Tomás Bernal Zuluaga (tbernalz@eafit.edu.co)

## Profesor
- **Nombre:** Edwin Nelson Montoya Munera
- **Correo:** emontoya@eafit.edu.co

# Reto 4 (Kubernetes)

## 1. Breve descripción de la actividad

Este repositorio presenta la implementación del despliegue de una aplicación basada en WordPress en un entorno Docker, dentro de un clúster de alta disponibilidad utilizando Kubernetes. Partiendo del desafío previo del curso, donde se exploraron diversas estrategias de despliegue, este proyecto se concentra en aprovechar las capacidades de Kubernetes para garantizar la escalabilidad, disponibilidad y robustez del sistema. Con un enfoque en la centralización de la aplicación, se emplea Nginx como equilibrador de carga para dirigir el tráfico entre dos instancias de WordPress, respaldadas por instancias separadas para la base de datos MySQL y el almacenamiento de archivos a través de NFS, alojadas en AWS con cinco máquinas virtuales. La inclusión de un host de bastión facilita el acceso a las instancias privadas, fortaleciendo la seguridad y la capacidad de respuesta ante la creciente demanda de usuarios. Este proyecto demuestra una sólida comprensión y habilidades en el despliegue de aplicaciones en entornos de alta disponibilidad con Kubernetes.

### 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

Aspectos cumplidos de la actividad propuesta por el profesor:

* Implementación en GCP o AWS, con cada estudiante recibiendo una invitación para unirse a la nube GCP.
* Despliegue de un clúster Kubernetes con un servicio administrado en la nube.
* Garantizar alta disponibilidad en todas las capas de la aplicación, incluyendo la capa de aplicación, base de datos y almacenamiento.
* Opción de desplegar la base de datos dentro del clúster Kubernetes o utilizar un servicio administrado de base de datos en la nube.
* Opción de desplegar el servidor NFS en el clúster o utilizar un servicio administrado de NFS en la nube.
* Implementación de un dominio para el servicio con soporte HTTPS.

### 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

* N/A

## 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.
Se utilizó la arquitectura presentada a continuación:

## 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

## 5. Resultado final.

## 6. Referencias.
