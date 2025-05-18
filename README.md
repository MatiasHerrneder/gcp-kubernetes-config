# IMPORTANTE, no dejar el cluster prendido innecesariamente
```
terraform destroy
```
##

## Uso de configuracion:

### Descargar gpc CLI
* https://cloud.google.com/sdk/docs/install

### Logearse con gcp e instalar plugins
```
gcloud auth login
gcloud components install gke-gcloud-auth-plugin
```

### Crear una cuenta de servicio para kubernetes
* Crear una cuenta de servicio (con permisos o roles de: Kubernetes Engine Admin, Viewer), descargar la clave y ponerla en la carpeta credentials con el nombre auth.json

## Imagenes cargadas en Docker Hub:
```
docker tag matiasherrneder/mipla-backend:latest
docker push matiasherrneder/mipla-backend:latest
docker tag TU_IMAGEN matiasherrneder/mipla-frontend:latest
docker push matiasherrneder/mipla-frontend:latest
```

## Comandos para levantar el cluster:

### Inicia terraform
```
terraform init
```

### Muestra los cambios a incorporar con apply (o errores si hay)
```
terraform plan
```

### Aplicar la configuracion del cluster
```
terraform apply
```

### Conectarse al cluster creado
```
gcloud container clusters get-credentials $(terraform output -raw cluster_name) --region $(terraform output -raw zone) --project $(terraform output -raw project_id)
```

### Verificar conexion
```
kubectl get nodes
```

### Despliegue de los manifiestos
En el directorio /manifests
```
kubectl apply -f . --recursive
```

### Verificar pods
```
kubectl get pods -n myapp
```

### Conseguir IP del frontend para conectarse
```
kubectl get svc frontend -n myapp
```
Obtenemos algo como:

| NAME      | TYPE          | CLUSTER-IP   | EXTERNAL-IP    | PORT(S)       | AGE
|-----------|---------------|--------------|----------------|---------------|-----
| frontend  | LoadBalancer  | 10.52.9.9    | 34.73.100.123  | 80:31709/TCP  | 84s

> La ip sería: 34.73.100.123