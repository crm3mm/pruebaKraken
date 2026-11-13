# Microservicio generador de PDF

Microservicio propuesto para convertir HTML en archivos PDF. El backend de otra
aplicación enviará el HTML y este servicio preparará las imágenes, generará el
documento y devolverá el PDF para que la aplicación pueda descargarlo en el
dispositivo del usuario.

> Este repositorio aún no contiene la implementación del servicio ni define un
> stack tecnológico. La API y el flujo de esta documentación son una propuesta
> inicial.

## Flujo esperado

1. El backend de la aplicación envía el HTML al microservicio.
2. El microservicio procesa el HTML y obtiene las imágenes que necesita el
   documento.
3. Espera a que las imágenes estén disponibles y genera el PDF conservando el
   diseño del HTML.
4. Devuelve el archivo PDF como respuesta.
5. La aplicación que inició la solicitud entrega el archivo al navegador o
   dispositivo para iniciar la descarga.

El microservicio no puede guardar directamente un archivo en el dispositivo del
usuario. Para iniciar esa descarga, el cliente de la aplicación debe recibir la
respuesta PDF y ofrecerla al usuario como archivo descargable.

## API propuesta

### `POST /api/pdf`

Recibe el HTML y devuelve el documento generado con el tipo de contenido
`application/pdf`.

Ejemplo de solicitud:

```http
POST /api/pdf
Content-Type: application/json
```

```json
{
  "html": "<html><body><h1>Reporte</h1></body></html>",
  "filename": "reporte.pdf"
}
```

`filename` puede ser opcional; si se omite, el servicio puede asignar un nombre
predeterminado. La respuesta debería incluir el encabezado
`Content-Disposition: attachment` para facilitar la descarga como archivo.

El contrato definitivo, incluyendo nombres de campos, autenticación, límites y
formato de errores, deberá establecerse al implementar el servicio.

## Imágenes

El HTML puede incluir imágenes referenciadas mediante URL. El servicio deberá
obtenerlas antes de generar el PDF y esperar a que terminen de cargar. Para que
esto funcione, las imágenes deben ser accesibles desde el entorno donde se
ejecute el microservicio.

Al descargar recursos indicados por solicitudes, la implementación deberá
limitar los destinos permitidos, el tamaño y el tiempo de descarga, y evitar el
acceso a direcciones de red internas o locales.

## Estado del proyecto

La conversión de HTML a PDF, la descarga de imágenes, el endpoint HTTP y la
descarga desde la aplicación cliente están pendientes de implementación.
