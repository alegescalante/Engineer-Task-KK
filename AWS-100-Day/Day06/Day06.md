# Day 6: Launch EC2 Instance

For this task, create an EC2 instance with following requirements:

1) The name of the instance must be `devops-ec2`.

2) You can use the Amazon Linux AMI to launch this instance.

3) The Instance type must be `t2.micro`.

4) Create a new RSA key pair named `devops-kp`.

5) Attach the default (available by default) security group.


To launch an instance

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
    <img width="1366" height="615" alt="image" src="https://github.com/user-attachments/assets/147bdf3b-6024-4192-b81b-41a34df5696a" />

2. From the EC2 console dashboard, in the **Launch instance** pane, choose **Launch instance**.
   <img width="338" height="253" alt="image" src="https://github.com/user-attachments/assets/d9748fbc-45c5-473f-a5af-a75b068cf697" />

3. Under Name and tags, for Name, enter a descriptive name for your instance. The name of the instance must be `devops-ec2`.
   <img width="859" height="130" alt="image" src="https://github.com/user-attachments/assets/901546e3-5d6a-4099-af76-e215dfb0a882" />
   
5. Under Application and OS Images (Amazon Machine Image), choose the Amazon Linux AMI to launch this instance.
   <img width="859" height="475" alt="image" src="https://github.com/user-attachments/assets/66290062-4d4c-48f7-b71a-04b594e1b824" />

5. Under Instance type, select `t2.micro`.
   <img width="859" height="245" alt="image" src="https://github.com/user-attachments/assets/133d9321-f230-453c-87cd-6a5d07e11d25" />

6. Under Key pair (login), for Key pair name, choose an existing key pair or choose Create new key pair to create your first key pair. **For this task, create a new RSA key pair named `devops-kp`.**
   <img width="859" height="189" alt="image" src="https://github.com/user-attachments/assets/13aeca86-e46e-4147-828b-fb3e33a0a688" />

7. Under Network settings, notice that we selected your default VPC, selected the option to use the default subnet in an Availability Zone that we choose for you, and configured a security group with a rule that allows connections to your instance from anywhere (0.0.0.0.0/0). **For this task, Attach the default (available by default) security group.**
   <img width="862" height="447" alt="image" src="https://github.com/user-attachments/assets/86a53719-7636-45d8-a64f-399e37be5168" />

8. Review a summary of your instance configuration in the Summary panel, and when you're ready, choose Launch instance.
   <img width="417" height="454" alt="image" src="https://github.com/user-attachments/assets/118dafda-3c41-4210-9189-05e1178ac1d1" />

9. If the launch is successful, choose the ID of the instance from the Success notification to open the Instances page and monitor the status of the launch.
    <img width="1132" height="137" alt="image" src="https://github.com/user-attachments/assets/8d45a06b-d153-4396-a5c0-a8b9e5ee062a" />


----

# AWS Cli

Para crear una instancia EC2 con una nueva clave utilizando la AWS CLI, debes seguir un proceso de dos pasos: primero generar el par de claves y luego lanzar la instancia referenciando ese nombre. 

# Paso 1: Crear y guardar la nueva Key Pair 
Es fundamental guardar el contenido de la clave en un archivo local para poder conectarte después por SSH. 
```bash
aws ec2 create-key-pair \
    --key-name MiNuevaClave \
    --query 'KeyMaterial' \
    --output text > MiNuevaClave.pem
```
Detalles importantes de los parámetros:
* --key-name: El nombre único que identificará a la clave en tu consola de AWS.
* --query 'KeyMaterial': Filtra la respuesta para obtener únicamente el contenido de la clave privada.
* --output text: Asegura que el formato sea texto plano para que el archivo .pem sea válido.
* > MiClaveNueva.pem: Redirige la clave directamente a un archivo en tu computadora. 

Configuración de seguridad (Linux/Mac)
Para que SSH te permita usar esta clave, debes restringir sus permisos, de lo contrario la conexión será rechazada por ser "demasiado abierta":
```bash
chmod 400 MiClaveNueva.pem
```

