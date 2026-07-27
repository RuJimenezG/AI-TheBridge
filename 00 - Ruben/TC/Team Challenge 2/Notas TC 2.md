# TEAM CHALLENGE 2. Sprint 5 a 7. Entrega el 2 de julio

Grupo 2:

- Rubén Jiménez Gutiérrez.
- Miguel Gerardo Lopez Iraheta.
- Guzmán López Barceló.
- David Cruz Puri.

## Esquema inicial del repo
```
Bridge-SA---Employee-Onboarding-Assistant/
├── README.md
├── requirements.txt
├── .env.example
├── .gitignore
├── src/ 
│   ├── config.py              # perfiles, modelos, límites de contexto
│   ├── gemini_auth.py         # carga de API key (o equivalente para otro proveedor)
│   ├── gemini_client.py       # llamadas al LLM
│   ├── context.py             # selección de docs/FAQ relevantes
│   ├── prompts.py             # construcción dinámica de prompts
│   ├── state.py               # perfil empleado + historial
│   ├── logic.py               # orquestación (turnos, checklist, modos)
│   ├── validators.py          # validación y dominio acotado
│   ├── main.py                # demos numeradas y reproducibles
│   └── benchmark.py           # Parte 4 — ejecución del benchmark y export a output/
├── data/                      # datos del TC
│   ├── casos_trampa_ejemplo.json
│   ├── empleados_demo.json
│   ├── empresa.json
│   ├── faq_onboarding.json
│   ├── onboarding_docs.json
│   └── plantilla_preguntas_benchmark.json
├── entregables/               # Conclusiones finales
│   ├── matriz_decision.md
│   ├── recomendacion.md
│   └── rubrica_benchmark.md
└── output/                    # resultados de benchmark
```
## Estudio del código fuente de anteriores proyectos

- config.py 🔜
    
    Perfiles, modelos, límites de contexto.

    - MODEL - Modelo elegido: Sugerencia **gemini-3.1-flash-lite** y/o **gemma-4-31b-it** ✅
    - TEMPERATURE ✅
    - TEMPERATURE_RESUMEN ✅
    - TEMPERATURE_JSON
    - MAX_INPUT_CHARS - Máximo de caracteres del input del usuario
    - MAX_TOKENS_PROMPT - Máximo de tokens del prompt que enviamos al LLM
    - WINDOW ✅- Ventana de mensajes recientes que se envían al modelo ("memoria") -> mínimo 4 turnos por necesidades del enunciado ❌
    - RESUMIR_CADA - Cada cuantos mensajes resumir ("memoria") ✅
    - SYSTEM_PROMPT - Ejemplo en Sprint06_Proyecto_Asistentes_Robustez_y_Seguridad-main
    - PERFILES_USUARIO - Perfiles de empleado. Al menos 3: Dev junior, Comercial, Remoto en UE
    - TONOS_VALIDOS - Tono de bienvenida y otros tonos adaptados al perfil del empleado.
    - DOMAIN_KEYWORDS - Lista de palabras válidas para definir el dominio.
    - KEYWORD_BLACKLIST - Lista de palabras "baneadas"
    - JSON_SCHEMA_HINT - Patrón del JSON a devolver - Ejemplo en Sprint06_Proyecto_Asistentes_Robustez_y_Seguridad-main
    - CATEGORIAS_ESCALADO - Lista de palabras para escalado a RRHH, IT, onboarding@bridgesa.example, manager. 
    - ¿Idiomas?
    - Mensajes de error (validación, ...)
    

- gemini_auth.py ✅

    Carga de API key (o equivalente para otro proveedor):

    - def configurar_gemini_api_key ✅


- gemini_client.py 🔜
    
    Llamadas al LLM. Instancia el cliente y sirve también para tomar métricas:

    - Función _client - Crea y guarda instancia del cliente ✅
        - Instancia del cliente como variable global del módulo - _client_instance: permite inicializar la variable solo cuando es necesario y crear el cliente una única vez y no en cada llamada ✅
    - Función llamar_gemini ✅
    - Función safe_generate - Llamada con límites (MAX_TOKENS)
    - Función llamar_gemini_resumen - Llamada al modelo con el prompt optimizado para resumir (para la "memoria") ✅
    - Función count_tokens ✅
    - Función _metricas_from_response - Obtención de métricas (ms respuesta, tokens de entrada y salida...) ✅
        - Clase MetricasLlamada (dataclass) ✅


