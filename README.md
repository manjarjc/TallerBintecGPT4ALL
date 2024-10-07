# Esta es la guía para el evento Bintec de Bancolombia 2024

## 1. GPT4ALL
Es una aplicación de escritorio creada por NomicAI que permite descargar y ejecutar modelos de lenguaje grande (LLM) en cualquier PC de escritorio o Laptop

![image](https://github.com/user-attachments/assets/ce10ac04-42db-408f-89bc-d451fb7dec09)

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
- Tener instalado GPT4All 
- Descargar modelos `Llama 3 8B Instruct` y `TheBloke/CodeLlama-7B-Instruct-GGUF`

## 5 Configurar GPT4ALL

## 5.1 Descargar modelos
En el panel de la izquierda, clic en `Modelos`, luego clic en `+Agregar Modelo`

![image](https://github.com/user-attachments/assets/eca50b73-2b14-4d69-b736-9bc2f6e6731e)

**Buscar el modelo en la lista y dar clic en `Instalar`. La descarga puede tardar varias horas dependiendo del tamaño**

## 5.1 Doc Locales (localdocs)
Por lo general, los modelos se entrenan con una amplia variedad de datos, lo que les proporciona una comprensión general, pero puede dar lugar a lagunas en áreas de conocimiento específicas. A veces, incluso pueden producir información desviada o sesgada, un subproducto del aprendizaje a partir de una web vasta y sin filtrar. Para solucionar este problema, se introdujo el concepto de bases de datos vectoriales. Estas bases de datos almacenan datos en un formato único conocido como «incrustación vectorial» (vector embeddings), que permite a los modelos captar y utilizar la información de forma más contextual y precisa.

`Doc Locales` es una caracteristica que permite guardar los archivos de una carpeta en disco como incrustaciones (embeddings) en una base de datos local que posteriormente utiliza un modelo que se ejecuta en GPT4ALL para responder preguntas sobre los documentos. Esto garantiza la privacidad de los datos. El proceso que consulta la base de datos vectorial para responder preguntas se conoce como RAG (Retrieval-Augmented Generation). Esta es una explicación super sencilla pero suficiente para lo que necesitamos hacer.

Doc Locales permite crear `Colecciones`. Cada Colección tiene un nombre y la ruta a una carpeta en disco donde se guardan los documentos que se desean consultar a traves de un chat.

**Recomendaciones**
- No guardar archivos de diferentes temas o areas en la misma carpeta. Por organización se debe crear una carpeta por área de búsqueda
- Solo guardar en la carpeta los documentos relevantes. Mas documentos implica un aumento de tamaño en la base de datos vectorial que ralentizará las respuestas. Aquí es más importante la calidad que la cantidad
- Por defecto solo se indexarán archivos con extensiones **txt,pdf,md,rst**

### 5.1.1 Crear una Colección
En primer lugar se debe crear una carpeta en disco, en cualquier ruta. Copiar en esta carpeta los documentos a consultar. Se recomienda crear una carpeta para cada categoría

a. Clic en `Docs Locales`

![image](https://github.com/user-attachments/assets/de0f2cd1-ae0f-4c4b-9d08-dea637261cc2)

b. Clic en `+Agregar colección`. Se muestra una ventana donde se debe escribir el nombre de la colección; luego clic en `Explorar` para seleccionar la carpeta

![image](https://github.com/user-attachments/assets/8f1d3a3b-721b-4352-8c99-337d58ba4d36)

c. Clic en `Crear Colección`

![image](https://github.com/user-attachments/assets/8a3a34ec-cf85-46a0-9361-de380a14452a)


**GPT4LL empieza a indexar los documentos en la carpeta seleccionada**
![image](https://github.com/user-attachments/assets/1f73fda9-566d-4a10-877b-6145002ebca1)

## A partir del momento que se crea una colección, GPT4ALL monitorea la carpeta. Si se eliminan o agregan documentos de la carpeta asociada a la colección ésta se actualiza automáticamente

![image](https://github.com/user-attachments/assets/9718b670-0359-48d0-aca3-81eb90fdf844)

## 6. Configuración del modelo
Para obtener los mejores resultados se deben configurar dos propiedades del modelo: `Indicación del sistema` y `Plantilla de indicación` que se usarán para chatear con los documentos de la colección creada. Estos cambios permitirán obtener una respuesta detallada que muestra que partes del documento se utilizaron para responder la pregunta. 

En GPT4ALL, en el panel de la izquierda clic en `Config` luego clic en `Modelo`
![image](https://github.com/user-attachments/assets/5d6c6b45-1777-4551-b9f6-4f951e43fb4a)

Buscar la propiedad `Indicación del sistema`, borrar el contenido y agregar el siguiente texto:
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

Ahora buscar la propiedad `Plantilla de indicación`, borrar el contenido y agregar lo siguiente:
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

## 7.3 Generación de código: validar cumplimiento de lineamientos

Este es un caso que no involucra Documentos Locales; debemos proporcionar unos lineamientos que se deben cumplir al desarrollar código para PostgreSQL. Para lograrlo, debemos configurar dos propiedades del modelo `TheBloke/CodeLlama-7B-Instruct-GGUF`: `Indicación del sistema` y `Plantilla de indicación`

Buscar la propiedad `Indicación del sistema`; borrar el contenido y agregar el siguiente texto:
```
### System:
Eres un asistente AI especializado en verificar el cumplimiento de lineamientos en el código SQL para PostgreSQL.
```

Ahora buscar la propiedad `Plantilla de indicación`; borrar el contenido y agregar lo siguiente:
```
### Human:

Consulta: %1

Debes verificar si el código SQL en la consulta cumple los siguientes lineamientos:

1. Organización y Estructura del Código
   - Nombres descriptivos:
     - Usa nombres claros y representativos para procedimientos y parámetros.
     - Ejemplo: 
       CREATE PROCEDURE update_order_status(order_id INT, new_status VARCHAR)
       BEGIN
         -- Lógica aquí
       END;

   - Comentarios claros:
     - Documenta la lógica del procedimiento.
     - Ejemplo:
       -- Actualiza el estado de un pedido específico

2. Seguridad y Control de Acceso
   - Uso de SECURITY DEFINER:
     - Limita los privilegios y utiliza SECURITY DEFINER solo si es necesario.
     - Ejemplo:
       CREATE PROCEDURE sensitive_procedure()
       SECURITY DEFINER
       BEGIN
         -- Lógica sensible
       END;

   - Validación de parámetros:
     - Valida las entradas para evitar ataques de inyección SQL.
     - Ejemplo:
       CREATE PROCEDURE validate_input(id INT)
       BEGIN
         IF id IS NULL THEN
           RAISE EXCEPTION 'El ID no puede ser NULL';
         END IF;
       END;

3. Eficiencia y Rendimiento
   - Evita SELECT *:
     - Selecciona solo las columnas necesarias.
     - Ejemplo:
       SELECT nombre, apellido FROM usuarios WHERE id = input_id;

   - Uso efectivo de índices:
     - Verifica los índices adecuados antes de crear procedimientos que interactúen con grandes conjuntos de datos.
   - Operaciones por lotes:
     - Utiliza operaciones en conjunto en vez de por fila.
     - Ejemplo:
       INSERT INTO inventario (producto_id, cantidad) 
       SELECT producto_id, SUM(cantidad) FROM ventas GROUP BY producto_id;

4. Manejo de Errores
   - Uso de bloques EXCEPTION:
     - Implementa manejo de excepciones para capturar errores.
     - Ejemplo:
       CREATE PROCEDURE handle_error_example()
       BEGIN
         BEGIN
           -- Operación que puede fallar
         EXCEPTION WHEN OTHERS THEN
           RAISE NOTICE 'Ocurrió un error';
         END;
       END;

5. Mantenimiento y Evolución
   - Versionado de procedimientos:
     - Usa nombres versionados para manejar cambios.
     - Ejemplo:
       CREATE PROCEDURE calculate_tax_v1()
       CREATE PROCEDURE calculate_tax_v2()

6. Optimización del Uso de Transacciones
   - Control de transacciones:
     - Mantén las transacciones cortas y claras.
     - Ejemplo:
       BEGIN;
       UPDATE cuentas SET balance = balance - 100 WHERE id = 1;
       COMMIT;

   - Manejo de bloqueos:
     - Evita bloqueos prolongados usando niveles de aislamiento adecuados.
     - Ejemplo:
       SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

7. Uso Eficiente de Tipos de Datos
   - Selección de tipos adecuados:
     - Utiliza tipos de datos que optimicen espacio y rendimiento.
     - Ejemplo:
       CREATE PROCEDURE set_user_age(user_id INT, user_age SMALLINT)

8. Modularidad y Reutilización
   - Encapsulación de lógica repetida:
     - Separa lógica común en funciones reutilizables.
     - Ejemplo:
       CREATE FUNCTION calcular_iva(precio DECIMAL) RETURNS DECIMAL AS $$
       BEGIN
         RETURN precio * 0.16;
       END;
       $$ LANGUAGE plpgsql;

Formato para uso en Prompts:

1. Nombre del procedimiento: create_procedure
   - Descripción: Crea un procedimiento almacenado en PostgreSQL.
   - Ejemplo:
     CREATE PROCEDURE procedure_name(param1 TYPE, param2 TYPE)
     LANGUAGE plpgsql
     AS $$
     BEGIN
       -- Código aquí
     END;
     $$;

2. Validación de parámetros: validate_input
   - Descripción: Valida los parámetros de entrada de un procedimiento.
   - Ejemplo:
     IF input IS NULL THEN
       RAISE EXCEPTION 'El parámetro no puede ser NULL';
     END IF;

Respuesta:

### Assistant:
%2
```

Ahora crear un nuevo chat y seleccionar el modelo `TheBloke/CodeLlama-7B-Instruct-GGUF`. Ingresar el siguiente código de ejemplo de un procedimiento almacenado para PostgreSQL en `Enviar un mensaje...`:

```
CREATE OR REPLACE PROCEDURE delete_transactions(max_records INT) AS $$
DECLARE 
    deleted_event INTEGER := 0;
    deleted_tran INTEGER := 0;
    total_records_deleted INTEGER := 0;
BEGIN
    BEGIN TRANSACTION;
    
    SELECT ID INTO deleted_tran FROM FACTURA WHERE date > CURRENT_DATE - INTERVAL '90 days';
    
    IF NOT FOUND THEN 
        RAISE EXCEPTION 'No se encontraron registros con más de 90 días. Terminando procedimiento.';
    END IF;
    
    FOR rec IN SELECT * FROM deleted_tran LOOP
        EXECUTE 'DELETE FROM FACTURA_DETALLE WHERE FK_FACTURA_ID = $1' INTO deleted_event USING rec.id;
        
        total_records_deleted := total_records_deleted + deleted_event;
    END LOOP;
    
    EXECUTE 'DELETE FROM FACTURA WHERE ID = $1' INTO deleted_tran USING rec.id;
        
    total_records_deleted := total_records_deleted + deleted_tran;
    
    IF total_records_deleted >= max_records THEN 
        -- Confirmar transaccion iniciada en el punto anterior y terminar procedimiento con commit
        COMMIT;
        RETURN QUERY SELECT total_records_deleted;
    ELSE
        ROLLBACK;
        RAISE EXCEPTION 'No se han eliminado suficientes registros.';
    END IF;    

END $$ LANGUAGE plpgsql;
```

La respuesta fue la siguiente:
![image](https://github.com/user-attachments/assets/db6a06eb-2d90-47e0-98c7-0cc718303b03)

# Fuentes consultadas
(Configuring Custom Models)[https://github.com/nomic-ai/gpt4all/wiki/Configuring-Custom-Models]
(The 6 Best LLM Tools To Run Models Locally)[https://medium.com/@amosgyamfi/the-6-best-llm-tools-to-run-models-locally-eedd0f7c2bbd]



