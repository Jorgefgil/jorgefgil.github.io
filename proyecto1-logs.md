# Detección de anomalías en logs

Este proyecto utiliza AWS Lambda, Amazon CloudWatch, Amazon SNS, y Amazon S3 para configurar un sistema que monitorea los logs de actividades de una aplicación o servicio, detecta anomalías (como accesos no autorizados o errores recurrentes) y genera alertas de manera automática.

## Servicios utilizados

- **Amazon CloudWatch**: Para registrar y monitorear los logs generados por tu aplicación o servicio.
- **AWS Lambda**: Para procesar los logs y detectar anomalías.
- **Amazon SNS**: Para enviar alertas cuando se detectan anomalías.
- **Amazon S3**: Para almacenar los logs procesados y los detalles de las anomalías detectadas.

## Requisitos

- **Cuenta de AWS**: Necesitarás una cuenta de AWS con permisos adecuados para crear y administrar los servicios mencionados.
- **AWS CLI**: Asegúrate de tener configurada la AWS CLI en tu máquina local para interactuar con los servicios de AWS.
- **Permisos de IAM para Lambda**: Debes contar con permisos adecuados para crear funciones Lambda que puedan interactuar con CloudWatch Logs y SNS.
- **Aplicación generadora de logs**: Necesitarás una aplicación o servicio que esté configurado para generar logs que puedan ser almacenados en CloudWatch Logs (como errores o intentos de inicio de sesión fallidos).

## Pasos para implementar el sistema

### 1. Configurar CloudWatch para registrar logs

Lo primero es configurar CloudWatch Logs para registrar las actividades de tu aplicación o servicio. Los logs pueden ser generados por una instancia EC2 o cualquier servicio que ejecute el código de la aplicación.

#### Crear un grupo de logs:
1. Inicia sesión en la consola de AWS.
2. Ve a CloudWatch y selecciona Logs.
3. Crea un nuevo grupo de logs donde se almacenarán los logs generados por tu aplicación.

#### Configurar tu aplicación para que envíe logs a CloudWatch:
1. Si estás utilizando EC2, puedes configurar el CloudWatch Agent para recopilar y enviar logs del sistema a CloudWatch.
2. Si estás utilizando un servicio gestionado, como AWS Lambda, puedes hacer que los logs se envíen automáticamente a CloudWatch.
3. Verifica que los logs se estén generando correctamente para actividades como intentos fallidos de inicio de sesión, errores recurrentes, etc.

### 2. Crear una función Lambda para analizar los logs

Ahora, crea una función Lambda que se ejecute cada vez que se registre un nuevo log en CloudWatch. Lambda analizará los logs en busca de patrones anómalos que puedan indicar accesos no autorizados o errores recurrentes.

#### Crear una nueva función Lambda:
1. En la consola de AWS, ve a Lambda y crea una nueva función.
2. Selecciona un rol de ejecución que tenga permisos para acceder a CloudWatch Logs y SNS.

#### Código de la función Lambda:
La función Lambda deberá procesar los logs y buscar patrones anómalos, como intentos de inicio de sesión fallidos o accesos a horas inusuales.

**Código de ejemplo (lambda_function.py):**

Puedes revisar el código utilizado en este proyecto en [este repositorio](lambdas/logs).

#### Configurar el trigger de Lambda:
1. En la consola de Lambda, configura un trigger para que la función se active automáticamente cuando se registren nuevos logs en CloudWatch.

### 3. Almacenar los resultados en S3 y enviar alertas con SNS

Si Lambda detecta anomalías en los logs, se almacenarán en un bucket S3 para su análisis posterior y se enviará una alerta mediante SNS.

#### Configurar un bucket en S3:
1. En la consola de AWS, crea un bucket S3 donde se almacenarán los logs de anomalías.
2. Asegúrate de que el bucket tenga los permisos adecuados para que Lambda pueda escribir en él.

#### Configurar un tema SNS:
1. Crea un tema en SNS al que se enviarán las alertas.
2. Configura suscriptores (como una dirección de correo electrónico o una función Lambda) para recibir las notificaciones.

### 4. Probar el sistema

1. **Genera logs**: Asegúrate de que tu aplicación esté generando logs que puedan ser almacenados en CloudWatch (por ejemplo, intentos fallidos de inicio de sesión o accesos a horas inusuales).
2. **Verifica el procesamiento en Lambda**: Asegúrate de que la función Lambda se ejecute correctamente cuando se registren nuevos logs en CloudWatch.
3. **Revisa los logs de CloudWatch** para ver los detalles de la ejecución de Lambda.
4. **Verifica S3**: Revisa que las anomalías detectadas se almacenen correctamente en el bucket de S3.
5. **Recibe alertas de SNS**: Si se detectan anomalías, deberías recibir un mensaje en el tema SNS que configuraste.

## Solución de problemas

Durante la implementación, podrías encontrar los siguientes errores comunes:

### Permisos de IAM:
- Asegúrate de que tu función Lambda tenga permisos adecuados para acceder a CloudWatch Logs, SNS y S3.

### Problemas con los logs:
- Verifica que tu aplicación esté generando logs en el formato esperado.
- Asegúrate de que la configuración de CloudWatch Logs esté correctamente establecida.

### Errores en Lambda:
- Verifica los logs de Lambda en la consola de AWS para depurar cualquier error que pueda estar ocurriendo durante el procesamiento de los logs.

## Conclusión

Este sistema te permitirá monitorear logs de actividades en tiempo real, detectar anomalías como accesos no autorizados o errores recurrentes, y generar alertas automáticamente para que los administradores puedan tomar medidas rápidamente.

¡Esperamos que este proyecto te ayude a mejorar la seguridad y la fiabilidad de tus aplicaciones!


