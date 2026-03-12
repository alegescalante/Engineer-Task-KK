# Day 7: Create a Public IP Address for Azure VM

For this task, allocate a `Public IP` address, name it as `datacenter-pip`.

- In the portal, search for and select **`Public IP addresses`**.

  <img width="1138" height="483" alt="image" src="https://github.com/user-attachments/assets/cf23be35-84e3-4ba1-9179-764f413d3995" />

- On the **`Public IP addresses`** page, select **`Create`**.

  <img width="1309" height="501" alt="image" src="https://github.com/user-attachments/assets/de67bed0-24f8-42c4-bf22-2686c2bf45ae" />

- On the **`Basics`** tab of the **`Create public IP address`** screen, enter required values.

  <img width="548" height="520" alt="image" src="https://github.com/user-attachments/assets/7956d7ea-8c4a-402d-9262-007de2c212fe" />
  <img width="594" height="564" alt="image" src="https://github.com/user-attachments/assets/daa76c51-e4a7-443d-b00b-fd266e3b02d0" />

- On the **`Review + create`** tab of the **`Create public IP address`** screen, review all matrix and click **`Create`**.

  <img width="371" height="568" alt="image" src="https://github.com/user-attachments/assets/0b454151-d446-40d3-9f5e-1a81ade2149a" />


# Azure Cli:
Para crear una dirección IP pública en Azure mediante la CLI, se utiliza el comando az network public-ip create. 

Para listar tus grupos de recursos en Azure CLI, el comando principal es az group list. 
Aquí tenés las opciones más comunes según cómo quieras ver la información:
Comandos básicos
Listado simple (JSON):
az group list
Muestra todos los grupos de recursos de la suscripción activa en formato JSON por defecto.
Listado en formato tabla:
az group list --output table
Es la opción más recomendada para una lectura rápida, ya que organiza la información en columnas legibles (Nombre, Ubicación, Estado). 

Filtrado y búsqueda avanzada
Filtrar por nombre específico:
Si buscás un grupo en particular, podés usar:
az group show --name NombreDelGrupo.
Filtrar por etiquetas (Tags):
az group list --tag nombreEtiqueta=valorEtiqueta --output table
Listar solo nombres (usando JMESPath):
az group list --query "[].name" --output tsv 
Otros comandos útiles de gestión
Si necesitás realizar otras acciones comunes, podés consultar la documentación oficial de Microsoft sobre el comando az group: 

Crear un grupo: az group create --name MiGrupo --location eastus.
Eliminar un grupo: az group delete --name NombreDelGrupo.
Verificar existencia: az group exists --name NombreDelGrupo

Comando base
Este es el comando estándar para crear una IP pública con una SKU Estándar (la recomendada actualmente por Microsoft): 
```bash
azurecli
az network public-ip create \
    --resource-group MiGrupoDeRecursos \
    --name MiPublicIP \
    --sku Standard \
    --allocation-method Static
```

Opciones comunes y parámetros
Al configurar tu IP, puedes ajustar los siguientes parámetros según tus necesidades:
* --resource-group: Nombre del grupo de recursos donde se alojará la IP.
* --name: El nombre que le darás al recurso.
* --sku: Define el nivel de la IP. Las opciones son Standard (recomendado) o Basic. Ten en cuenta que la SKU básica está entrando en fase de retiro.
* --allocation-method: Puede ser Static (estática) o Dynamic (dinámica). Las IPs de SKU estándar deben ser estáticas.
* --dns-name: (Opcional) Te permite asignar un nombre de dominio (ej. mi-app.southcentralus.cloudapp.azure.com).
* --zone: Para mayor disponibilidad, puedes especificar zonas de disponibilidad (ej. --zone 1 2 3). 

Cómo verificar la creación
Una vez ejecutado el comando, puedes consultar los detalles de la IP (incluyendo la dirección IP numérica asignada) con el siguiente comando:
azurecli
```bash
az network public-ip show \
    --resource-group MiGrupoDeRecursos \
    --name MiPublicIP \
    --query ipAddress \
    --output tsv
```

Este comando filtrará la salida para mostrarte únicamente la dirección IP

Para listar direcciones IP públicas en Azure CLI, utiliza el comando az network public-ip list. Para una salida limpia que muestre solo el nombre, la IP y el grupo de recursos, ejecuta:
az network public-ip list --query "[].{Name:name, IP:ipAddress, ResourceGroup:resourceGroup}" -o table. 

Comandos útiles:
Listar todas las IPs públicas: az network public-ip list -o table.
Listar por grupo de recursos: az network public-ip list --resource-group MiGrupoRecursos -o table.
Ver detalles de una IP específica: az network public-ip show --name MiIPPublica --resource-group MiGrupoRecursos. 

Para AKS (Kubernetes), puedes usar az vmss list-instance-public-ips
