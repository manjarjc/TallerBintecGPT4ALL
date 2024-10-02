# TallerBintecGPT4ALL
Esta es la guía para el evento Bintec de Bancolombia 2024

# 1. GPT4ALL
Es una aplicación de escritorio creada por NomicAI que permite descargar y ejecutar modelos de lenguaje grande (LLM) en cualquier PC de escritorio o Laptop https://docs.gpt4all.io/index.html

![image](https://github.com/user-attachments/assets/5b6f61d1-131b-4082-b313-4535feae1975)

# 1.1 Ventajas
No requiere acceso a internet No requiere GPU Privacidad de la información Acceso a una amplia variedad de modelos

# 1.2 Desventajas
El prompt debe ser más detallado Hay modelos que requieren 16GB o más de RAM Mas lento para responder

# 1.3 Requerimientos
Contar con un PC con al menos 8GB de RAM y un procesador Intel Corei5.  
Tener instalado GPT4All Link descarga 
Descargar modelos “Llama 3 8B Instruct” y “Phi-3 Mini Instruct”

# 1.4 Configurar GPT4ALL

# 1.4.1 Doc Locales (localdocs)
Por lo general, los LLM se entrenan con una amplia variedad de datos, lo que les proporciona una comprensión general, pero puede dar lugar a lagunas en áreas de conocimiento específicas. A veces, incluso pueden producir información desviada o sesgada, un subproducto del aprendizaje a partir de una web vasta y sin filtrar. Para solucionar este problema, se introdujo el concepto de bases de datos vectoriales. Estas bases de datos almacenan datos en un formato único conocido como «incrustación vectorial» (vector embeddings), que permite a los LLM captar y utilizar la información de forma más contextual y precisa.

«Doc Locales» es una caracteristica que permite guardar los archivos de una carpeta en disco como incrustaciones (embeddings) en una base de datos local que posteriormente utiliza un modelo que se ejecuta en GPT4ALL para responder preguntas sobre los documentos. Esto garantiza la privacidad de los datos. El proceso que consulta la base de datos vectorial para responder preguntas se conoce como RAG (Retrieval-Augmented Generation). Esta es una explicación super sencilla pero suficiente para lo que necesitamos hacer.

![image](https://github.com/user-attachments/assets/1f73fda9-566d-4a10-877b-6145002ebca1)


A partir del momento que se crea una colección, GPT4ALL monitorea la carpeta. Si se eliminan o agregan documentos de la carpeta asociada a la colección ésta se actualiza automáticamente

![image](https://github.com/user-attachments/assets/9718b670-0359-48d0-aca3-81eb90fdf844)



