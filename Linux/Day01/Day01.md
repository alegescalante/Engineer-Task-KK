Para crear el usuario rose con las especificaciones solicitadas en el servidor de aplicaciones 1, ejecutá el siguiente comando como root o usando sudo: 
bash
useradd -u 1354 -d /var/www/rose -m rose
Usa el código con precaución.

Detalles del comando:
-u 1354: Define el UID específico.
-d /var/www/rose: Establece la ruta del home directory.
-m: Crea el directorio automáticamente si no existe. 
¿Necesitás también configurar los permisos de grupo de Apache para que el servidor pueda leer los archivos en ese directorio?







2 sitios
RSocket Messaging with Spring – Jose Carvajal – Passionate Software Engineer
Server 1. - Add the RSocket dependencies in the pom. xml: io. 2. - Create the server application: public class ServerApp { public ...

GitHub

Linux Mastery: Creating Directories, Setting Permissions, and Ensuring Security.
20 may 2024 — -m : This option ensures that a home directory for the new user is created. If this option is not used, the home directory will no...

Medium

como verifico que se creo el usuario
Para verificar que el usuario rose se creó correctamente con el UID y el directorio personal (home) que definiste, podés usar cualquiera de estos métodos:
1. Usar el comando id 
Es la forma más rápida de confirmar el UID. 
Ask Ubuntu
Ask Ubuntu
 +1
bash
id rose
Usa el código con precaución.

Resultado esperado: Debería mostrar algo como uid=1354(rose) gid=1354(rose) grupos=1354(rose).
2. Consultar el archivo /etc/passwd 
Este archivo almacena la configuración de todos los usuarios del sistema, incluyendo su directorio home. 
Hostinger
Hostinger
 +1
bash
grep rose /etc/passwd
Usa el código con precaución.

Resultado esperado: Una línea con el formato rose:x:1354:1354::/var/www/rose:/bin/sh (o la shell que tenga asignada). El penúltimo campo confirma la ruta /var/www/rose. 
Unix & Linux Stack Exchange
Unix & Linux Stack Exchange
 +1
3. Verificar la existencia del directorio home
Para asegurarte de que el directorio físico fue creado: 
KodeKloud
KodeKloud
bash
ls -ld /var/www/rose
Usa el código con precaución.

Resultado esperado: Deberías ver el directorio con el usuario rose como dueño.
4. Usar el comando getent
Es una alternativa más limpia que grep para consultar las bases de datos administrativas. 
nixCraft
nixCraft
 +1
bash
getent passwd rose
