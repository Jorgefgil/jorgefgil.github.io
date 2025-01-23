# Proyecto 1: Detección de Anomalías en Logs con AWS Lambda y CloudWatch

## Descripción

En este proyecto, creamos un sistema de monitoreo para detectar anomalías en los logs generados por una aplicación o servicio. Utilizamos **AWS Lambda**, **Amazon CloudWatch**, **SNS**, y **S3** para procesar los logs, detectar patrones inusuales como accesos no autorizados, y generar alertas.

## Servicios Involucrados:

- **AWS Lambda**
- **Amazon CloudWatch**
- **Amazon SNS**
- **Amazon S3**

## Pasos Detallados:

1. **Configurar CloudWatch para registrar logs**:
   Configuramos **CloudWatch Logs** para registrar las actividades de la aplicación. Estos logs pueden incluir errores, intentos de inicio de sesión fallidos, etc.

2. **Crear una función Lambda**:
   Creamos una función **Lambda** que se ejecuta cada vez que se registran nuevos logs. La función analiza los logs y detecta anomalías como intentos fallidos de inicio de sesión.

3. **Almacenar en S3 y enviar alertas con SNS**:
   Si se detectan anomalías, almacenamos los detalles en **S3** y enviamos alertas a los administradores mediante **SNS**.

## Repositorio de Código

Puedes revisar el código utilizado en este proyecto en [este repositorio](https://github.com/username/proyecto1).
