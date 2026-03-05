The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.  
By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  
For this task, allocate an Elastic IP address named `xfusion-eip` using Terraform.  
The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to accomplish this task.  
Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  

Solution:  
```
resource "aws_eip" "xfusion-eip" {
  domain   = "vpc"
  tags = {
        Name        = "xfusion-eip"
      }
}
```

Comandos principales de AWS CLI
Listar todas las VPC: Ejecutá el siguiente comando para obtener los detalles de todas las VPC en tu región configurada:
aws ec2 describe-vpcs
Listar con un formato legible (Tabla): Para ver la información de forma más ordenada en tu terminal:
aws ec2 describe-vpcs --output table
Filtrar por una VPC específica: Si conocés el ID de la VPC:
aws ec2 describe-vpcs --vpc-ids vpc-0e9801d129EXAMPLE

Para listar las Elastic IP (EIP) asociadas a tu cuenta en AWS mediante la CLI, el comando principal es describe-addresses. 
Amazon AWS Documentation
Amazon AWS Documentation
 +1
Comandos principales
Listar todas las Elastic IP: Muestra los detalles de todas las direcciones reservadas en la región actual:
aws ec2 describe-addresses
Formato de tabla: Para una lectura rápida de las IPs y sus asociaciones:
aws ec2 describe-addresses --output table
Filtrar por una IP específica:
aws ec2 describe-addresses --public-ips 1.2.3.4
[reference](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip#tags-1)  
[back](https://github.com/alegescalante/Engineer-Task-KK/tree/main)  