Nota para usuarios de Windows (PowerShell):
Si usas PowerShell, el redireccionamiento estándar > puede generar problemas de codificación. Utiliza este comando para asegurar la compatibilidad con clientes SSH:
```bash
aws ec2 create-key-pair --key-name MiClaveNueva --query 'KeyMaterial' --output text | out-file -encoding ascii -filepath MiClaveNueva.pem
```

# Paso 2: Lanzar la instancia EC2 
Utiliza el comando run-instances incluyendo el parámetro --key-name con el nombre exacto que usaste en el paso anterior. 

````bash
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --count 1 \
    --instance-type t2.micro \
    --key-name MiNuevaClave \
    --security-group-ids sg-0123456789abcdef0 \
    --subnet-id subnet-6e7f829e
```

Requisitos previos para el comando:
* AMI ID: El identificador de la imagen (ej: Amazon Linux 2) disponible en tu región.
* Security Group: Debe tener el puerto 22 (SSH) abierto para que puedas usar tu nueva clave para entrar.
* Subnet ID: El ID de la subred dentro de tu VPC donde se alojará la máquina. 

Para verificar que la instancia se ha creado correctamente, puedes usar el comando aws ec2 describe-instances filtrando por el nombre de tu clave. 

# Listar Key Pair

Para listar los pares de claves (Key Pairs) en tu cuenta de AWS mediante la CLI, debes utilizar el comando describe-key-pairs. 

Comando básico
Este comando te devolverá una lista con el nombre, el ID y la huella digital (fingerprint) de todas las claves en la región configurada: 

```bash
aws ec2 describe-key-pairs
```
Opciones útiles de filtrado
Si tienes muchas claves, puedes refinar la salida con estos parámetros:
* Listar solo los nombres: Ideal para una lectura rápida.
```bash
aws ec2 describe-key-pairs --query 'KeyPairs[*].KeyName' --output text
```

* Buscar una clave específica: Si quieres verificar si una clave existe.
```bash
aws ec2 describe-key-pairs --key-names "MiNombreDeClave"
```

* Formato de tabla: Para una visualización más organizada en la terminal.
```bash
aws ec2 describe-key-pairs --output table
````

Datos que obtendrás
La respuesta (en formato JSON por defecto) incluye detalles como el KeyPairId, el KeyType (RSA o ED25519) y la fecha de creación (CreateTime). 

Ten en cuenta que las claves son específicas por región. Si no ves una clave que creaste, asegúrate de estar consultando la región correcta usando el parámetro --region (ej. --region us-east-1)

   
# Listar Ec2

Para listar tus instancias EC2 mediante la AWS CLI, el comando principal es describe-instances. Este comando ofrece gran flexibilidad para filtrar y dar formato a los resultados. 

Comandos principales para listar instancias
Listado completo (JSON): Muestra toda la información detallada de todas las instancias en la región actual.
```bash
aws ec2 describe-instances
```

Listado resumido en tabla: Ideal para ver rápidamente el ID, tipo y estado de tus máquinas.
```bash
aws ec2 describe-instances --query 'Reservations[*].Instances[*].{ID:InstanceId,Type:InstanceType,State:State.Name,Name:Tags[?Key==`Name`].Value | [0]}' --output table
```