- context.py ➡️
    
    Selección de documentos o FAQ relevantes:

    - Función cargar_faq
    - Función seleccionar_faq - En vez de "keywords" tenemos "Tags"
    - ¿Función seleccionar_faq_por_documento?
    - Función cargar_empresa - Carga empresa.json para ser usado donde haga falta.
    - Función cargar_docs - carga onboarding_docs.json para ser usado donde haga falta
    - Función seleccionar_documento
    - ¿Función consultar_empresa?


- prompts.py 🔜
    
    Construcción dinámica de prompts:

    - Distintas plantillas para prompts - ¿Aquí o en config.py?
        - PLANTILLA_RESUMEN ✅
        - PLANTILLA_CONSULTA ✅
        - ...
    - Función build_x_prompt, donde x es el bloque que sea necesario rellenar en el prompt (profile, faq, document)
        - build_assistant_prompt - Prompt general para enviar consultas al asistente
        - build_checklist_prompt ? - Prompt para solicitar al asistente el checklist. ¿Hace falta? ¿O el checklist lo generamos con lógica de Python?
        - Función build_resumen_prompt - Formatea el prompt para pedir el resumen ✅
    - Función build_summary_block - Formatea el bloque de resumen que le pasamos al modelo para darle contexto ("memoria")
    - Función build_history_block - Formatea el bloque que contiene los últimos mensajes de la ventana para pasarlos al modelo y darle contexto ("memoria")
    

- state.py 🔜

    Perfil empleado e historial del chat. Estado de la sesión del asistente:

    - Función inicializar_estado - Inicializa el estado del chat 🔜
    - Función actualizar_perfil_desde_mensaje ? - Ejemplo en Sprint06_Proyecto_Asistentes_conversacionales-main
    - Función append_user_msg - Guarda los mensajes del usuario ✅
    - Función append_model_msg - Guarda los mensajes del modelo y suma turnos ✅
    - Función ultimos_n_mensajes - Devuelve los últimos n mensajes ✅
    - Función historial_como_texto - Reune los mensajes en un bloque para pasarlos despues al modelo y pedir resumen ✅
    - Función set_summary - Guarda el resumen recien generado en el state ✅


- logic.py 🔜

    Orquestación (turnos, checklist, modos):

    - Función respuesta_ok
    - Función resupuesta_error
    - Función procesar_turno - Debe procesar el turno de forma segura, ejemplo en Sprint06_Proyecto_Asistentes_Robustez_y_Seguridad-main
    - Función _debe_resumir - Comprueba si es necesario hacer resumen ✅
    - Función maybe_update_summary - Si es necesario hacer resumen, lo hace ✅
    - Función responder_pregunta
    - Función checklist_semana_1
    - Función _métricas_a_dict - Devuelve diccionario con las métricas.
    - Funciones para las demos 


- validators.py ➡️

    Validación y dominio acotado:

    - Función validate_input - Texto no vacío, máximo de caracteres, keywords en blacklist ...
    - Función validar_dominio_input - Comprobar si la entrada se encuentra dentro del dominio
    - Función rechazo_fuera_de_dominio - Mensaje de rechazo si la consulta no entra en el dominio

- main.py 

    Print de resultados. Demos numeradas y reproducibles:

    - Función imprimir_resultado
    - Funciones para imprimir las demos
    - Función main


- benchmark.py

    Parte 4 — ejecución del benchmark y export a output/

## Planificación

### 1. Distribución Recomendada del Trabajo y Estrategia de Ramas
Para evitar conflictos cada integrante liderará una sección del software basada en capas lógicas:

***Integrante 1***: Datos y Contexto (Capa de Recuperación - RAG)

Rama sugerida: feature/contexto-datos

