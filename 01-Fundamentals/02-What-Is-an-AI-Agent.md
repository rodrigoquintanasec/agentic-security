# 🤖 What Is an AI Agent?

> Un **AI Agent** es un sistema de software capaz de interpretar un objetivo, decidir cómo abordarlo, utilizar herramientas, mantener el estado necesario y ejecutar acciones para completar una tarea con un grado determinado de autonomía.
>
> El modelo puede aportar razonamiento y generación. La capacidad agéntica surge del sistema construido alrededor del modelo.

---

## Introducción

El término **AI Agent** se utiliza con creciente frecuencia para describir productos muy diferentes: chatbots, asistentes, copilotos, automatizaciones basadas en Large Language Models, sistemas de investigación, herramientas de programación y plataformas capaces de ejecutar procesos completos.

Sin embargo, no toda aplicación que utiliza un **Large Language Model (LLM)** es un agente.

Una interfaz conversacional no es necesariamente agéntica. Una aplicación que invoca un modelo para resumir un documento tampoco lo es. Incluso un sistema que utiliza varias herramientas puede continuar siendo un workflow determinista si la secuencia de ejecución fue definida completamente por el desarrollador.

La distinción resulta importante porque el nivel de autonomía modifica:

- La arquitectura.
- La superficie de ataque.
- Los activos involucrados.
- Las Trust Boundaries.
- El modelo de identidad.
- Los requisitos de autorización.
- El impacto de una decisión incorrecta.
- Los controles necesarios para operar el sistema de manera segura.

Un sistema que únicamente genera una respuesta presenta riesgos relacionados principalmente con el contenido producido.

Un sistema capaz de seleccionar herramientas, modificar recursos, delegar tareas o persistir información puede producir consecuencias mucho más amplias.

```text
Text Generation
      │
      ▼
Information Impact

Agentic Action
      │
      ▼
System and Business Impact
```

La seguridad de un agente no puede analizarse observando únicamente el modelo.

Debe analizarse el sistema completo que transforma una entrada en una decisión y esa decisión en una acción.

Este documento establece una definición operativa de **AI Agent** que será utilizada en el resto del repositorio.

---

## Objetivo

Al finalizar este capítulo, el lector debería ser capaz de:

- Diferenciar un modelo de una aplicación basada en modelos.
- Distinguir chatbots, asistentes, copilotos, workflows y agentes.
- Reconocer las propiedades que convierten una aplicación en un sistema agéntico.
- Comprender el ciclo operativo de un agente.
- Identificar el papel del modelo, las herramientas, la memoria y la orquestación.
- Analizar distintos grados de autonomía.
- Diferenciar capacidad técnica, autoridad y ejecución autorizada.
- Reconocer cuándo una arquitectura no necesita realmente un agente.
- Identificar las implicancias de seguridad derivadas de cada nivel de autonomía.

---

## El modelo no es el agente

Un **modelo** recibe una entrada y produce una salida.

```text
Input
  │
  ▼
Model
  │
  ▼
Output
```

El modelo puede:

- Interpretar lenguaje.
- Generar contenido.
- Clasificar información.
- Resumir documentos.
- Producir código.
- Proponer decisiones.
- Estructurar información.
- Seleccionar una respuesta probable.

Pero el modelo, de manera aislada, normalmente no controla directamente el entorno en el que opera.

No administra por sí mismo:

- Identidades.
- Tokens.
- Herramientas.
- Memoria persistente.
- APIs.
- Autorizaciones.
- Ejecución de comandos.
- Aprobaciones humanas.
- Registro de acciones.
- Recuperación ante fallos.

Estas capacidades son proporcionadas por el software que rodea al modelo.

```text
┌─────────────────────────────────────────┐
│            Agentic System               │
│                                         │
│   ┌───────────┐      ┌──────────────┐   │
│   │   Model   │      │    Memory    │   │
│   └───────────┘      └──────────────┘   │
│                                         │
│   ┌───────────┐      ┌──────────────┐   │
│   │  Planner  │      │    Tools     │   │
│   └───────────┘      └──────────────┘   │
│                                         │
│   ┌───────────┐      ┌──────────────┐   │
│   │ Policies  │      │ Observability│   │
│   └───────────┘      └──────────────┘   │
└─────────────────────────────────────────┘
```

Por lo tanto:

> **El modelo puede constituir el motor de razonamiento del agente, pero el agente es el sistema completo que transforma ese razonamiento en comportamiento.**

Esta separación será fundamental durante todo el repositorio.

Los controles de seguridad no deben concentrarse únicamente en el modelo. También deben proteger la arquitectura que:

- Construye el contexto.
- Mantiene el estado.
- Expone herramientas.
- Evalúa políticas.
- Autoriza acciones.
- Ejecuta operaciones.
- Registra decisiones.
- Contiene fallos.

---

## Una definición operativa de AI Agent

A efectos de este repositorio, utilizaremos la siguiente definición:

