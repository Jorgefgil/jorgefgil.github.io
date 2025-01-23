# Reconocimiento de imágenes para seguridad con IA

Este proyecto implementa un sistema de reconocimiento de imágenes para seguridad utilizando diversos servicios de AWS. El sistema carga imágenes desde Amazon S3, utiliza Amazon Rekognition para detectar caras en las imágenes y envía alertas si se detectan caras desconocidas. Los resultados de las detecciones se almacenan en Amazon DynamoDB y se envían notificaciones mediante Amazon SNS.

## Servicios utilizados

- **Amazon Rekognition**: Para el análisis de imágenes y la detección de caras.
- **AWS Lambda**: Para ejecutar el código que procesa las imágenes y maneja los resultados.
- **Amazon S3**: Para almacenar las imágenes que se analizan.
- **Amazon DynamoDB**: Para almacenar los resultados de las detecciones.
- **Amazon SNS**: Para enviar notificaciones en caso de detecciones de rostros desconocidos.

## Diagrama de la arquitectura que vamos a realizar

[Diagrama](/Capturadepantalla.png)

## Pasos para implementar el sistema

### 1. Crear un Bucket en S3

Primero, necesitas un bucket en Amazon S3 donde se cargarán las imágenes. Este bucket será utilizado para almacenar las imágenes de las cámaras de seguridad.

1. Inicia sesión en la consola de AWS.
2. Ve a S3 y crea un nuevo bucket con un nombre único.
3. Configura los permisos del bucket para permitir acceso a Lambda.

### 2. Crear una función Lambda

La función Lambda será responsable de procesar las imágenes cargadas en S3, detectar caras usando Rekognition y almacenar los resultados en DynamoDB. Sigue estos pasos:

1. Crea una función Lambda:
   - En la consola de AWS, ve a Lambda y crea una nueva función.
   - Configura un rol de ejecución de Lambda con permisos para acceder a S3, Rekognition, DynamoDB y SNS.
   - Sube el código de la función Lambda. El código en Python usa el servicio de Rekognition para analizar las imágenes y busca rostros en ellas. Si encuentra una cara, guarda los resultados en DynamoDB y envía una notificación SNS.

   **Código de ejemplo (lambda_function.py):**
   
[Ejemplo de código que puedes utilizar](lambdas/reconocimiento-caras).

2. Configuración de la función Lambda:
   - Asegúrate de que la función Lambda esté configurada para ejecutarse cuando se cargue una imagen en S3. Para hacer esto, puedes configurar un evento de activación desde la consola de S3.

### 3. Crear un tema SNS

En la consola de SNS, crea un nuevo tema SNS.

1. Configura el ARN del tema en el código de Lambda para enviar las notificaciones.

### 4. Crear una tabla en DynamoDB

En la consola de DynamoDB, crea una nueva tabla llamada `FaceDetectionResults`.

1. Define una clave primaria para la tabla, como `ImageName` (tipo String).
2. Asegúrate de que la tabla esté en la misma región que tu función Lambda.

### 5. Probar el sistema

1. Cargar una imagen en el bucket S3.
2. Verifica los logs de Lambda para asegurarte de que la función Lambda se ejecuta correctamente.
3. Revisa DynamoDB para confirmar que los resultados se almacenaron correctamente.
4. Revisa SNS para confirmar que las notificaciones se enviaron correctamente.

## Solución de problemas

Durante la implementación, puedes encontrar los siguientes errores comunes:

### Problema de permisos con SNS:
- Asegúrate de que la función Lambda tiene los permisos adecuados para publicar en SNS.
- Verifica el ARN del tema SNS y asegúrate de que esté correcto.

### Error de DynamoDB:
- Verifica que la tabla DynamoDB exista y tenga el nombre correcto en el código.
- Revisa los permisos de Lambda para escribir en DynamoDB.

### Errores en Rekognition:
- Asegúrate de que las imágenes en S3 estén en un formato compatible con Rekognition.
- Verifica que la imagen tenga la calidad necesaria para detectar caras.

## Conclusión

Este sistema de reconocimiento de imágenes proporciona una forma automatizada de detectar personas en imágenes cargadas a S3, almacenando los resultados en DynamoDB y enviando alertas en tiempo real. La arquitectura es escalable y aprovecha varios servicios gestionados de AWS para ofrecer un flujo de trabajo eficiente.

¡Esperamos que este proyecto sea útil para mejorar la seguridad en tu entorno!

