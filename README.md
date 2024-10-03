# TallerBintecGPT4ALL
Esta es la guía para el evento Bintec de Bancolombia 2024

## 1. GPT4ALL
Es una aplicación de escritorio creada por NomicAI que permite descargar y ejecutar modelos de lenguaje grande (LLM) en cualquier PC de escritorio o Laptop https://docs.gpt4all.io/index.html

![image](https://github.com/user-attachments/assets/5b6f61d1-131b-4082-b313-4535feae1975)

Para la descarga e instalación de GPT4ALL seguir el enlace [instalación](https://docs.gpt4all.io/gpt4all_desktop/quickstart.html) y descargar el instalador de acuerdo al sistema operativo de su PC o laptop

## 2 Ventajas
- No requiere acceso a internet 
- No requiere GPU 
- Privacidad de la información 
- Acceso a una amplia variedad de modelos

## 3 Desventajas
- El prompt debe ser más detallado 
- Hay modelos que requieren 16GB o más de RAM 
- Mas lento para responder

## 4 Requerimientos
- Contar con un PC con al menos 8GB de RAM y un procesador Intel Corei5.  
- Tener instalado GPT4All Link descarga 
- Descargar modelos `Llama 3 8B Instruct` y `Phi-3 Mini Instruct`

## 5 Configurar GPT4ALL

## 5.1 Descargar modelos
En el panel de la izquierda, clic en `Modelos`, luego clic en `+Agregar Modelos`

![image](https://github.com/user-attachments/assets/eca50b73-2b14-4d69-b736-9bc2f6e6731e)

**Buscar el modelo en la lista y dar clic en `Instalar`. La descarga puede tardar varias horas dependiendo del tamaño**

## 5.1 Doc Locales (localdocs)
Por lo general, los modelos se entrenan con una amplia variedad de datos, lo que les proporciona una comprensión general, pero puede dar lugar a lagunas en áreas de conocimiento específicas. A veces, incluso pueden producir información desviada o sesgada, un subproducto del aprendizaje a partir de una web vasta y sin filtrar. Para solucionar este problema, se introdujo el concepto de bases de datos vectoriales. Estas bases de datos almacenan datos en un formato único conocido como «incrustación vectorial» (vector embeddings), que permite a los LLM captar y utilizar la información de forma más contextual y precisa.

`Doc Locales` es una caracteristica que permite guardar los archivos de una carpeta en disco como incrustaciones (embeddings) en una base de datos local que posteriormente utiliza un modelo que se ejecuta en GPT4ALL para responder preguntas sobre los documentos. Esto garantiza la privacidad de los datos. El proceso que consulta la base de datos vectorial para responder preguntas se conoce como RAG (Retrieval-Augmented Generation). Esta es una explicación super sencilla pero suficiente para lo que necesitamos hacer.

Doc Locales permite crear `Colecciones`. Cada Colección tiene un nombre y la ruta a una carpeta en disco donde se guardan los documentos que se desean consultar a traves de un chat.

**Recomendaciones**
- No guardar archivos de diferentes temas o areas en la misma carpeta. Por organización se debe crear un carpeta por área de búsqueda
- Solo guardar en la carpeta los documentos relevantes. Mas documentos implica un aumento de tamaño en la base de datos vectorial que ralentizará las respuestas. Aquí es más importante la calidad que la cantidad
- Por defecto solo se indexarán archivos con extensiones **txt,pdf,md,rst**

### 5.1.1 Crear una Colección
En primer lugar se debe crear una carpeta en disco, en cualquier ruta. Copiar en esta carpeta los documentos a consultar. Se recomienda crear una carpeta para cada categoría

a. Clic en `Docs Locales`

![image](https://github.com/user-attachments/assets/de0f2cd1-ae0f-4c4b-9d08-dea637261cc2)

b. Clic en `+Agregar colección`

![image](https://github.com/user-attachments/assets/8a3a34ec-cf85-46a0-9361-de380a14452a)

c. Escribir nombre y seleccionar carpeta

![image](https://github.com/user-attachments/assets/8f1d3a3b-721b-4352-8c99-337d58ba4d36)

c. Clic en `Crear Colección`

**GPT4LL empieza a indexar los documentos en la carpeta seleccionada**
![image](https://github.com/user-attachments/assets/1f73fda9-566d-4a10-877b-6145002ebca1)

A partir del momento que se crea una colección, GPT4ALL monitorea la carpeta. Si se eliminan o agregan documentos de la carpeta asociada a la colección ésta se actualiza automáticamente

![image](https://github.com/user-attachments/assets/9718b670-0359-48d0-aca3-81eb90fdf844)

## 6. Configuración del modelo
Para obtener los mejores resultados debemos configurar dos propiedades del modelo que se usará para chatear con los documentos de la colección creada. Estos cambios permitirán obtener una respuesta detallada de que parte del documento se utilizó para responder la pregunta. 

En el panel de la izquierda clic en `Config` luego clic en `Modelo`
![image](https://github.com/user-attachments/assets/5d6c6b45-1777-4551-b9f6-4f951e43fb4a)

En la propiedad "Indicación del sistema" borrar el contenido y agregar el siguiente texto:
```
### System:
Eres un asistente AI especializado en proporcionar información basada únicamente en documentos locales (localdocs). Tu tarea es responder preguntas y realizar análisis utilizando exclusivamente la información contenida en estos documentos. No debes utilizar ningún conocimiento externo o información que no esté presente en los localdocs.

Instrucciones:
1. Analiza cuidadosamente la consulta del usuario.
2. Busca información relevante únicamente en los documentos locales proporcionados.
3. Si la información solicitada no se encuentra en los localdocs, indica claramente que no puedes responder debido a la falta de información en los documentos locales.
4. No generes ni inventes información que no esté explícitamente presente en los localdocs.
5. Cita la fuente específica del documento local cuando proporciones información.
6. Si se te pide una opinión o análisis, básalo únicamente en la información disponible en los localdocs.

Recuerda: Tu conocimiento se limita estrictamente a lo que está contenido en los documentos locales. No utilices información externa bajo ninguna circunstancia.
```

En "Plantilla de indicación" borrar el contenido y agregar lo siguiente:
```
<|start_header_id|>user<|end_header_id|>
Consulta: %1

Instrucciones para el asistente:
1. Analiza la consulta anterior.
2. Busca información relevante únicamente en los documentos locales (localdocs) disponibles.
3. Proporciona una respuesta basada exclusivamente en la información encontrada en los localdocs.
4. Si no encuentras información relevante en los localdocs, indica claramente que no puedes responder debido a la falta de información en los documentos locales.
5. Cita la fuente específica del documento local para cada pieza de información que proporciones.
<|eot_id|><|start_header_id|>assistant<|end_header_id|>
%2<|eot_id|>
```

## 7. Casos de uso

## 7.1 Creación de un asistente para consultar temas relacionados con el código de tránsito
Se deben seguir los pasos del numeral `5.1.1` para crear la colección "Transito" utilizando el archivo [código nacional de tránsito](https://github.com/manjarjc/TallerBintecGPT4ALL/blob/main/documentos/transito/ley-769-de-2002-codigo-nacional-de-transito_3704_0.pdf)
Tambien puede descargar el archivo de [aquí](https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=5557)

**En el panel de la izquierda dar clic en `Chats`, luego dar clic en `+Nuevo Chat`**
![image](https://github.com/user-attachments/assets/4e272c37-4189-4cad-845c-392aad6b3543)


**Ahora seleccionar el modelo `Llama 3 8B instruct` y dar clic en `DocumentosLocales` para seleccionar la colección `Transito`**

![image](https://github.com/user-attachments/assets/1959e3cc-5ada-4c6f-a6e5-b9551ddf102a)

**En la parte inferior, en el cuadro que dice `Enviar un mensaje` escribir la siguiente pregunta:**
![image](https://github.com/user-attachments/assets/016da59b-2b38-4855-ab2a-68962e3c687d)

```
Cuáles son las sanciones por no portar un seguro obligatorio vigente (SOAT)?
```

**Presionar enter, lo cual inicia el proceso de buscar en DocumentosLocales para responder**
![image](https://github.com/user-attachments/assets/26a8d9bd-c8b6-4e99-85f5-3ae4caf885c1)

**La respuesta puede variar pero siempre debe mencionar que partes del documento se consultaron**
![image](https://github.com/user-attachments/assets/0d6c3a6a-b5b2-4615-8283-84613422db02)

**Hacer las siguientes preguntas y verificar las fuentes consultadas:**
```
﻿En qué circunstancias puede un guardia de tránsito inmovilizar mi vehículo?
```
```
Qué pasos debe seguir un conductor para reportar un accidente y levantar un croquis del incidente de tránsito?
```
```
Qué debe hacer un conductor en caso de que su vehículo sea inmovilizado por las autoridades de tránsito?
```

## 7.2 Asesor de finanzas personales
Se debe crear la colección "FinanzasPersonales" y agregar el archivo [finanzas](https://github.com/manjarjc/TallerBintecGPT4ALL/blob/main/documentos/finanzas-personales/reporte_ingresos_egresos_familia_detallado.pdf)

Hacer la siguiente pregunta:
```
De acuerdo al reporte de ingresos y egresos familiares que gastos se pueden recortar para lograr un ahorro del 5%?
```

![image](https://github.com/user-attachments/assets/ca42a4fe-be9c-4899-96d3-341c6ca40793)

## 6.3 Generación de código cumpliendo lineamientos

```
Necesito un procedimiento almacenado para borrar registros de dos tablas de una base de datos de PostgreSQL que realice las siguientes tareas:
1. Definir un parametro de entrada "max_records" que permita establecer la cantidad maxima de registros a eliminar de las dos tablas
2. Recuperar de la tabla "TRANSACTION_INFORMATION" los registros donde el valor del campo "date" tenga más de 90 días. Devolver el campo "ID"
3. Si no se devuelven registros en el punto 2 terminar el procedimiento
4. Iniciar una transaccion antes de iniciar el bucle
5. Iterar sobre cada uno de los registros devueltos en el punto 2
6. Eliminar los registros de la tabla "EVENT_TRANSACTION" donde el campo "FK_TRANSACTION_ID" sea igual al campo "ID" del punto 2 y guardar la cantidad de registros eliminados en la variable "deleted_event"
7. Eliminar el registro de la tabla "TRANSACTION_INFORMATION" donde el valor del campo "ID" que sea igual al campo "ID" del punto 2  y guardar la cantidad de registros eliminados en la variable "deleted_tran"
8. En cada ciclo, actualizar la variable "total_records_deleted", sumando al valor actual el valor de las variables "deleted_event" y "deleted_tran"
9. En cada ciclo se debe verificar si el valor de la variable "total_records_deleted" es mayor o igual al parametro de entrada "max_records". Si se cumple la condición finalizar el procedimiento
10. Confirmar la transacción iniciada en el punto 4
11. Devolver el valor de la variable "total_records_deleted"
12. Implementar un control de errores que haga un rollback de transacciones pendientes
```


