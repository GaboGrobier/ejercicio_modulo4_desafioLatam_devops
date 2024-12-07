# PruebaModulo4-bucket

Este archivo de configuración de GitHub Actions se utiliza para automatizar el proceso de verificación del código y despliegue de una aplicación web en AWS S3 y CloudFront. El flujo de trabajo se divide en dos trabajos: **VerificarCodigo** y **DesplegarAWS**.

## Descripción de los Jobs

### 1. **VerificarCodigo**
Este trabajo verifica si el código en el repositorio está correcto antes de proceder con el despliegue.

- **Obtener código:** Se utiliza la acción `actions/checkout` para obtener el código del repositorio.
- **Verificación:** Si la obtención del código fue exitosa, se imprime un mensaje indicando que el código se verificó correctamente.

### 2. **DesplegarAWS**
Este trabajo se encarga de desplegar la aplicación en AWS S3 y realizar la invalidación de la caché de CloudFront, pero solo si el trabajo anterior (`VerificarCodigo`) fue exitoso.

- **Obtener código:** Se obtiene nuevamente el código del repositorio.
- **Configurar credenciales de AWS:** Utiliza la acción `aws-actions/configure-aws-credentials` para configurar las credenciales necesarias para acceder a los servicios de AWS.
- **Verificar S3:** Verifica la existencia del bucket de S3 utilizando la clave de acceso y el path configurados en los secretos del repositorio.
- **Desplegar archivos en S3:** Copia los archivos `index.html` y `error.html` a la ruta configurada en el bucket S3.
- **Invalidar caché de CloudFront:** Realiza una invalidación de la caché de CloudFront para asegurar que los cambios sean reflejados inmediatamente.

## Uso

Este flujo de trabajo se ejecuta automáticamente cuando se realiza una solicitud de *pull request* a la rama `main`.

### Requisitos

- **AWS credentials:** Deben configurarse los secretos `AWS_ACCESS_KEY_ID` y `AWS_SECRET_ACCESS_KEY` en el repositorio.
- **S3 bucket path:** Configura el secreto `AWS_S3_PATH` para indicar la ubicación en el bucket S3 donde se desplegarán los archivos.
- **CloudFront distribution ID:** Configura el secreto `CLOUDFRONT_DISTRIBUTION_ID` para invalidar la caché correctamente.

Este flujo de trabajo ayuda a automatizar el proceso de verificación y despliegue de aplicaciones web, asegurando que solo se despliegue código verificado y manteniendo los archivos actualizados en S3 y CloudFront.
