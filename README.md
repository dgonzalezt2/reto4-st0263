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

Este repositorio presenta la implementación del despliegue de una aplicación basada en WordPress en un entorno Docker, dentro de un clúster de alta disponibilidad utilizando Kubernetes en Google Cloud Platform (GCP). Partiendo del desafío previo del curso [Reto3](https://github.com/dgonzalezt2/reto3-st0263), donde se exploraron diversas estrategias de despliegue, este proyecto se concentra en aprovechar las capacidades de Kubernetes para garantizar la escalabilidad, disponibilidad y robustez del sistema. Con un enfoque en la centralización de la aplicación, se emplea Nginx como equilibrador de carga para dirigir el tráfico entre dos instancias de WordPress, respaldadas por instancias separadas para la base de datos MySQL y el almacenamiento de archivos a través de NFS, alojadas en GCP. La inclusión de un host de bastión facilita el acceso a las instancias privadas, fortaleciendo la seguridad y la capacidad de respuesta ante la creciente demanda de usuarios. Este proyecto demuestra una sólida comprensión y habilidades en el despliegue de aplicaciones en entornos de alta disponibilidad con Kubernetes en GCP.

### 1.1. Que aspectos cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

Aspectos cumplidos de la actividad propuesta por el profesor:

* Implementación en GCP.
* Despliegue de un clúster Kubernetes con un servicio administrado en la nube.
* Garantizar alta disponibilidad en todas las capas de la aplicación, incluyendo la capa de aplicación, base de datos y almacenamiento.
* Opción de desplegar la base de datos dentro del clúster Kubernetes.
* Opción de desplegar el servidor NFS en el clúster.
* Implementación de un dominio para el servicio con soporte HTTPS.

### 1.2. Que aspectos NO cumplió o desarrolló de la actividad propuesta por el profesor (requerimientos funcionales y no funcionales)

* N/A

## 2. Información general de diseño de alto nivel, arquitectura, patrones, mejores prácticas utilizadas.
Se utilizó la arquitectura presentada a continuación:

Arquitectura [Reto3](https://github.com/dgonzalezt2/reto3-st0263):

* ![image](https://github.com/dgonzalezt2/reto4-st0263/assets/81880494/a8af5c96-dfba-4f82-bcc6-078d4a51ad31)


## 3. Descripción del ambiente de desarrollo y técnico: lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

### deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
spec:
  replicas: 2
  selector:
    matchLabels:
      app: wordpress
  template:
    metadata:
      labels:
        app: wordpress
    spec:
      containers:
      - name: wordpress
        image: wordpress:latest
        ports:
        - containerPort: 80
        env:
        - name: WORDPRESS_DB_NAME
          value:  wordpress
        - name: WORDPRESS_DB_HOST
          value:  10.17.96.3:3306
        - name: WORDPRESS_DB_USER
          value: root
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-password-secret
              key: password
        resources:
          requests:
            memory: "256Mi"
            cpu: "256m"
          limits:
            memory: "512Mi"
            cpu: "512m"
        volumeMounts:
        - name: wordpress-nfs
          mountPath: /var/www/html
      volumes:
      - name: wordpress-nfs
        persistentVolumeClaim:
          claimName: wordpress-nfs
```

### mysql-secret.yaml
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-password-secret
type: Opaque
data:
  password: d3BwYXNzd29yZA==
```

### nfs.yaml

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: wordpress-nfs-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    path: /nfs_telematica
    server: 10.17.97.10

---

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wordpress-nfs
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: ""
  volumeName: wordpress-nfs-pv
```

### managed-cert.yaml
```yaml
apiVersion: networking.gke.io/v1
kind: ManagedCertificate
metadata:
  name: managed-cert
spec:
  domains:
    - reto3.me
    - reto4.reto3.me
```

### nodeport.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress-nodeport
spec:
  type: NodePort
  selector:
    app: wordpress
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### ingress.yaml

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: managed-cert-ingress
  annotations:
    kubernetes.io/ingress.global-static-ip-name: reto
    networking.gke.io/managed-certificates: managed-cert
    ingressClassName: "gce"
spec:
  defaultBackend:
    service:
      name: wordpress-nodeport
      port:
        number: 80
```

## 4. Descripción del ambiente de EJECUCIÓN (en producción) lenguaje de programación, librerias, paquetes, etc, con sus numeros de versiones.

# OpenLens
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/d081cdff-013a-4155-ac0c-c8c73d11ad65)
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/f3d387a8-c39c-498e-8c67-60588803ed8d)

# GCP
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/2ac97f7a-ce5a-41d4-8a54-9f0429be97ba)

# DB GCP 
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/78f8df95-117e-4c58-bb74-2da37aaf93dc)

# Instancia GCP
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/10e3d1d6-eff8-4dc4-8ee2-df64cdd8c193)

# IPS
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/1bf323c4-7829-4ee0-bd5d-cb7abe69fdf3)

# Certificado SSL
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/81880494/dfd09b3c-bdcd-414f-815d-efc62e446425)
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/81880494/7cd6996f-f54d-4551-bd6f-f0b1a2c168f1)
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/8d6f70a7-a1ea-4251-b1ee-16ee9c36ea82)


## 5. Resultado final.

[Video sustentacion](https://youtu.be/VVQD8q8idtg)

[DominioReto4](https://reto4.reto3.me/)

![image](https://github.com/dgonzalezt2/reto4-st0263/assets/82610906/e53713a7-75cc-42b2-b7f2-e1e2e4178aa5)
![image](https://github.com/dgonzalezt2/reto4-st0263/assets/81880494/edb7c184-6d26-4c89-a771-261d7f76a175)

## 6. Referencias.

* [Usar certificados SSL administrados por Google](https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs#gcloud)
* [KUBERNETES De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://www.youtube.com/watch?v=DCoBcpOA7W4)
* [Wordpress High Availability on Kubernetes](https://medium.com/@icheko/wordpress-high-availability-on-kubernetes-f6c0bcc2f28d)
* [WordPress on Kubernetes Cluster — Step-by-Step Guide](https://engr-syedusmanahmad.medium.com/wordpress-on-kubernetes-cluster-step-by-step-guide-749cb53e27c7)
* [HIGHLY AVAILABLE WORDPRESS ON KUBERNETES](https://matthewdavis.io/highly-available-wordpress-on-kubernetes/)
* [How to deploy WordPress on Kubernetes — Part 1](https://medium.com/codex/how-to-deploy-wordpress-on-kubernetes-part-1-62cc5bd74410)
* [How to deploy WordPress on Kubernetes — Part 2](https://medium.com/codex/how-to-deploy-wordpress-on-kubernetes-part-2-df1cc9cbaa2e)



