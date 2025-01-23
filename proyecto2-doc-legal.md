# Clasificación de documentos legales usando AI

Este proyecto usa varios servicios de AWS para extraer, clasificar y almacenar metadatos de documentos legales (como contratos, acuerdos de confidencialidad, etc.) utilizando Amazon Textract, AWS Lambda, Amazon S3 y Amazon DynamoDB.

El objetivo es automatizar el procesamiento de documentos legales, clasificarlos según su tipo y almacenar los metadatos relevantes en DynamoDB para su posterior análisis.

## Servicios utilizados

- **Amazon Textract**: Para extraer texto y datos estructurados de documentos escaneados o imágenes.
- **AWS Lambda**: Para ejecutar el código que invoca Textract y procesa los resultados.
- **Amazon S3**: Para almacenar los documentos legales que serán procesados.
- **Amazon DynamoDB**: Para almacenar los metadatos extraídos y clasificados de los documentos.

## Requisitos

- **Cuenta de AWS**: Necesitarás tener acceso a una cuenta de AWS con permisos adecuados para crear y administrar los servicios mencionados.
- **AWS CLI**: Asegúrate de tener configurada la AWS CLI en tu máquina local para interactuar con los servicios de AWS.
- **IAM Role para Lambda**: Debes crear un rol de IAM con permisos adecuados para interactuar con S3, Textract y DynamoDB.
- **Conocimiento básico de expresiones regulares** (si se va a usar para la clasificación).

## Pasos para implementar el sistema

### 1. Crear un Bucket en S3

El primer paso es crear un bucket en Amazon S3 donde se cargarán los documentos legales. Este bucket servirá como almacenamiento para los documentos que se procesarán.

1. Inicia sesión en la consola de AWS.
2. Ve a S3 y crea un nuevo bucket. Dale un nombre único y selecciona la región que prefieras.
3. Configura los permisos del bucket para permitir el acceso adecuado a Lambda.

### 2. Crear una función Lambda

La función Lambda se activará automáticamente cuando un documento sea cargado en el bucket de S3. Esta función invocará Amazon Textract para extraer texto y datos estructurados de los documentos.

1. **Crear una nueva función Lambda**:
   - En la consola de AWS, ve a Lambda y crea una nueva función.
   - Selecciona un rol de ejecución con permisos para S3, Textract y DynamoDB.

2. **Código de la función Lambda**: La función Lambda debe ejecutar el servicio Textract y luego procesar los resultados para extraer los metadatos y clasificarlos.

   **Código de ejemplo (lambda_function.py)**:
   
  Puedes revisar el código utilizado en este proyecto en [este repositorio](lambdas/legal).

3. **Configuración de Lambda**:
   - Configura un trigger en la consola de S3 para que la función Lambda se active cuando un documento sea cargado en el bucket de S3.
   - En la configuración de Lambda, asegúrate de asignar suficiente memoria y tiempo de ejecución para que Textract pueda procesar el documento adecuadamente.

### 3. Crear una tabla en DynamoDB

Crea una tabla en Amazon DynamoDB para almacenar los metadatos extraídos de los documentos procesados.

1. En la consola de DynamoDB, crea una nueva tabla llamada `LegalDocumentsMetadata`.
2. Define una clave primaria para la tabla, como `DocumentName` (tipo String).
3. Asegúrate de que la tabla esté en la misma región que tu función Lambda.

### 4. Probar el sistema

1. Sube un documento a tu bucket S3 (por ejemplo, un contrato o acuerdo de confidencialidad).
2. Verifica los logs de Lambda para asegurarte de que el documento fue procesado correctamente.
3. Revisa DynamoDB para confirmar que los metadatos se almacenaron correctamente.
4. Puedes ajustar el proceso de clasificación de documentos según sea necesario. Si deseas usar técnicas más avanzadas de clasificación, podrías integrar un modelo de aprendizaje automático (aunque este podría requerir permisos adicionales).

## Solución de problemas

Durante la implementación, puedes encontrar los siguientes errores comunes:

### Error con Textract:
- Asegúrate de que el documento esté en un formato compatible con Textract (PDF o imágenes escaneadas).
- Verifica que la función Lambda tenga los permisos adecuados para invocar Textract.

### Problemas con DynamoDB:
- Asegúrate de que la tabla de DynamoDB exista y tenga el nombre correcto.
- Verifica que la función Lambda tenga los permisos adecuados para escribir en DynamoDB.

### Problemas de clasificación:
- Si la clasificación no funciona correctamente, ajusta las palabras clave o el algoritmo de clasificación para mejorar la precisión.

## Conclusión

Este sistema automatiza el procesamiento y clasificación de documentos legales, lo que reduce el tiempo y esfuerzo manual para analizar y organizar grandes volúmenes de documentos. Utilizando servicios escalables de AWS, el proceso es eficiente y flexible, y puedes extenderlo para incluir modelos más complejos de clasificación si es necesario.

¡Esperamos que este proyecto te sea útil para gestionar tus documentos legales de manera más eficiente!