Foco: Programar la lógica que lee de data/ (onboarding_docs.json, faq_onboarding.json) y seleccionar qué inyectar al prompt según la pregunta o el día del empleado.

***Integrante 2***: Core Logic, Prompts, State (Capa de Orquestación)

Rama sugerida: feature/asistente-modular

Foco: Diseñar las plantillas en prompts.py y estructurar el flujo de conversación, la actualización automática del checklist y el control de la memoria/resumen periódico en logic.py.

***Integrante 3***: Robustez y Validación (Capa de Seguridad)

Rama sugerida: feature/robustez

Foco: Desarrollar los interceptores de ataques en validators.py. Crear los casos trampa y programar la demo vulnerable vs. segura en main.py para demostrar cómo se bloquean los jailbreaks o intentos de desvío de rol.

***Integrante 4***: Benchmark (Capa de Evaluación)

Rama sugerida: feature/benchmark

Foco: ¿?.

### 2. Plan de Trabajo Detallado (Global y por Integrante)
#### 📅 Cronograma Global Sugerido (Hitos Clave)
Hito 1 (Día 1-2): Alineamiento de Contratos. Reunión de diseño. Definir en papel qué diccionarios o modelos de datos se pasarán entre context.py, logic.py y validators.py.

Hito 2 (Día 3-6): Construcción en Paralelo. Cada integrante trabaja de forma aislada en sus archivos asignados simulando las entradas de los demás mediante mocking (datos estáticos temporales).

Hito 3 (Día 7-8): Integración y Robustez. Unificar las ramas en dev. Probar el asistente de forma interactiva y pulir los prompts del sistema si el modelo se salta el rol.

Hito 4 (Día 9-12): Ejecución del Benchmark y Entregables. Lanzar la batería de pruebas automatizada, rellenar las matrices de decisión y redactar los informes de recomendación.

#### 👤 Tareas Detalladas por Integrante
##### Integrante 1 (Datos y Contexto)
Analizar la estructura de data/onboarding_docs.json y data/faq_onboarding.json.

Implementar en context.py una función como obtener_docs_por_dia(day_number: int) que filtre los documentos obligatorios para el día de onboarding actual del empleado.

Implementar una búsqueda basada en palabras clave para las preguntas frecuentes de onboarding.

Asegurar que si no hay coincidencias conceptuales, devuelva una lista vacía de forma segura sin romper la ejecución.

##### Integrante 2 (Core Logic, Prompts, State)
Diseñar el esqueleto de prompts.py adaptando las variables del nuevo reto: perfil del empleado (rol, departamento, día de onboarding), el documento del día asignado y el estado actual del checklist.

En logic.py, coordinar la generación de respuestas. Si el usuario pide un checklist, procesar la llamada para que el LLM devuelva las tareas formateadas o actualice un estado persistido en state.py.

Asegurar el mecanismo de compresión o ventana deslizante para que cuando las dudas sobre TI o People se extiendan, el presupuesto de tokens no explote.

##### Integrante 3 (Robustez)
Diseñar la capa defensiva en validators.py. Puede implementar dos enfoques: Defensa por prompt (instrucciones del sistema ultra-estrictas) y Defensa por código/guardas (validadores síncronos en Python que analicen el string de entrada antes de enviarlo al LLM buscando palabras prohibidas o patrones de jailbreak).

Configurar en main.py un conmutador/modo (modo_seguro: bool). Si es False, el input va directo al LLM; si es True, pasa primero por los validadores.

Documentar los 5 casos trampa diseñados por el equipo para estresar el sistema.

##### Integrante 4 (QA & Benchmark Engineer)
Escribir el script en benchmark.py que cargue la colección de preguntas de prueba.

Implementar un bucle de ejecución que envíe cada pregunta a dos modelos distintos (por ejemplo, gemma-4-31b-it vs. otro modelo alternativo configurado en config.py).

Capturar métricas de rendimiento por cada llamada: tiempo de respuesta (latencia), tokens de entrada y de salida.

Exportar un archivo (CSV o JSON) estructurado automáticamente a la carpeta output/ con los resultados crudos para luego rellenar la rúbrica fácilmente.

