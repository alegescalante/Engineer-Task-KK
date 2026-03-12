The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units.  
For this task, create an EC2 instance using Terraform with the following requirements:  
    The name of the instance must be `xfusion-ec2`.
    Use the Amazon Linux `ami-0c101f26f147fa7fd` to launch this instance.
    The Instance type must be `t2.micro`.
    Create a new RSA key named `xfusion-kp`.
    Attach the `default` (available by default) security group.
    The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance.
    Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal. 

Solution:  
```
resource "tls_private_key" "xfusion_kp" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_kp.public_key_openssh
}

data "aws_security_group" "default" {
  name = "default"
}

resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

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


reference:  
  - [RSA key](https://registry.terraform.io/providers/hashicorp/tls/latest/docs/resources/private_key)  
  - [ec2](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance#vpc_security_group_ids-1)    

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks/tree/main)  