> Un **AI Agent** es un sistema de software que recibe o deriva un objetivo, observa información relevante, selecciona dinámicamente los pasos necesarios, utiliza modelos y herramientas para ejecutar esos pasos, evalúa los resultados y continúa operando hasta completar la tarea, alcanzar una condición de finalización o requerir intervención externa.

Esta definición contiene varios elementos importantes:

1. Existe un objetivo.
2. El sistema interpreta su entorno.
3. Puede seleccionar dinámicamente acciones.
4. Puede utilizar herramientas.
5. Mantiene suficiente estado para continuar una tarea.
6. Evalúa los resultados obtenidos.
7. Puede iterar.
8. Opera con algún grado de autonomía.
9. Sus acciones permanecen sujetas a restricciones.
10. Existe una condición de finalización o control.

No todos los agentes implementan estas propiedades de la misma forma ni con el mismo nivel de autonomía.

Sin embargo, estas características permiten distinguir un sistema agéntico de una aplicación que únicamente invoca un modelo para generar contenido.

---

## Propiedades fundamentales de un agente

No existe una única implementación universal de AI Agent.

Sin embargo, la mayoría de los sistemas agénticos combina varias de las siguientes propiedades.

---

### Objetivo

El sistema recibe una meta que describe el resultado esperado.

```text
"Analizá estos incidentes y prepará un informe."

"Encontrá una fecha disponible y programá la reunión."

"Investigá por qué falló el despliegue y proponé una corrección."
```

El objetivo suele ser más abstracto que una instrucción técnica individual.

Esto obliga al agente a determinar:

- Qué información necesita.
- Qué subtareas debe realizar.
- Qué herramientas debe utilizar.
- En qué orden debería actuar.
- Qué resultados intermedios necesita validar.
- Cuándo considerar terminada la tarea.

Cuanto más abstracto sea el objetivo, mayor será el espacio de decisión disponible para el agente.

Ese espacio adicional puede aportar flexibilidad, pero también incrementa la variabilidad y el riesgo.

---

### Observación

El agente necesita obtener información sobre su entorno.

Las observaciones pueden provenir de:

- Mensajes del usuario.
- Resultados de herramientas.
- APIs.
- Bases de datos.
- Repositorios.
- Sistemas empresariales.
- Documentos.
- Sensores.
- Memoria.
- Otros agentes.

El agente utiliza estas observaciones para actualizar su comprensión del estado actual.

```text
Environment
     │
     ▼
Observation
     │
     ▼
Agent State
```

Desde seguridad, toda observación debe evaluarse según:

- Su origen.
- Su propietario.
- Su nivel de confianza.
- Su integridad.
- Su sensibilidad.
- Su tenant.
- Su antigüedad.
- Su propósito autorizado.

Una observación es información.

No debería convertirse automáticamente en una instrucción.

---

### Razonamiento

El sistema utiliza un modelo para interpretar la situación y determinar posibles cursos de acción.

Esto puede incluir:

- Análisis.
- Clasificación.
- Comparación.
- Priorización.
- Inferencia.
- Selección de herramientas.
- Evaluación de resultados.
- Revisión de un plan.

El razonamiento no equivale a autorización.

El hecho de que el modelo concluya que una acción es conveniente no significa que el sistema deba ejecutarla.

```text
Proposed Action
       ≠
Authorized Action
```

La propuesta puede provenir del modelo.

La autorización debe provenir de políticas y controles independientes.

---

### Planificación

En tareas complejas, el agente puede descomponer el objetivo en subtareas.

```text
Objetivo:
Investigar un incidente de producción.

Plan:
1. Consultar alertas.
2. Recuperar logs.
3. Correlacionar eventos.
4. Identificar el componente afectado.
5. Preparar una hipótesis.
6. Proponer medidas de contención.
7. Generar el informe.
```

Los planes pueden ser:

- Generados completamente al comienzo.
- Construidos paso a paso.
- Modificados según nuevas observaciones.
- Delegados parcialmente a otros agentes.
- Interrumpidos por una política.
- Reiniciados ante un error.

La planificación aumenta la flexibilidad del sistema, pero también introduce nuevas condiciones de fallo.

Un objetivo legítimo puede producir un plan incorrecto.

Un plan correcto puede seleccionar una herramienta inapropiada.

Una tarea intermedia puede exceder el alcance autorizado.

Por lo tanto, cada paso debe evaluarse dentro del contexto de autoridad y riesgo correspondiente.

---

### Acción

Un agente se diferencia especialmente por su capacidad para producir efectos sobre un entorno.

Las acciones pueden incluir:

- Consultar información.
- Crear un archivo.
- Modificar un registro.
- Enviar un mensaje.
- Ejecutar código.
- Actualizar infraestructura.
- Crear una incidencia.
- Programar una reunión.
- Realizar una compra.
- Iniciar una transacción.
- Delegar trabajo.

```text
Reasoning
    │
    ▼
Action Selection
    │
    ▼
Authorization
    │
    ▼
Execution
    │
    ▼
External Side Effect
```

El paso entre selección y ejecución representa uno de los puntos de control más importantes de una arquitectura agéntica.

El agente puede proponer.

La arquitectura debe verificar si puede ejecutar.