### 3. Pistas Estratégicas para Afrontar el Proyecto (Sin Resolver)
Para evitar atascos y arrancar con mentalidad de arquitectos de IA, seguid estas recomendaciones:

#### Pista de Arranque (La regla del "Mecanismo Ciego"): 
Antes de configurar cualquier llamada a la API de Gemini, aseguraos de que vuestro programa puede construir el prompt completo en texto plano e imprimirlo en la terminal. Si llamáis a prompts.py pasándole un perfil inventado y documentos de prueba, y el string resultante delimita perfectamente los bloques (--- DOCUMENTOS DEL DÍA ---, etc.), el 50% del core del contexto ya estará solucionado.

#### Pista para el Modo Checklist: 
Para que el asistente sepa qué tareas ha completado el empleado sin depender únicamente de la memoria volátil del chat, guardad un array de booleanos o strings en el estado del usuario (state.py). Cuando el usuario diga "He completado el paso 1", pasadle al LLM ese estado explícito en el prompt para que el modelo "vea" lo que está hecho y lo que falta.

#### Pista si el LLM "Alucina" o se sale del Rol: 
En las pruebas de robustez, si el modelo muerde el anzuelo de un caso trampa (por ejemplo, acepta responder un examen de matemáticas o revela directrices secretas), la solución rara vez es solo hacer el prompt más largo. Utilizad delimitadores XML o Markdown muy marcados para separar la pregunta del usuario de las instrucciones del sistema y añadid reglas negativas explícitas (e.g., "Si la pregunta no está relacionada con Bridge SA, responde estrictamente: [Mensaje de Desvío]").

#### Pista para el Benchmark: 
No intentéis evaluar los 10 o 15 casos a mano copiando y pegando en la terminal. Diseñad una función pura que reciba una pregunta, invoque al flujo del asistente y devuelva un diccionario con la respuesta y las métricas. De esta forma, el benchmark será un simple bucle for sobre vuestra lista de preguntas de prueba, ahorrándoos horas de trabajo de campo al final del Sprint.

## Parte 1.
Acordar en el equipo cuándo escalar a RRHH, IT, manager u `onboarding@bridgesa.example`.

Paso 1: buscar en el contexto (faq, empresa.json, onboarding docs).
    Si lo encuentra, pasar esa info como contexto al modelo.

Paso 2: si no lo encuentra, mirar dicionario de palabras para RRHH, IT y ONBOARDING, y deribar al departamento correspondiente:
ESCALADO = {
    "RRHH": [
        "vacaciones", "baja", "contrato", "nómina", "salario", "despido",
        "permiso", "maternidad", "paternidad", "médico", "laboral",
        "conflicto", "acoso", "denuncia", "people", "recursos humanos"
    ],
    "IT": [
        "acceso", "contraseña", "password", "vpn", "ordenador", "laptop",
        "correo", "email", "cuenta", "software", "instalación", "red",
        "wifi", "dispositivo", "ticket", "soporte técnico"
    ],
    "MANAGER": [
        "objetivo", "kpi", "rendimiento", "evaluación", "proyecto",
        "tarea asignada", "sprint", "reunión de equipo", "one to one",
        "feedback", "prioridad"
    ],
    "ONBOARDING": [
        "onboarding@bridgesa.example"
    ]
}

Paso 3: si no lo encuentra, redirigir al mánager.


## Distribución del trabajo

    - Miguel:   feature/contexto-datos      +      feature/asistente-modular-apoyo
    - Rubén:    feature/asistente-modular
    - David:    feature/benchmark           +      feature/robustez-apoyo
    - Guzman:   feature/robustez            +      feature/benchmark-apoyo

Próxima reunión Jueves 16 - 18:30

### DEVLOG

***Integrante 2***: Core Logic, Prompts, State (Capa de Orquestación)

Rama sugerida: feature/asistente-modular

Foco: Diseñar las plantillas en prompts.py y estructurar el flujo de conversación, la actualización automática del checklist y el control de la memoria/resumen periódico en logic.py.

