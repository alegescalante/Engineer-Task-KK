En KodeKloud Engineer, la tarea de Group Creation and User Assignment (Creación de Grupos y Asignación de Usuarios) es un ejercicio práctico de administración de Linux diseñado para que los administradores de sistemas practiquen la gestión de accesos en servidores remotos. 

# Resumen de la Tarea
El objetivo principal es crear un grupo específico y asignar o crear un usuario dentro de ese grupo en múltiples servidores de aplicaciones (App Servers) dentro del centro de datos virtual (Stratos Datacenter). 

# Pasos para Completar la Tarea
Conexión a los Servidores: Debes acceder mediante SSH a cada uno de los servidores de aplicaciones indicados (comúnmente App Server 1, 2 y 3) utilizando las credenciales proporcionadas (ej. tony, steve, banner).
Escalada de Privilegios: Una vez dentro, cambia al usuario root o utiliza sudo para tener los permisos necesarios para modificar el sistema.
Creación del Grupo: Utiliza el comando groupadd seguido del nombre del grupo especificado en tu tarea (ej. nautilus_developers o nautilus_noc).
```bash
sudo groupadd <nombre_del_grupo>
```
listar grupos
```bash
cat /etc/group
```
Listar solo los nombres de los grupos:
bash
cut -d: -f1 /etc/group
Listar grupos del usuario actual:
bash
groups
Listar grupos de un usuario específico:
bash
groups nombre_usuario
Listar todos los grupos de forma ordenada:
bash
getent group
Ver los grupos y su ID (incluyendo grupos primarios y secundarios) del usuario actual:
bash
id -nG
 
Tecnocratica
Tecnocratica
 +4
Detalles de la información
Al listar, los grupos suelen mostrarse con este formato:
Nombre del grupo: Ejemplo: sudo.
Contraseña: Generalmente x (indicando que la contraseña real está en /etc/gshadow).
GID (Group ID): El número de identificación único.
Miembros: Una lista de usuarios separados por comas que pertenecen al grupo. 

Creación o Verificación del Usuario: Si el usuario solicitado no existe, créalo con useradd. Si ya existe, puedes saltar este paso.
```bash
sudo useradd <nombre_del_usuario>
```

Asignación al Grupo: Añade el usuario al grupo recién creado como un grupo secundario (suplementario) usando usermod.
```bash
sudo usermod -a -G <nombre_del_grupo> <nombre_del_usuario>
```

Nota: La opción -a es crucial para "añadir" (append) y no reemplazar los grupos existentes del usuario.
Validación: Verifica que el usuario pertenezca al grupo correcto antes de finalizar.
```bash
groups <nombre_del_usuario>
# O también:
id <nombre_del_usuario>
```


Consejos Clave
No reinicies los servidores: Muchos estudiantes fallan la tarea al intentar reiniciar el sistema después de los cambios; esto no es necesario y puede causar que la validación falle.
Repetición: Asegúrate de realizar estos pasos en todos los servidores mencionados en la descripción de la tarea para obtener el puntaje completo.
Uso de gpasswd: Como alternativa para añadir usuarios a grupos, KodeKloud también sugiere el uso de sudo gpasswd --add <usuario> <grupo>. 

Respuesta: Para completar esta tarea en KodeKloud, debes conectarte vía SSH a los servidores indicados, crear el grupo con sudo groupadd <grupo>, crear el usuario si es necesario con sudo useradd <usuario> y finalmente asignarlo al grupo con sudo usermod -a -G <grupo> <usuario>
