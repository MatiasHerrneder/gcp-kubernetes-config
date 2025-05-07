# IMPORTANTE, siempre correr al finalizar
>terraform destroy
##
#### TODO:
* Pasar a secrets las variables de entorno

## Uso de configuracion:

### Descargar gpc CLI
> https://cloud.google.com/sdk/docs/install

### Logearse con gcp e instalar plugins
```
gcloud auth login
gcloud components install gke-gcloud-auth-plugin
```

### Crear una cuenta de servicio para kubernetes
* Crear una cuenta de servicio (con permisos o roles de: Kubernetes Engine Admin, Viewer), descargar la clave y ponerla en la carpeta credentials con el nombre auth.json

##
### Cambios de posibles de configuracion:
* Modificar el archivo variables.tf para especificar los datos del proyecto

* Modificar variables de entorno en backend-env.yaml

* Modificar el path a la imagen en backend.yaml y frontend.yaml

## Para cargar en docker hub las imagenes:
```
docker tag TU_IMAGEN TU_USUARIO_DOCKER/mipla-backend:latest
docker push TU_USUARIO_DOCKER/mipla-backend:latest
docker tag TU_IMAGEN TU_USUARIO_DOCKER/mipla-frontend:latest
docker push TU_USUARIO_DOCKER/mipla-frontend:latest
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
gcloud container clusters get-credentials $(terraform output -raw cluster_name) --region $(terraform output -raw region) --project $(terraform output -raw project_id)
```

### Verificar conexion
```
kubectl get nodes
```

### Despliegue de las aplicaciones
primero ejecutar el de las variables de entorno
```
kubectl apply -f backend-env.yaml
kubectl apply -f frontend.yaml
kubectl apply -f backend.yaml
kubectl apply -f database.yaml
```

## Otros comandos
```
gcloud container clusters list --project=tu-proyecto-id
kubectl get pods
kubectl get services
```