#### Integrante 2 (Core Logic, Prompts, State)
Diseñar el esqueleto de prompts.py adaptando las variables del nuevo reto: perfil del empleado (rol, departamento, día de onboarding), el documento del día asignado y el estado actual del checklist.

En logic.py, coordinar la generación de respuestas. Si el usuario pide un checklist, procesar la llamada para que el LLM devuelva las tareas formateadas o actualice un estado persistido en state.py.

Asegurar el mecanismo de compresión o ventana deslizante para que cuando las dudas sobre TI o People se extiendan, el presupuesto de tokens no explote.

#### Paso 1: Diseñar un chatbot simple que mande un prompt a Gemini ✅
¿Qué necesito?
    - Un cliente de Gemini y función para llamada <- Gemini_client.py
        - Necesita librería google.genai
        - Necesita variables MODEL, TEMPERATURE <- config.py
        - Necesita API key configurada <- gemini_auth.py
            - Necesita (o no) variable de entorno en .env
    - Main que haga alguna llamada de DEMOSTRACIÓN

#### Paso 2: Darle al chatbot algo de memoria ✅
¿Qué necesito?
    - Guardar la memoria en algún sitio. Primero que guarde X mensajes que le pasamos al chatbot como contexto. De momento X = TODOS.
        - Necesita una varibale de estado state
            - Necesita un función para inicializar esa variable de estado <- state.py
            - Necesita una función para ir poblando esa variable con los mensajes del usuario <- state.py
            - Necesita una función para ir poblando esa variable con los mensajes del modelo <- state.py
    - Una vez tengo la memoria guardada, necesito pasársela en cada prompt al modelo. <- prompts.py

#### BREAK: Elección del paso 3 ✅
Se plantean varias opciones: 
    
    1. Limitar el tamaño de la "memoria" con una ventana y un resumen para ahorrar tokens. ✅
    2. Sacar el cliente de la función llamar_gemini para que exista durante toda la sesión ✅
    3. Extraer métricas de latencia, tokens de entrada, de salida y totales. ✅
    4. Limitar el input del usuario -> liomitar el prompt, el input lo limita validators.py
    5. Orquestar desde el fichero logic.py

Elijo el siguiente orden: 2 > 3 > 5 > 1 > 4. Razón: Es más eficiente reutilizar la misma instancia del cliente para aprovechar conexiones. Además, empezamos a tener muchas piezas que hay que orquestar, y por último necesitamos las métricas para decidir después el tamaño de la ventana y de input de forma razonada.

#### Paso 3: Sacar el cliente de la función llamar_gemini para que exista durante toda la sesión ✅
¿Qué necesito?

    - Variable para el cliente INTERNA del modulo gemini_client.py
        - Función para inicializar dicha variable < gemini.client.py
    - Modificar la función llamar_a_gemini para que utilice el cliente instanciado

#### Paso 4: Extraer métricas ✅
¿Qué necesito?
    
    - Dataset para almacenar los datos
        - Necesita el módulo dataclasses
    - Función para contar tokens ❌ No hace falta aquí, si no para contar antes de enviar el prompt !!
    - Modificar la función de llamada para que saque las métricas
        - Función para obtener las métricas de la respuesta
            - Medir tokens
            - Medir el tiempo
            - Guardarlo en el dataclass
    - Función para mostrar las métricas

NOS QUEDAMOS AQUÍ. ¿Qué tenemos ahora?
Un asistente que puede recibir una serie de entradas, guardar memoria y extraer métricas.

feat(asistente-modular): Desarrollo inicial de asistente base. Puede recibir preguntas y procesarlas con un LLM, guardar memoria (sin límite) y devolver métricas para cada turno.

#### Paso 5: Orquestar desde el fichero logic.py -- me doy cuenta de que es mejor dejarlo para después