---

### Uso de herramientas

Una herramienta proporciona al agente una capacidad externa.

Puede ser:

- Una función local.
- Una API.
- Una consulta SQL.
- Un navegador.
- Un intérprete de código.
- Un sistema de archivos.
- Un servicio empresarial.
- Una integración SaaS.
- Un servidor MCP.
- Otro agente.

Una herramienta define lo que técnicamente puede hacerse.

No define por sí sola:

- Quién está autorizado.
- Qué recursos pueden afectarse.
- Qué parámetros son válidos.
- Qué límites de negocio se aplican.
- Qué acciones requieren aprobación.
- Qué evidencia debe registrarse.
- Durante cuánto tiempo puede utilizarse.
- Si la acción puede delegarse.

```text
Available Tool
      ≠
Authorized Capability
      ≠
Authorized Invocation
```

La autorización debería comprobarse para cada operación sensible, recurso afectado y contexto de ejecución.

---

### Estado

Un agente necesita conservar información suficiente para continuar una tarea.

El estado puede incluir:

- Objetivo actual.
- Plan.
- Subtarea activa.
- Resultados previos.
- Herramientas invocadas.
- Errores.
- Recursos creados.
- Aprobaciones obtenidas.
- Presupuesto restante.
- Condiciones de finalización.

```text
Current State
      +
New Observation
      │
      ▼
Updated State
```

El estado no siempre es memoria de largo plazo.

Puede existir únicamente durante una ejecución y desaparecer al finalizar.

Desde seguridad, el estado debe protegerse porque puede afectar directamente las decisiones futuras del agente.

---

### Memoria

La memoria permite reutilizar información más allá de una interacción inmediata.

Puede clasificarse, de manera práctica, como:

- Memoria de sesión.
- Memoria de trabajo.
- Memoria episódica.
- Memoria semántica.
- Memoria de largo plazo.
- Memoria compartida entre agentes.

La memoria puede almacenar:

- Preferencias.
- Historial.
- Decisiones.
- Resúmenes.
- Hechos.
- Resultados de herramientas.
- Estrategias anteriores.
- Estado de procesos.

Desde seguridad, la memoria introduce preguntas adicionales:

- ¿Quién puede escribirla?
- ¿Quién puede leerla?
- ¿Qué información puede persistir?
- ¿Durante cuánto tiempo?
- ¿Cómo se conserva la procedencia?
- ¿Cómo se elimina?
- ¿Puede un usuario acceder a la memoria de otro?
- ¿Puede contenido no confiable contaminar decisiones futuras?

La memoria no debe considerarse solamente una característica de experiencia de usuario.

Es un activo capaz de influir sobre el comportamiento futuro del sistema.

---

### Iteración

Un agente puede ejecutar un ciclo de:

```text
Observe
   │
   ▼
Reason
   │
   ▼
Plan
   │
   ▼
Act
   │
   ▼
Evaluate
   │
   └───────────────┐
                   ▼
                Continue
```

La iteración permite adaptarse a nueva información.

También puede producir:

- Loops.
- Consumo excesivo.
- Acciones repetidas.
- Acumulación de errores.
- Amplificación de una instrucción manipulada.
- Propagación de estados incorrectos.
- Denial of Wallet.
- Comportamientos difíciles de reproducir.

Todo loop debe disponer de límites claros:

- Máximo de pasos.
- Tiempo máximo.
- Presupuesto.
- Cantidad de llamadas.
- Condiciones de error.
- Criterios de escalamiento.
- Condición de finalización.

---

### Autonomía

La autonomía representa el grado de libertad que posee el sistema para decidir y actuar sin intervención humana directa.

No es una propiedad binaria.

```text
No Autonomy
     │
     ├── Suggestion
     ├── Drafting
     ├── Supervised Execution
     ├── Bounded Autonomy
     └── High Autonomy
```

Un agente puede poseer autonomía alta para buscar información pública y autonomía mínima para ejecutar una transferencia.

Por lo tanto, la autonomía debe analizarse por:

- Acción.
- Recurso.
- Usuario.
- Entorno.
- Impacto.
- Reversibilidad.

---

## El ciclo agéntico

Un modelo simplificado del ciclo de un agente puede representarse de la siguiente manera:

```text
Goal
 │
 ▼
Observe Environment
 │
 ▼
Build or Update Context
 │
 ▼
Reason and Select Next Step
 │
 ▼
Evaluate Policy and Authorization
 │
 ▼
Invoke Tool or Produce Output
 │
 ▼
Observe Result
 │
 ▼
Update State
 │
 ├──────── Goal Incomplete ────────┐
 │                                  │
 └──────── Goal Complete            │
                                    │
                    Repeat ◄────────┘
```

Desde seguridad, los puntos críticos no se limitan a la generación del modelo.

También incluyen:

- Construcción del contexto.
- Selección de la herramienta.
- Evaluación de políticas.
- Autorización.
- Validación de parámetros.
- Ejecución.
- Procesamiento del resultado.
- Actualización de memoria.
- Determinación de finalización.