Listar solo instancias en ejecución: Filtra para ignorar las máquinas apagadas o terminadas.
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
```

Obtener solo los IDs: Útil si necesitas pasar los IDs a otro comando.
```bash
aws ec2 describe-instances --query 'Reservations[*].Instances[*].InstanceId' --output text
```

Filtrado avanzado
Puedes combinar filtros para encontrar instancias específicas por sus atributos: 

* Por etiqueta (Tag): ... --filters "Name=tag:Name,Values=MiServidor".
* Por tipo de instancia: ... --filters "Name=instance-type,Values=t2.micro". 

Notas importantes
1- Regiones: Los resultados son específicos de la región configurada. Para ver instancias en otra región, añade --region nombre-region (ej. --region us-west-2).
2- Paginación: Si tienes muchas instancias, la CLI usará un paginador. Puedes desactivarlo con --no-cli-pager si prefieres ver todo de una vez. 


# Listar Security Groups

Para listar los grupos de seguridad (Security Groups) en AWS mediante la interfaz de línea de comandos (CLI), se utiliza el comando describe-security-groups. 
Amazon AWS Documentation
Amazon AWS Documentation
 +1
Comandos principales para listar Security Groups
Listado completo (JSON): Muestra todos los detalles, incluyendo las reglas de entrada y salida de cada grupo en la región configurada.
bash
aws ec2 describe-security-groups
Usa el código con precaución.

Listado resumido en tabla: Ideal para ver rápidamente el ID, el nombre y la VPC asociada.
bash
aws ec2 describe-security-groups --query 'SecurityGroups[*].{Name:GroupName,ID:GroupId,VPC:VpcId}' --output table
Usa el código con precaución.

Listar por ID específico: Si solo quieres los detalles de uno o varios grupos concretos.
bash
aws ec2 describe-security-groups --group-ids sg-0123456789abcdef0
Usa el código con precaución.

Filtrar por VPC: Útil para ver solo los grupos de seguridad que pertenecen a una red específica.
bash
aws ec2 describe-security-groups --filters "Name=vpc-id,Values=vpc-0123456789abcdef0"
Usa el código con precaución.

 
Amazon AWS Documentation
Amazon AWS Documentation
 +5
Otras consultas útiles
Obtener solo los IDs: Útil para automatización o scripts.
bash
aws ec2 describe-security-groups --query 'SecurityGroups[*].GroupId' --output text
Usa el código con precaución.

Ver reglas específicas: Si necesitas auditar qué grupos permiten acceso por un puerto específico (ej. puerto 22 para SSH).
bash
aws ec2 describe-security-groups --filters "Name=ip-permission.from-port,Values=22" --query 'SecurityGroups[*].GroupName'
Usa el código con precaución.

 
Amazon AWS Documentation
Amazon AWS Documentation
 +3
Recuerda que, al igual que otros recursos de EC2, los grupos de seguridad son específicos de la región; si no aparecen, verifica que estás en la región correcta o añade el parámetro --region

# Listar Subredes
Para listar las subredes (subnets) en tu cuenta de AWS mediante la CLI, el comando principal es describe-subnets. Al igual que con otros recursos, puedes ver la información completa o filtrar los datos para mayor claridad. 
Amazon AWS Documentation
Amazon AWS Documentation
Comandos principales para listar subredes
Listado completo (JSON): Muestra todos los detalles técnicos de cada subred (ID de VPC, bloque CIDR, zona de disponibilidad, etc.).
bash
aws ec2 describe-subnets
Usa el código con precaución.

Listado resumido en tabla: Ideal para identificar rápidamente el ID de la subred, su rango de IPs y a qué VPC pertenece.
bash
aws ec2 describe-subnets --query 'Subnets[*].{ID:SubnetId,CIDR:CidrBlock,VPC:VpcId,AZ:AvailabilityZone}' --output table
Usa el código con precaución.

Filtrar por una VPC específica: Si solo te interesan las subredes de una red en particular.
bash
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-0123456789abcdef0"
Usa el código con precaución.

Listar por nombre (Tag Name): Útil si has etiquetado tus subredes (ej. "Public-Subnet").
bash
aws ec2 describe-subnets --filters "Name=tag:Name,Values=MiSubred"
Usa el código con precaución.

 

Datos clave que verás en los resultados
1. SubnetId: El identificador necesario para lanzar instancias EC2.
2. CidrBlock: El rango de direcciones IP asignado a esa subred.
3. AvailableIpAddressCount: Cuántas direcciones IP quedan libres para nuevos recursos.
4. MapPublicIpOnLaunch: Indica si las instancias lanzadas aquí reciben automáticamente una IP pública


   
