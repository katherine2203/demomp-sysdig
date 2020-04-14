# Tech Seed Cloud Monitoring
 En este tutorial podremos crear y configurar un cluster en IBM Kubernetes Service, y revisar las métricas con el servicio de creado de Cloud Monitoring with Sysdig

## Uso de Cluster en IBM Cloud

### Creando un cluster en IKS
 Creando un cluster clasico
 ```shell
 ibmcloud ks cluster create classic --name tech-seed-cluster
```

 Revisando los clusters existentes en nuestra cuenta (Este comando debe repetirse hasta que el state cambie a **normal**)
```shell
 ibmcloud ks cluster ls
```

 Revisando los worker nodes de nuestro cluster
 ```shell
 ibmcloud ks worker ls --cluster tech-seed-cluster
 ```
 
### Configurando el cluster creado
 Viendo los clusters creados
```shell
 ibmcloud ks clusters    
```
 Configurar el cluster para utilizar kubetcl desde nuestro terminal
```shell
 ibmcloud ks cluster config --cluster tech-seed-cluster
 ``````
 Ver nuestros nodos
 ```shell
 kubectl get nodes
 ```
 Aplicar la configuración del deployment
 ```shell
 kubectl apply --namespace prod -f ./tmp.deployment.yml
 ```

### Creando Cloud Monitoring
 
1. Ingresamos a la web de IBMCloud: https://cloud.ibm.com
2. Nos dirigimos a catalogo, y luego a Herramientas de Desarrollo
3. Seleccionamos el servicio: *Supervisión de IBM Cloud con Sysdig*
4. Seleccionamos una región para este caso en particular usaremos *Dallas*
5. Seleccionamos un plan de precios, para casos demostrativos utilizaremos *Prueba*
6. En configurar su recurso, colocamos un nombre de servicio: *supervision-tech-seed*
7. Seleccionamos un grupo de recursos: *Default*
8. Colocamos una etiqueta (opcional): *tech-seed*
9. IBM Platform metrics (Seleccionamos si deseamos tener todas la métricas de todos los servicios instanciados en la región)
10. En la terminal vemos el estado del recurso creado para lo cual utilizamos:
```shell
 ibmcloud resource service-instance supervision-tech-seed
```
Verificamos que el state esté marcado como: **active**

### Configurando el Cloud Monitoring with Sysdig

1. Ingresamos al servicio y colocamos **view sysdig**
2. Nos aparece un wizard con una pantalla de bienvenida, luego colocamos *Next*
3. Escogemos el método de instalación: *Kubernetes|GKE|OpenShift*
4. Copiamos el access key
5. Seleccionamos *Open instructions*
6. Instalamos el agente de sysdig: Seleccionamos la opción con Public Endpoint
```shell
curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a  *aca_colocamos_el_access_key_que_se_nos_dio*  -c ingest.us-south.monitoring.cloud.ibm.com -ac 'sysdig_capture_enabled: false'
```
7. Una vez instalado el sysdig en el cluster aparecerá un aviso **You have 1 agent connected! GO TO NEXT STEP!** click en *GO TO NEXT STEP*
8. Listo está terminada la configuración preliminar del Cloud Monitoring with Sysdig, para terminar colocamos *LET'S GET STARTED*

### Habilitando la entrega continua en nuestro cluster de Kubernetes

1. Ingresamos nuevamente a nuestro dashboard (https://cloud.ibm.com)
2. En nuestro resumen de recursos seleccionamos *Clústeres* e ingresamos a **tech-seed-cluster**
3. En la izquierda seleccionamos **DevOps (Nuevo)**
4. Le damos click a *Crear cadena de herramientas*
5. Nos salen los distintos tipos de herramientas, colocamos *Desarrollar una app de Kubernetes*
6. Colocamos el nombre de nuestra cadena de herramientas: *tech-seed-toolchain*
7. Seleccionamos una region: *Dallas*
8. Seleccionamos un grupo de recursos: *Default*
9. En la URL del repositorio de origen: https://github.ibm.com/harry-bazalar/tech-seed-cloud-monitoring