Un sistema puede proteger correctamente la entrada inicial y fallar en cualquiera de las etapas posteriores.

---

## Model, LLM Application, Workflow y Agent

Para comprender el alcance agéntico conviene diferenciar cuatro niveles.

---

### Model

Un modelo transforma una entrada en una salida.

```text
Input → Model → Output
```

No posee necesariamente:

- Herramientas.
- Memoria.
- Identidad.
- Autoridad.
- Autonomía.
- Capacidad de actuar.

---

### LLM Application

Una aplicación integra uno o más modelos dentro de una funcionalidad.

```text
User
  │
  ▼
Application Logic
  │
  ▼
Model
  │
  ▼
Response
```

La lógica principal continúa controlada por software tradicional.

Ejemplos:

- Resumir documentos.
- Clasificar tickets.
- Generar descripciones.
- Traducir contenido.
- Extraer entidades.

---

### Workflow

Un workflow combina modelos y herramientas siguiendo una secuencia definida por el desarrollador.

```text
Input
  │
  ▼
Classifier
  │
  ├── Route A
  └── Route B
         │
         ▼
       Tool
         │
         ▼
      Response
```

El modelo puede participar en algunas decisiones, pero la estructura general fue predeterminada.

Los workflows suelen ofrecer:

- Mayor predictibilidad.
- Mayor trazabilidad.
- Menor libertad de decisión.
- Pruebas más sencillas.
- Menor superficie de comportamiento.

---

### Agent

El agente decide dinámicamente cómo completar un objetivo dentro de límites establecidos.

```text
Goal
  │
  ▼
Agent
  │
  ├── Selects next step
  ├── Chooses tools
  ├── Evaluates results
  ├── Updates state
  └── Continues or stops
```

El desarrollador define:

- El entorno.
- Las herramientas.
- Las políticas.
- Los límites.
- Las identidades.
- Las condiciones de finalización.

El agente selecciona el camino de ejecución.

---

## Chatbot, Assistant, Copilot, Agent y Agentic System

Estos términos se superponen en la industria y no poseen límites universalmente aceptados.

Por ese motivo, en este repositorio se utilizarán como categorías operativas y no como taxonomías absolutas.

| Sistema | Propósito principal | Selección dinámica de pasos | Herramientas | Estado o memoria | Acciones externas | Autonomía |
|---|---|---:|---:|---:|---:|---:|
| Chatbot | Conversar o responder | Baja | Opcional | Limitado | Normalmente no | Baja |
| Assistant | Ayudar al usuario | Limitada | Opcional | Posible | Limitadas | Baja |
| Copilot | Asistir dentro de un workflow humano | Media | Sí | Posible | Bajo supervisión | Baja/media |
| Agent | Completar objetivos mediante acciones | Alta | Sí | Habitual | Sí | Media/alta |
| Agentic System | Coordinar agentes y componentes | Alta | Sí | Habitual | Sí | Variable |
| Multi-Agent System | Coordinar agentes especializados | Distribuida | Sí | Individual o compartido | Sí | Media/alta |

La interfaz no determina la categoría.

Un sistema puede parecer un chatbot y ejecutar detrás de esa interfaz un workflow agéntico complejo.

Del mismo modo, una herramienta con una interfaz sofisticada puede continuar siendo una automatización determinista.

---

## ¿Qué es un chatbot?

Un chatbot está diseñado principalmente para mantener una interacción conversacional.

Puede:

- Responder preguntas.
- Generar texto.
- Recuperar información.
- Mantener historial.
- Invocar alguna herramienta limitada.

Pero la conversación por sí sola no lo convierte en agente.

```text
Conversation
     ≠
Agency
```

La pregunta clave es:

> ¿El sistema selecciona y ejecuta dinámicamente acciones para perseguir un objetivo, o únicamente genera respuestas dentro de una conversación?

---

## ¿Qué es un assistant?

Un asistente ayuda a una persona a completar tareas.

Puede ofrecer:

- Recomendaciones.
- Resúmenes.
- Borradores.
- Información contextual.
- Acciones limitadas.

El término describe principalmente la relación con el usuario, no una arquitectura técnica específica.

Un asistente puede estar implementado mediante:

- Reglas.
- Workflows.
- Un modelo.
- Un agente.
- Varios agentes.

Por lo tanto:

> **Assistant describe un rol de producto. Agent describe propiedades del sistema.**

---

## ¿Qué es un copilot?

Un copiloto está diseñado para trabajar junto a una persona dentro de una actividad concreta.

Ejemplos:

- Desarrollo.
- Análisis de datos.
- Operaciones.
- Atención al cliente.
- Investigación.
- Administración de sistemas.

Generalmente:

- El humano conserva la iniciativa.
- El sistema propone o prepara acciones.
- El usuario revisa los resultados.
- La ejecución sensible permanece supervisada.

```text
Human
  │
  ├── Defines objective
  ├── Reviews output
  └── Approves action
          │
          ▼
       Copilot
```

El copiloto puede contener capacidades agénticas sin operar con autonomía amplia.

---

## ¿Qué es un Agentic System?

