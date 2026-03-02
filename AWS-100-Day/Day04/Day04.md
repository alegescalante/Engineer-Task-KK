Para habilitar el versionado en el bucket de S3 especificado, podés utilizar la interfaz de línea de comandos de AWS (AWS CLI) o la consola web. El versionado permite conservar, recuperar y restaurar cualquier versión de los objetos almacenados, protegiéndolos contra eliminaciones accidentales o sobrescrituras. 

Opción 1: Mediante AWS CLI (Recomendado para DevOps) 
Ejecutá el siguiente comando en tu terminal para activar el versionado de forma inmediata: 
```bash
aws s3api put-bucket-versioning \
    --bucket xfusion-s3-15816 \
    --versioning-configuration Status=Enabled
```
Usa el código con precaución.

Para verificar que el cambio se aplicó correctamente, podés ejecutar: 

```bash
aws s3api get-bucket-versioning --bucket xfusion-s3-15816
```
Usa el código con precaución.

La salida debería mostrar: 
```bash
{"Status": "Enabled"}
```
## Opción 2: Mediante la Consola de AWS 
* 1 - Iniciá sesión en la Consola de Amazon S3.
* En la lista de Buckets, hacé clic en el nombre xfusion-s3-15816.
* Seleccioná la pestaña Properties (Propiedades).
* Buscá la sección Bucket Versioning (Versionado de buckets) y hacé clic en Edit (Editar).
* Seleccioná Enable (Habilitar) y hacé clic en Save changes (Guardar cambios).

## Consideraciones importantes
* Costos: AWS cobra por todas las versiones almacenadas de un objeto. Se recomienda configurar Lifecycle Rules (reglas de ciclo de vida) para eliminar versiones antiguas automáticamente después de un tiempo y optimizar costos.
* Eliminación: Cuando borrás un objeto con el versionado activo, S3 inserta un "delete marker" (marcador de eliminación). El objeto no desaparece permanentemente; podés recuperarlo eliminando dicho marcador.
* Persistencia: Una vez que el versionado se habilita, no se puede desactivar por completo, solo se puede suspender (lo que detiene la creación de nuevas versiones pero mantiene las existentes)
