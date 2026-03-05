# Day 6: Create a Subnet in Azure Virtual Network

For this task, create a Virtual Network (VNet) named `datacenter-vnet` and one subnet named `datacenter-subnet` within the VNet in the `East US` region. Make sure the IPv4 address range is `10.0.0.0/16`.

Steps:

1. Under the **Basic** tab, create **`Virtual network`** with existing **resource group** with name **`datacenter-vnet`** in **`EAST US`** region.

   <img width="1248" height="471" alt="image" src="https://github.com/user-attachments/assets/3f138330-83f4-4608-a72f-5c916659b7cf" />

2. Click **Next**, Go to **IP Address** tab and edit the default subnet with name **`datacenter-subnet`** and **Save**

   <img width="585" height="574" alt="image" src="https://github.com/user-attachments/assets/6ece7c3a-3c93-448f-ae4f-624a22813b97" />

3. After renaming the default subnet, verify subnet name and required address space. in this case, it is **`10.0.0.0/16`**

   <img width="1242" height="483" alt="image" src="https://github.com/user-attachments/assets/fe33c6b2-6d68-4125-bdff-1a940928d915" />

4. Select **Review + Create**, **Create**.

   <img width="1238" height="483" alt="image" src="https://github.com/user-attachments/assets/e718d069-4f39-4255-b05e-9fb106f46b28" />

5. Verify deployment.

   <img width="763" height="391" alt="image" src="https://github.com/user-attachments/assets/3af17197-d583-4178-a9e4-d336c0641f2a" />

----
## Azure Cli
Para crear una Virtual Network (VNet) con una subnet y una ubicación específica usando Azure CLI, se utiliza el comando az network vnet create. Este comando permite definir la red y su primera subred en un solo paso. 

Comando Directo
Copia y pega el siguiente comando (ajustando los valores entre < >):
```bash
az network vnet create \
  --name <NombreVNet> \
  --resource-group <NombreGrupoRecursos> \
  --location <Ubicacion> \
  --address-prefix 10.0.0.0/16 \
  --subnet-name <NombreSubnet> \
  --subnet-prefix 10.0.1.0/24
```

Parámetros Clave
* --location: Define la región de Azure donde se desplegará la red (por ejemplo, eastus, westeurope, brazilsouth).
* --address-prefix: El bloque de direcciones IP principal para toda la VNet.
* --subnet-prefix: El rango específico que se asignará a la subred. Debe estar dentro del rango de la VNet. 

Ejemplo Práctico
Si quieres crear una red llamada "RedCorporativa" en la región de EE. UU. Este:
```bash
az network vnet create \
  --name RedCorporativa \
  --resource-group MiGrupoRG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name SubredFrontEnd \
  --subnet-prefix 10.0.1.0/24
```

Notas Importantes
* Grupo de Recursos: Debes tener un grupo de recursos creado previamente con az group create.
* Direcciones Reservadas: Ten en cuenta que Azure reserva 5 direcciones IP en cada subred para uso interno (la primera, la última y tres de gestión).
* Subredes Adicionales: Si necesitas agregar más subredes después de crear la VNet, usa el comando az network vnet subnet create
  
## Listar subred-subnet
Para listar las subredes dentro de una red virtual (VNet) específica en Azure mediante la CLI, el comando principal es az network vnet subnet list. 

Comandos para listar subredes
Listado básico en formato de tabla:
Muestra el nombre de la subred, el prefijo de dirección y el estado de aprovisionamiento de forma legible.
```bash
az network vnet subnet list \
  --resource-group <NombreGrupo> \
  --vnet-name <NombreVNet> \
  --output table
```

Listar solo nombres y rangos de IP:
Si solo necesitás ver qué segmentos de red están ocupados, podés filtrar la salida:
```bash
az network vnet subnet list \
  --resource-group <NombreGrupo> \
  --vnet-name <NombreVNet> \
  --query "[].{SubnetName:name, CIDR:addressPrefix}" \
  --output table
```

Otras consultas de utilidad
* Ver detalles de una subred específica:
   Para obtener toda la configuración (como NSGs o tablas de rutas asociadas), usá:
   az network vnet subnet show -g <NombreGrupo> --vnet-name <NombreVNet> -n <NombreSubred>
* Listar IPs disponibles en la subred:
   Si necesitás saber qué direcciones IP están libres para asignar, podés usar este comando (actualmente en vista previa):
   az network vnet subnet list-available-ips -g <NombreGrupo> --vnet-name <NombreVNet> -n <NombreSubred>