Un **Agentic System** es la arquitectura completa que permite que uno o más agentes operen.

Puede incluir:

- Agentes.
- Modelos.
- Orquestadores.
- Memoria.
- Herramientas.
- RAG.
- Servidores MCP.
- Sistemas de identidad.
- Motores de políticas.
- Sandboxes.
- Flujos de aprobación.
- Observabilidad.
- Mecanismos de recuperación.

```text
Agentic System
│
├── Agent Runtime
│   ├── Model
│   ├── Instructions
│   ├── State
│   └── Planner
│
├── Identity and Policy
├── Memory
├── Tools
├── RAG Sources
├── MCP Servers
├── External Systems
├── Human Oversight
└── Observability
```

El agente es un actor dentro del sistema.

La seguridad debe evaluarse sobre el sistema completo.

---

## ¿Qué es un Multi-Agent System?

Un sistema multiagente contiene varios agentes que colaboran, se supervisan, compiten o se delegan tareas.

```text
                    Orchestrator
                         │
        ┌────────────────┼────────────────┐
        │                │                │
        ▼                ▼                ▼
 Research Agent     Analysis Agent    Action Agent
        │                │                │
        └────────────────┼────────────────┘
                         ▼
                    Shared Result
```

Los agentes pueden especializarse por:

- Dominio.
- Herramienta.
- Privilegio.
- Fuente de datos.
- Tipo de acción.
- Nivel de confianza.

Desde seguridad, un sistema multiagente debe considerar:

- Autenticación entre agentes.
- Autorización.
- Identidad del agente originador.
- Integridad de mensajes.
- Preservación del scope.
- Propagación de restricciones.
- Memoria compartida.
- Agentes comprometidos.
- Delegación circular.
- Cascading failures.
- Accountability.

---

## Agentic Workflow vs AI Agent

No toda solución que utiliza el término *agentic* necesita un agente completamente autónomo.

Un **Agentic Workflow** puede incorporar decisiones de modelos dentro de una estructura controlada.

```text
Developer-Controlled Structure
             +
Model-Based Decisions
             =
Agentic Workflow
```

Ejemplo:

```text
Ticket
  │
  ▼
Model Classification
  │
  ▼
Predefined Routing
  │
  ├── Billing Queue
  ├── Security Queue
  └── Technical Queue
```

Un agente, en cambio, puede decidir dinámicamente:

- Qué información consultar.
- Qué herramientas utilizar.
- Qué subtareas crear.
- Cuándo modificar el plan.
- Cuándo detenerse.

```text
Goal
  │
  ▼
Dynamic Decision Loop
  │
  ├── Observe
  ├── Reason
  ├── Select
  ├── Act
  └── Evaluate
```

Los workflows ofrecen mayor predictibilidad.

Los agentes ofrecen mayor flexibilidad.

La decisión correcta depende del problema.

---

## Autonomy y Agency no son lo mismo

La autonomía es una propiedad importante, pero no constituye el único criterio para identificar un agente.

Un sistema puede realizar automáticamente una secuencia completa sin ser agéntico.

```text
Every night:
1. Export database.
2. Compress backup.
3. Upload file.
4. Delete old copy.
```

Este proceso es autónomo, pero determinista.

No interpreta objetivos ni selecciona dinámicamente su estrategia.

Por el contrario, un sistema puede ser agéntico y requerir aprobación humana antes de cada acción sensible.

```text
Goal
  │
  ▼
Agent creates plan
  │
  ▼
Agent proposes action
  │
  ▼
Human approves
  │
  ▼
System executes
```

Por lo tanto:

> **Autonomy indica cuánto puede actuar sin intervención. Agency indica cuánto puede decidir cómo perseguir un objetivo.**

---

## Espectro de comportamiento agéntico

En lugar de clasificar los sistemas únicamente como “agent” o “not agent”, resulta más útil analizar varias dimensiones.

| Dimensión | Baja | Media | Alta |
|---|---|---|---|
| Goal abstraction | Instrucción concreta | Tarea | Objetivo amplio |
| Step selection | Predefinida | Parcialmente dinámica | Dinámica |
| Tool selection | Fija | Limitada | Abierta dentro del catálogo |
| Iteration | Sin loop | Loop acotado | Loop adaptativo |
| State | Stateless | Sesión | Persistente |
| Delegation | Ninguna | Controlada | Dinámica |
| External impact | Solo lectura | Cambios limitados | Acciones críticas |
| Human oversight | Continua | Por excepción | Limitada |
| Reversibility | Alta | Variable | Potencialmente baja |

Este análisis multidimensional permite estimar con mayor precisión:

- Superficie de ataque.
- Necesidad de supervisión.
- Controles de autorización.
- Requisitos de observabilidad.
- Impacto potencial.
- Complejidad de validación.

---

## ¿Cuándo una aplicación se vuelve realmente agéntica?

Una aplicación comienza a presentar características agénticas cuando varias de las siguientes condiciones aparecen simultáneamente:

