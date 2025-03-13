# Proyecto de Infraestructura AKS con Prometheus y Grafana

Este proyecto utiliza Terraform para desplegar un clúster de Azure Kubernetes Service (AKS) con Stack de monitoreo Prometheus-Grafana.

## Componentes de la infraestructura

- **Grupo de recursos de Azure**: `MiGrupoRecursos` en la región `East US`
- **Clúster AKS**: `MiClusterAKS` con 2 nodos de tipo `Standard_DS2_v2`
- **Sistema de monitoreo**: Prometheus y Grafana instalados mediante Helm Chart

## Diagrama de arquitectura


![imagen](https://github.com/user-attachments/assets/907f9f3f-2fe4-481d-b1e9-6c9296577dc2)


## Requisitos previos

- Terraform instalado (versión recomendada: >= 1.0.0)
- CLI de Azure instalado y configurado
- kubectl instalado
- Helm instalado

## Estructura del proyecto

```
.
├── README.md
├── main.tf                   # Definición principal del clúster AKS
├── variables.tf              # Variables para personalizar el despliegue
├── outputs.tf                # Valores de salida después del despliegue
├── prometheus-grafana.tf     # Configuración de Prometheus y Grafana
└── .gitignore                # Archivos a ignorar en Git
```

## Variables disponibles

| Variable | Descripción | Valor por defecto |
|----------|-------------|------------------|
| location | Región de Azure donde se despliegan los recursos | East US |
| node_count | Número de nodos en el clúster AKS | 2 |
| vm_size | Tamaño de las máquinas virtuales para los nodos | Standard_DS2_v2 |

## Outputs

| Output | Descripción |
|--------|-------------|
| aks_name | Nombre del clúster AKS creado |
| resource_group | Nombre del grupo de recursos |
| grafana_admin_password | Comando para obtener la contraseña de admin de Grafana |
| kube_config | Configuración de kubectl para acceder al clúster (sensible) |

## Cómo usar este proyecto

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/tu-repositorio.git
cd tu-repositorio
```

### 2. Inicializar Terraform

```bash
terraform init
```

### 3. Revisar el plan de ejecución

```bash
terraform plan
```

### 4. Aplicar la configuración

```bash
terraform apply
```

### 5. Acceder al clúster AKS

Una vez desplegada la infraestructura, puedes configurar kubectl para acceder al clúster:

```bash
az aks get-credentials --resource-group MiGrupoRecursos --name MiClusterAKS
```

### 6. Acceder a Grafana

Para obtener la contraseña de admin de Grafana:

```bash
kubectl get secret --namespace monitoring prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```

Para acceder a la interfaz web de Grafana:

```bash
kubectl port-forward -n monitoring svc/prometheus-grafana 3000:80
```

Luego abre `http://localhost:3000` en tu navegador y usa:
- Usuario: admin
- Contraseña: [la obtenida del comando anterior]

## Limpieza de recursos

Para eliminar todos los recursos creados:

```bash
terraform destroy
```

## Notas adicionales

- Este proyecto utiliza la identidad de tipo System Assigned para el clúster AKS
- Prometheus y Grafana se instalan en el namespace `monitoring`