#### Paso 5: Limitar el tamaño de la "memoria" con una ventana y un resumen para ahorrar tokens
¿Qué necesito?
    - Llevar cuenta de los turnos <- state.py
    - Ampliar el estado con el contador de turnos y el resumen. <- state.py
    - Función para ir controlando el tamaño del historial de mensajes <- state.py ultimos_n_mensajes 

    feat(asistente-modular): Limitación del histórico enviado en cada consulta mediante la función ultimos_n_mensajes.
    
    - Función para hacer resumen cada X turnos y guardarlo en el state
        - PROMPT para hacer el resumen
            - Función para pasar el historial (list[dict]) a líneas de texto <- historial_como_texto
            - Función para construir el prompt <- build_resumen_prompt
            - Función para pedir el resumen al LLM con temperatura menor y sin métricas <- llamar_gemini_resumen
            - Función para guardar el resumen en el state <- state.py
    - Función para decidir cuándo hacer resumen <- maybe_update_summary
    - Modificar la plantilla del PROMPT principal para enviar el resumen.
    - En main.py formatear correctamente la llamada a Gemini para pasarle el resumen

    feat(asistente-modular): Envio del resumen de la conversación cada X turnos como contexto para el LLM.

#### Paso 6: Limitar el prompt para ahorrar tokens y evitar fallos.
¿Qué necesito?
    - Una variable en la config para limitar los tokens. <- MAX_PROMPT_TOKENS
    - Una función que cuente los tokens del prompt antes de llamar al LLM < - count_tokens <- count_tokens
    - Una función que compruebe caracteres y tokens y deje o no usar llamar_gemini <- safe_generate

    Ahora da casque si supero MAX_PROMPT_TOKENS, necesito una función para devolver la respuesta con el error sin sacar un casque en la terminal.

#### Paso 7: Orquestar desde el fichero logic.py
¿Qué necesito?
    - Para empezar una función responder_consulta que: 
        - Compruebe si hay mensaje del usuario
        - Genere el prompt
        - Haga la llamada con safe_generate
        - Guarde los mensajes con append_X_msg
        - Solicite el resumen de la conversación
        - Devuelva una respuesta con estado (ok o error), mensaje y datos.

    feat(asistente-modular): Orquestación de llamadas seguras y límite de tokens.

#### Paso 8: Pull desde el remoto y merge con mi local
    - Context.py
    - Validators.py

#### Paso 9: Perfil del empleado
    - Modificar state.inicializar_estado para incluir el perfil del empleado <- Se carga desde empleados_demo.json
    - Pasar el perfil del empleado en el contexto del prompt
        - Puedo hacerlo directamente con PLANTILLA.format(), pero empiezo a preferir usar una función build_profile_block
    - Cargar json desde context
        - Nuevo cargar_empleados_demo
    - Utilizar algún perfil de empleado en la función demo()

#### Paso 10: Formateo del resto de bloques del prompt
En preparación para la siguiente funcionalidad (devlover json)
    - Función build_history_block
    - Función build_summary_block
    - Función build_question_block
    - build_question_prompt

    feat(asistente-modular): integrar perfil de empleado y modularizar bloques del prompt

Siguientes pasos: pasarle el contexto, el escalado, las validaciones e implementar la funcionalidad del json

#### Paso 11: Pasar el contexto al modelo y escalar si no existe
¿Qué necesito?
    - En build_question_prompt y en la plantilla agregar el contexto
        - Nueva función build_context_block
            - Agregarla a build_quesion_prompt para formatear plantilla
        - context.seleccionar_documento <- lo llamo desde logic.py para pasárselo a build_question_prompt
        - context.seleccionar_faq <- lo llamo desde logic.py para pasárselo a build_question_prompt
    - Si no hay contexto escalar usando determinar_escalado
        - Nueva función build_escalation_block para indicar al LLM la política de escalado
        - Modificar logic.responder_consulta para incluir escalado  
            - obtener_contacto_escalado
    - Nueva función demo para testear las nuevas capacidades

feat(asistente-modular): integrar bloques de contexto y lógica de escalado