- Recibe objetivos en lugar de instrucciones exactas.
- Decide dinámicamente los próximos pasos.
- Selecciona entre múltiples herramientas.
- Evalúa resultados intermedios.
- Modifica su estrategia.
- Mantiene estado.
- Ejecuta acciones externas.
- Opera durante múltiples ciclos.
- Delega tareas.
- Continúa trabajando sin intervención constante.

No existe un umbral universal.

Sin embargo, desde seguridad conviene tratar el sistema como agéntico cuando el modelo puede influir significativamente sobre:

```text
What action is taken
        +
Which tool is used
        +
Which resource is affected
        +
When execution stops
```

Cuanto mayor sea esa influencia, mayor deberá ser la independencia de los controles que validan la acción.

---

## ¿Cuándo no necesitás un agente?

Una de las decisiones de seguridad más efectivas puede ser no utilizar un agente.

No todos los problemas requieren razonamiento abierto, planificación dinámica o autonomía.

Un workflow tradicional suele ser preferible cuando:

- La secuencia es conocida.
- Las reglas son estables.
- El resultado debe ser determinista.
- Las acciones son sensibles.
- La auditoría exige trazabilidad exacta.
- Existe baja tolerancia al error.
- La lógica puede expresarse mediante reglas.
- El costo de una decisión incorrecta es alto.

```text
Known Process
     +
Stable Rules
     +
High Impact
     =
Prefer Deterministic Workflow
```

Un agente puede ser apropiado cuando:

- Los pasos no pueden anticiparse completamente.
- La tarea requiere interpretar información no estructurada.
- Existen múltiples estrategias válidas.
- Es necesario adaptarse a resultados intermedios.
- La tarea tolera variabilidad.
- La flexibilidad produce valor real.

> **La menor autonomía capaz de resolver correctamente el problema suele ser la opción más segura, predecible y fácil de operar.**

---

## Agent as Tool, Delegate or Principal

Un agente puede ocupar diferentes posiciones dentro de un modelo de identidad.

---

### Agent as Tool

El agente opera como una herramienta del usuario.

```text
User
  │
  ▼
Agent
  │
  ▼
Proposed Result
```

El usuario conserva responsabilidad y control sobre la acción final.

---

### Agent as Delegate

El agente recibe autoridad limitada para actuar en nombre del usuario.

```text
User
  │
  ▼
Delegation Grant
  │
  ▼
Agent
  │
  ▼
Scoped Action
```

La delegación debería definir:

- Identidad del delegante.
- Identidad del agente.
- Recursos permitidos.
- Acciones permitidas.
- Propósito.
- Duración.
- Límites.
- Capacidad de revocación.

---

### Agent as Principal

El agente posee identidad propia y actúa bajo políticas asignadas directamente.

```text
Agent Identity
      │
      ▼
Policy Evaluation
      │
      ▼
Authorized Resource
```

Esta modalidad facilita:

- Auditoría.
- Revocación.
- Separación de responsabilidades.
- Atribución.
- Políticas específicas.
- Gestión del ciclo de vida.

---

## Human-in-the-Loop, Human-on-the-Loop y Human-out-of-the-Loop

La participación humana puede adoptar diferentes formas.

---

### Human-in-the-Loop

Una persona interviene directamente antes de una decisión o acción.

```text
Agent Proposal
      │
      ▼
Human Approval
      │
      ▼
Execution
```

Adecuado para:

- Acciones irreversibles.
- Operaciones financieras.
- Modificación de permisos.
- Comunicación externa sensible.
- Cambios en producción.

---

### Human-on-the-Loop

El sistema opera con autonomía, pero una persona supervisa y puede intervenir.

```text
Agent Execution
      │
      ├── Monitoring
      ├── Alerts
      └── Human Intervention
```

Adecuado para tareas repetitivas con:

- Límites claros.
- Buena observabilidad.
- Acciones reversibles.
- Blast Radius reducido.

---

### Human-out-of-the-Loop

El sistema opera sin intervención humana durante la ejecución.

```text
Goal
  │
  ▼
Autonomous Execution
  │
  ▼
Result
```

Solo debería considerarse cuando:

- El riesgo está acotado.
- Los permisos son mínimos.
- Existen límites técnicos fuertes.
- Las acciones son reversibles.
- El comportamiento está ampliamente validado.
- La detección y contención son automáticas.

La presencia de un humano no debe utilizarse como sustituto de controles técnicos.

La supervisión humana debe formar parte de un modelo de Defense in Depth.

---

## La arquitectura real importa más que la etiqueta

La industria todavía utiliza definiciones diferentes para términos como:

- Agent.
- Assistant.
- Agentic Workflow.
- Copilot.
- Autonomous System.

Estas diferencias no deberían impedir el análisis de seguridad.

En lugar de depender únicamente de la etiqueta comercial, conviene preguntar:

- ¿Recibe objetivos abstractos?
- ¿Selecciona dinámicamente los pasos?
- ¿Utiliza herramientas?
- ¿Puede producir efectos externos?
- ¿Mantiene memoria?
- ¿Puede delegar?
- ¿Opera sin supervisión?
- ¿Qué autoridad posee?
- ¿Cómo se limita?
- ¿Cómo se detiene?

