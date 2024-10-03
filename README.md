# TallerBintecGPT4ALL
Esta es la guía para el evento Bintec de Bancolombia 2024

## 1. GPT4ALL
Es una aplicación de escritorio creada por NomicAI que permite descargar y ejecutar modelos de lenguaje grande (LLM) en cualquier PC de escritorio o Laptop https://docs.gpt4all.io/index.html

![image](https://github.com/user-attachments/assets/5b6f61d1-131b-4082-b313-4535feae1975)

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
- Descargar modelos “Llama 3 8B Instruct” y “Phi-3 Mini Instruct”

## 5 Configurar GPT4ALL

## 5.1 Descargar modelos
En el panel de la izquierda, clic en "Modelos", luego clic en "+Agregar Modelos"
![image](https://github.com/user-attachments/assets/eca50b73-2b14-4d69-b736-9bc2f6e6731e)
Buscar el modelo en la lista y dar clic en "Instalar". La descarga puede tardar varias horas dependiendo del tamaño

## 5.1 Doc Locales (localdocs)
Por lo general, los LLM se entrenan con una amplia variedad de datos, lo que les proporciona una comprensión general, pero puede dar lugar a lagunas en áreas de conocimiento específicas. A veces, incluso pueden producir información desviada o sesgada, un subproducto del aprendizaje a partir de una web vasta y sin filtrar. Para solucionar este problema, se introdujo el concepto de bases de datos vectoriales. Estas bases de datos almacenan datos en un formato único conocido como «incrustación vectorial» (vector embeddings), que permite a los LLM captar y utilizar la información de forma más contextual y precisa.

«Doc Locales» es una caracteristica que permite guardar los archivos de una carpeta en disco como incrustaciones (embeddings) en una base de datos local que posteriormente utiliza un modelo que se ejecuta en GPT4ALL para responder preguntas sobre los documentos. Esto garantiza la privacidad de los datos. El proceso que consulta la base de datos vectorial para responder preguntas se conoce como RAG (Retrieval-Augmented Generation). Esta es una explicación super sencilla pero suficiente para lo que necesitamos hacer.

Doc Locales permite crear "Colecciones". Cada Colección tiene un nombre y la ruta a una carpeta en disco donde se guardan los documentos que se desean consultar a traves de un chat.

**Recomendaciones**
- No guardar archivos de diferentes temas o areas en la misma carpeta. Por organización se debe crear un carpeta por área de búsqueda
- Solo guardar en la carpeta los documentos relevantes. Mas documentos implica un aumento de tamaño en la base de datos vectorial que ralentizará las respuestas. Aquí es más importante la calidad que la cantidad
- Por defecto solo se indexarán archivos con extensiones **txt,pdf,md,rst**

### 5.1.1 Crear una Colección
En primer lugar se debe crear una carpeta en disco, en cualquier ruta. Copiar en esta carpeta los documentos a consultar. Se recomienda crear una carpeta para cada categoría

a. Clic en “Docs Locales”

![image](https://github.com/user-attachments/assets/de0f2cd1-ae0f-4c4b-9d08-dea637261cc2)

b. Clic en “+Agregar colección”

![image](https://github.com/user-attachments/assets/8a3a34ec-cf85-46a0-9361-de380a14452a)

c. Escribir nombre y seleccionar carpeta

![image](https://github.com/user-attachments/assets/8f1d3a3b-721b-4352-8c99-337d58ba4d36)

c. Clic en "Crear Colección"

**GPT4LL empieza a indexar los documentos en la carpeta seleccionada**
![image](https://github.com/user-attachments/assets/1f73fda9-566d-4a10-877b-6145002ebca1)

A partir del momento que se crea una colección, GPT4ALL monitorea la carpeta. Si se eliminan o agregan documentos de la carpeta asociada a la colección ésta se actualiza automáticamente

![image](https://github.com/user-attachments/assets/9718b670-0359-48d0-aca3-81eb90fdf844)

## 6. Configuración del modelo
Para obtener los mejores resultados debemos configurar dos propiedades del modelo que se usará para chatear con los documentos de la colección creada. Estos cambios permitirán obtener una respuesta detallada de que parte del documento se utilizó para responder la pregunta. 

En el panel de la izquierda clic en "Config" luego clic en "Modelo"
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


