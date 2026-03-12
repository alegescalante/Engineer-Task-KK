# Day 7: Change EC2 Instance Type

During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the Status check is completed (if its still in Initializing state) before making any changes to the instance.

1) Change the instance type from `t2.micro` to `t2.nano` for `devops-ec2` instance.

2) Make sure the ec2 instance `devops-ec2` is in running state after the change.

Steps:

1. Open the Amazon EC2 Dashboard. Under `Resource` section, you can already see an instance is running.
   <img width="719" height="245" alt="image" src="https://github.com/user-attachments/assets/978f647a-ad8f-4cec-9cc0-4f790c0a90e0" />

2. Select the instance and from `Instance state` dropdown, click on `Instance Stop`
   <img width="1132" height="180" alt="image" src="https://github.com/user-attachments/assets/f91be5d3-7c6e-4fc0-a654-2ff93f79fca4" />

3. Confirm the insatnce is `stopped` from instance status column
   <img width="1132" height="144" alt="image" src="https://github.com/user-attachments/assets/c0dc40cc-7a9d-40fc-834b-362abfea8f20" />

4. Click **Actions** → **Instance settings** → **Change instance type**
   <img width="1124" height="480" alt="image" src="https://github.com/user-attachments/assets/863be03c-276b-4268-a3b5-74c1fe785f61" />

5. Choose your new instance type (`t2.micro` → `t2.nano`) and **Change instance type**
   <img width="1348" height="394" alt="image" src="https://github.com/user-attachments/assets/9465c9be-5ac6-4c5a-a7d3-055bcb7d86ca" />

6. Start instance from **Instance state** dropdown and verify instance is running and instance type is `t2.nano`
   <img width="1132" height="145" alt="image" src="https://github.com/user-attachments/assets/f45f6343-acea-408f-9c91-c043f67a7110" />

# Aws cli:
Para listar tus instancias de Amazon EC2 utilizando la AWS CLI, el comando principal que debes usar es describe-instances. 

Comandos principales
Listar todas las instancias:
Muestra toda la información detallada de tus instancias en formato JSON por defecto.
```bash
aws ec2 describe-instances
```

Listar con formato de tabla (más legible):
Ideal si quieres una vista rápida desde la terminal.
```bash
aws ec2 describe-instances --output table
```

Filtrar solo por instancias activas (running):
Útil para ignorar aquellas que están apagadas o terminadas.
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
```

Comandos avanzados (proyecciones)
Si solo necesitas datos específicos (como el ID y el nombre), puedes usar el parámetro --query para limpiar la salida: 

Listar ID de instancia y Estado:
```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].{ID:InstanceId,Status:State.Name}" --output table
```

Obtener la IP pública de tus instancias:
```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].PublicIpAddress" --output text
```

Requisitos previos
Para que estos comandos funcionen, asegúrate de haber configurado tus credenciales previamente con el comando aws configure, donde deberás ingresar tu Access Key ID, Secret Access Key y la región predeterminada.

Para modificar una instancia de Amazon EC2 mediante la AWS CLI, el comando principal es modify-instance-attribute. La mayoría de los cambios estructurales requieren que la instancia esté en estado detenido (stopped). 

1. Cambiar el tipo de instancia (Resize) 
Este es el cambio más común. Para realizarlo, debés seguir estos tres pasos:
Detener la instancia:
```bash
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
```

Modificar el atributo del tipo:
```bash
aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --instance-type "{\"Value\": \"t3.medium\"}"
```

(Nota: Reemplazá t3.medium por el tipo deseado).
Iniciar la instancia:
```bash
aws ec2 start-instances --instance-ids i-1234567890abcdef0
```


2. Modificar etiquetas (Tags)
Para cambiar el nombre o agregar etiquetas de organización, usá create-tags. Esto sobreescribirá el valor si la clave ya existe. 

```bash
aws ec2 create-tags --resources i-1234567890abcdef0 --tags Key=Name,Value=MiNuevaInstancia
```

3. Cambiar opciones de metadatos (IMDSv2)
Si necesitás forzar el uso de tokens para mayor seguridad (IMDSv2), se usa un comando específico que no requiere reiniciar la instancia: 

```bash
aws ec2 modify-instance-metadata-options --instance-id i-1234567890abcdef0 --http-tokens required
```

4. Otros atributos comunes
Podés modificar comportamientos específicos con modify-instance-attribute: 
Habilitar protección contra terminación:
--disable-api-termination "{\"Value\": true}"
Cambiar comportamiento al apagar (Stop o Terminate):
--instance-initiated-shutdown-behavior "{\"Value\": \"stop\"}"
Modificar grupos de seguridad:
```bash
aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --groups sg-903004f8 sg-1a2b3c4d
```

Importante: Al detener e iniciar una instancia, si no tiene una Elastic IP asignada, su dirección IP pública cambiará. 