La arquitectura real importa más que el nombre del producto.

---

## Architecture Perspective

> **La clasificación correcta no depende de la interfaz. Depende de quién controla la secuencia de decisiones y acciones.**

Consideremos tres sistemas con la misma interfaz conversacional.

### Sistema A

```text
User
  │
  ▼
Model
  │
  ▼
Text Response
```

Es una aplicación LLM conversacional.

### Sistema B

```text
User
  │
  ▼
Model
  │
  ▼
Predefined Workflow
  │
  ▼
Approved Tool
```

Es una aplicación con un workflow asistido por un modelo.

### Sistema C

```text
User Goal
    │
    ▼
Agent
    │
    ├── Builds plan
    ├── Selects tools
    ├── Executes steps
    ├── Evaluates results
    └── Updates strategy
```

Es un sistema agéntico.

Los tres podrían presentarse al usuario mediante una ventana de chat.

Sin embargo, sus modelos de riesgo son completamente diferentes.

---

## Implicancias de seguridad

Cuanto más agéntico sea un sistema, mayor importancia adquieren:

- Identidad propia del agente.
- Autorización por acción.
- Permisos de corta duración.
- Limitación de herramientas.
- Validación de parámetros.
- Separación entre instrucciones y datos.
- Procedencia del contexto.
- Aislamiento de memoria.
- Supervisión humana.
- Trazabilidad.
- Contención.
- Revocación.
- Reversibilidad.
- Límites de costo y ejecución.

```text
Increasing Agency
       │
       ▼
Increasing Decision Freedom
       │
       ▼
Increasing Need for Independent Controls
```

El objetivo no es eliminar toda autonomía.

Es impedir que la autonomía se convierta en autoridad ilimitada.

---

## Pensar como un Agentic Security Engineer

Durante una revisión, un profesional de Agentic Security no se limita a preguntar:

> ¿Utiliza un LLM?

Intenta responder preguntas más amplias.

### Sobre la naturaleza del sistema

- ¿Es un modelo, una aplicación, un workflow o un agente?
- ¿Qué parte del comportamiento es determinista?
- ¿Qué decisiones toma el modelo?
- ¿Qué decisiones toma la aplicación?
- ¿Qué decisiones toma una persona?

### Sobre los objetivos

- ¿Recibe instrucciones concretas u objetivos abstractos?
- ¿Puede reinterpretar el objetivo?
- ¿Puede crear nuevos subobjetivos?
- ¿Quién verifica que el objetivo continúa siendo válido?

### Sobre las acciones

- ¿Puede producir efectos externos?
- ¿Puede realizar cambios o solo consultas?
- ¿Las acciones son reversibles?
- ¿Puede repetirlas?
- ¿Puede seleccionar libremente los parámetros?

### Sobre las herramientas

- ¿Las herramientas son fijas o dinámicas?
- ¿El agente puede descubrir nuevas herramientas?
- ¿Puede encadenarlas?
- ¿Puede delegar su uso?
- ¿Qué control autoriza cada invocación?

### Sobre autonomía y control

- ¿Cuántos pasos puede ejecutar?
- ¿Cuándo debe detenerse?
- ¿Qué acciones requieren aprobación?
- ¿Cómo se revoca su capacidad?
- ¿Qué ocurre si entra en un loop?

### Sobre estado y memoria

- ¿Qué estado mantiene?
- ¿Qué persiste entre sesiones?
- ¿Quién puede modificarlo?
- ¿Cómo se preserva la procedencia?
- ¿Cómo se elimina?

### Sobre responsabilidad

- ¿Actúa como el usuario?
- ¿Posee identidad propia?
- ¿Quién conserva accountability?
- ¿Puede reconstruirse por qué actuó?

Responder estas preguntas permite clasificar correctamente el sistema y determinar qué controles necesita.

---

## Modelo de clasificación recomendado

Antes de evaluar la seguridad de una solución, conviene documentar:

```text
System Type:
Model / LLM Application / Workflow / Agent / Multi-Agent

Goal Abstraction:
Low / Medium / High

Decision Autonomy:
Low / Medium / High

Tool Access:
None / Read-Only / Write / High-Impact

State:
Stateless / Session / Persistent

Delegation:
None / Fixed / Dynamic

Human Oversight:
Continuous / Approval-Based / Exception-Based / None

Action Reversibility:
High / Medium / Low

Permission Scope:
Narrow / Moderate / Broad

Potential Blast Radius:
Local / Application / Tenant / Enterprise / External
```

Esta clasificación proporciona más información de seguridad que utilizar únicamente la etiqueta *AI Agent*.

---

## Anti-patterns conceptuales

### Llamar agente a cualquier aplicación con LLM

Dificulta comprender la autonomía real y puede llevar a aplicar controles incorrectos.

---

### Confundir generación con acción

Una respuesta textual y una operación sobre un sistema externo no poseen el mismo nivel de riesgo.

---

### Asumir que más autonomía significa mayor calidad