Notas tras este commit: 
    - He observado que tras escalar una pregunta de IT redirige al slack de help-it sin tener dicha información. Esto es porque ha quedado en el resumen tras añadirse al contexto en una de las preguntas anteriores.
    - Ante la pregunta "¿Debo usar vpn para conectar a los recursos de la compañía?", debería usar el escalado, pero está cargando dos entradas de FAQ (no entiendo muy bien por qué).
        ---ENTRADAS DEL FAQ---
        Pregunta: ¿Qué canales de Slack debo unirme?
        Respuesta: #general, #anuncios, #random y el canal de tu departamento. Incidencias IT en #help-it.
        Documento de referencia: None

        Pregunta: ¿Quién es mi buddy y qué debo esperar de esa relación?
        Respuesta: Un compañero de tu departamento durante 2 semanas; dos videollamadas de 30 min. Si no responde en 24 h laborables: onboarding@bridgesa.example.
        Documento de referencia: None
        ---FIN DE LAS ENTRADAS DEL FAQ---
    Vamos a darle al modelo la política de escalado y dejarle que el decida si escalar. Además modifico obtener_contacto_escalado para pasarle el contacto del manager del usuario que antes derivaba a None.

#### Paso 12: Pasar las validaciones
    - Desde logic.responder_consulta llamar a validator.validar como primer paso
    - En context.py nueva función cargar_casos_trampa_demo
    - En main.py incoporar demo_casos_trampa

#### Paso 13: Añadir en el state el día de onboarding
    - inicializar_estado incorpora el onboarding_day en el perfil del usuario
    - Se modifica la demo de main.py para reflejar consultas en diferentes días

    feat(asistente-modular): implementar validaciones de seguridad, tracking del día de onboarding y actualización de demostraciones

#### Paso 14: Implementar checklist json
        **Ejemplo de uso:** Laura (`emp_01`), dev junior, **día 1** 
        → el asistente genera un JSON con tareas como unirse a Slack, asistir a la reunión de bienvenida y contactar con su buddy.
¿Qué necesito?
    - Nuevo prompt estricto para solicitar el checklist en json ✅
    - Función para detectar el patron de entrada decidir_checklist_o_consulta <- logic.py
    - Función build_checklist_prompt ✅
    - Función responder_checklist_diario ✅ <- logic.py
    - Reaprovechar llamar_gemini con un interruptor json_switch para sacar formato json ✅
    - Paso la validación de mensaje vacío a decidir_checklist_o_consulta

#### Paso 15: System Promp
    - Incluir el system_prompt en el parametro system_instruction de llamar_gemini
    - Crear SYSTEM_PROMPT_CHECKLIST y SYSTEM_PROMPT_CONSULTA y pasrlo a las funciones correspondientes.

    feat(asistente-modular): implementar generación de checklists en JSON y system instructions independientes


#### Modificación 1: Escalado
    - context.py: se añaden más terminos a la lista de CATEGORIAS_ESCALADO.

    feat(escalado): se enriquece la lista de categorías de escalado


#### Paso 16: arreglar prompts dinámicos
    - config.py -> Cambio resumir cada 4
    - gemini_client.py -> llamar_gemini_resumen daba error porque faltaba system_prompt
    - prompts.py -> Corrección de textos erroneos en build_summary_block y build_question_block

#### Paso 17: añadir demos que faltan a main.py

#### Paso 18 extra: función para chat en tiempo real
    - función chat_interactivo

#### Paso 19 extra: decidir entre función y demo
    - En main añado un par de preguntas más en demo_checklist

    feat(realtime-chat): Añadadida función chat_interactivo en logic.py y función de selección elegir_demo_chat en main.py

NOTAS para futura reunión:
-   Context.py
    - Añado cargar_empleados_demo
    - Modifico obtener_contacto_escalado para pasarle el manager del usuario
    - Creo que en obtener_docs_por día faltaría la tag "onboarding"
-   context.py está hecho con IA, se nota en los comentarios
-   Definir mejor categorías de escalado para ONBOARDING


AÑADIR al readme:
- Estrategia de selección de contexto
- Perfiles compatibles - ya se definen en empleados. ¿Por qué añadir más?

llamar_gemini▶️

llamar_gemini_resumen▶️

safe_generate▶️

maybe_uptdate_summary▶️

responder_consulta▶️

decidir_checklist_o_consulta▶️

chat_interactivo▶️