La autonomía adicional introduce flexibilidad, pero también aumenta:

- Variabilidad.
- Complejidad.
- Superficie de ataque.
- Dificultad de prueba.
- Riesgo operativo.

---

### Utilizar un agente donde un workflow es suficiente

Incrementa innecesariamente:

- Incertidumbre.
- Costo.
- Latencia.
- Complejidad.
- Dificultad de validación.
- Riesgo operativo.

---

### Considerar que Human-in-the-Loop elimina el riesgo

Una aprobación superficial o desinformada puede convertirse en un control meramente formal.

---

### Confundir memoria con historial

La memoria puede influir en decisiones futuras y debe gestionarse como un activo, no únicamente como una característica de experiencia de usuario.

---

### Evaluar únicamente el modelo

Ignora:

- Identidades.
- Herramientas.
- Políticas.
- Memoria.
- APIs.
- Permisos.
- Efectos externos.

---

## Preguntas para decidir si utilizar un agente

Antes de construir uno, responder:

1. ¿La tarea requiere realmente planificación dinámica?
2. ¿Los pasos pueden definirse previamente?
3. ¿La variabilidad del modelo aporta valor?
4. ¿Cuál sería el impacto de una decisión incorrecta?
5. ¿Puede limitarse la autoridad?
6. ¿Las acciones son reversibles?
7. ¿Existe una alternativa determinista?
8. ¿Puede evaluarse el comportamiento de manera confiable?
9. ¿Puede observarse y auditarse?
10. ¿La organización puede operarlo y responder ante incidentes?

Cuando las respuestas no sean claras, conviene comenzar con un workflow acotado y aumentar la autonomía progresivamente.

---

## Relación con el resto de Fundamentals

Este capítulo establece el vocabulario utilizado por los siguientes documentos.

```text
What Is an AI Agent?
        │
        ├── From LLM Applications to Agentic Systems
        ├── Agentic System Components
        ├── Autonomy, Agency and Delegation
        ├── Agentic Assets
        ├── Agentic Trust Boundaries
        ├── Agentic Threats and Risk
        ├── Agentic Security Objectives
        └── Agentic Security Controls
```

Las distinciones introducidas aquí permitirán determinar:

- Qué activos existen.
- Qué componentes deben aislarse.
- Qué acciones necesitan autorización.
- Qué nivel de supervisión resulta apropiado.
- Qué amenazas pertenecen realmente al dominio agéntico.
- Qué controles deben aplicarse.

---

## Key Takeaways

- Un modelo no es un agente.
- Una aplicación que utiliza un LLM no es necesariamente agéntica.
- Un workflow sigue una secuencia definida; un agente selecciona dinámicamente cómo perseguir un objetivo.
- La interfaz conversacional no determina si un sistema es agéntico.
- Un agente combina objetivos, observaciones, razonamiento, herramientas, estado y acciones.
- Autonomy y Agency son propiedades relacionadas, pero diferentes.
- La capacidad de producir efectos externos cambia significativamente el riesgo.
- Un Assistant describe un rol de producto; un Agent describe características del sistema.
- Un Agentic System incluye la arquitectura completa que rodea y controla a los agentes.
- Los sistemas multiagente introducen relaciones adicionales de identidad, confianza y delegación.
- No todos los problemas necesitan un agente.
- Cuando un workflow determinista resuelve la tarea, suele ser más predecible y fácil de proteger.
- La autonomía debería asignarse progresivamente y de manera proporcional al riesgo.
- La arquitectura real importa más que la etiqueta comercial.
- Cuanto mayor sea la libertad de decisión, mayor deberá ser la independencia de los controles de autorización y ejecución.

---

## Referencias

Este documento sintetiza definiciones y patrones arquitectónicos publicados por fuentes oficiales y organizaciones reconocidas.

La terminología utilizada constituye una definición operativa para este repositorio y no pretende imponer una taxonomía universal.

### OpenAI

- [A Practical Guide to Building AI Agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/)
- [Agents SDK](https://developers.openai.com/api/docs/guides/agents)
- [Building Agents](https://developers.openai.com/tracks/building-agents)

### Anthropic

- [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)
- [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [How We Built Our Multi-Agent Research System](https://www.anthropic.com/engineering/multi-agent-research-system)
- [Writing Effective Tools for AI Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)

### NIST

- [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative)
- [Software and AI Agent Identity and Authorization](https://www.nccoe.nist.gov/projects/software-and-ai-agent-identity-and-authorization)
- [Building Evaluation Probes into Agentic AI](https://www.nist.gov/programs-projects/building-evaluation-probes-agentic-ai)

### OWASP GenAI Security Project

- [Agentic AI — Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [Agentic Threats Navigator](https://genai.owasp.org/resource/owasp-gen-ai-security-project-agentic-threats-navigator/)
- [Securing Agentic Applications Guide](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/)
- [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)

---

> **Un AI Agent no se define solamente por su capacidad de generar una respuesta inteligente.**
>
> **Se define por su capacidad de transformar objetivos en decisiones y decisiones en acciones dentro de un entorno controlado.**
