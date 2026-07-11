# 🧠 Building an Agentic Security Mindset

> **Agentic Security no consiste únicamente en proteger prompts o modelos.**
>
> Consiste en diseñar sistemas capaces de interpretar objetivos, utilizar herramientas, mantener estado, delegar tareas y ejecutar acciones sin exceder la identidad, la autoridad ni el riesgo que la organización está dispuesta a aceptar.

---

## 👋 Introducción

La seguridad de aplicaciones basadas en inteligencia artificial suele comenzar con una conversación sobre **Prompt Injection**, fuga de información, alucinaciones o respuestas inseguras.

Estos problemas son importantes, pero representan solamente una parte del sistema.

Una aplicación generativa tradicional puede recibir una entrada, invocar un modelo y devolver una respuesta. Un sistema agéntico puede ir mucho más lejos:

- Interpretar un objetivo de alto nivel.
- Descomponerlo en tareas.
- Construir y modificar un plan.
- Consultar memoria.
- Recuperar información externa.
- Seleccionar herramientas.
- Ejecutar funciones.
- Interactuar con APIs y sistemas empresariales.
- Delegar subtareas a otros agentes.
- Repetir acciones hasta alcanzar una condición de finalización.

Esta capacidad transforma el problema de seguridad.

Una respuesta incorrecta ya no queda necesariamente limitada a la pantalla. Puede convertirse en una transferencia, un correo electrónico, una modificación de infraestructura, una operación sobre una base de datos o una decisión que afecte un proceso de negocio.

```text
Model Output
     │
     ▼
Agent Decision
     │
     ▼
Tool Invocation
     │
     ▼
External Action
     │
     ▼
Business Impact
```

El riesgo diferencial de los sistemas agénticos no proviene únicamente de aquello que el modelo **genera**.

También proviene de aquello que el sistema puede:

- Interpretar.
- Recordar.
- Planificar.
- Autorizar.
- Delegar.
- Ejecutar.
- Repetir.
- Propagar.

Por este motivo, Agentic Security debe analizar el sistema completo y no solamente el modelo que participa en él.

---

## 🎯 Objetivo

El objetivo de este documento es desarrollar el modelo mental necesario para analizar sistemas agénticos desde una perspectiva de ingeniería de seguridad.

Al finalizar, el lector debería comprender:

- Por qué Agentic Security es más amplia que Prompt Security.
- Qué cambia cuando una aplicación adquiere capacidad de actuar.
- Cómo se relacionan autonomía, capacidad, identidad y autoridad.
- Por qué un modelo no debería funcionar como mecanismo principal de autorización.
- Cómo los datos no confiables pueden influir sobre decisiones y acciones.
- Por qué los controles deben aplicarse fuera y alrededor del modelo.
- Cómo razonar en términos de activos, Trust Boundaries, escenarios de amenaza, riesgo y Blast Radius.
- Qué preguntas debería formular un Agentic Security Engineer durante una revisión de arquitectura.

Este documento no desarrolla todavía amenazas concretas como Prompt Injection, Memory Poisoning o Tool Poisoning. Esos temas se abordarán en sus secciones especializadas.

El propósito inicial es comprender **cómo pensar antes de diseñar o evaluar controles**.

---

## 🏛️ ¿Qué es Agentic Security?

**Agentic Security** es la disciplina de ingeniería orientada a proteger sistemas de inteligencia artificial capaces de perseguir objetivos, mantener estado, utilizar capacidades externas y ejecutar acciones con distintos grados de autonomía.

Su alcance incluye, entre otros:

- El modelo y sus endpoints.
- Los prompts e instrucciones.
- El runtime del agente.
- La memoria.
- El contexto.
- Los mecanismos de planificación.
- Las herramientas.
- Las identidades.
- Los permisos.
- Los datos recuperados.
- Los servidores MCP.
- Las APIs externas.
- Los agentes colaboradores.
- Los mecanismos de aprobación humana.
- Las políticas de ejecución.
- La telemetría y la respuesta ante incidentes.

Por lo tanto, proteger un agente no significa solamente impedir que genere una respuesta no deseada.

También significa verificar:

- Bajo qué identidad opera.
- Quién le otorgó autoridad.
- Qué acciones puede ejecutar.
- Sobre qué recursos puede actuar.
- Durante cuánto tiempo conserva sus permisos.
- Qué datos utiliza para decidir.
- Cómo se valida cada acción.
- Qué decisiones requieren aprobación humana.
- Cómo puede detenerse o revocarse.
- Cómo se reconstruye lo ocurrido.
- Cómo se limita el impacto cuando algo falla.

---

## 🔄 De aplicaciones generativas a sistemas capaces de actuar

En una aplicación LLM simple, el flujo puede representarse de la siguiente manera:

```text
User
  │
  ▼
Prompt
  │
  ▼
Model
  │
  ▼
Response
```

El impacto principal suele encontrarse en el contenido producido:

- Información incorrecta.
- Divulgación de datos.
- Contenido inseguro.
- Incumplimiento de políticas.
- Manipulación de la respuesta.

En un sistema agéntico, el flujo puede ser significativamente más complejo:

```text
User Goal
    │
    ▼
Agent Runtime
    │
    ├── Model
    ├── System Instructions
    ├── Planner
    ├── Context
    ├── Memory
    ├── Policy Layer
    ├── Tool Registry
    ├── RAG Sources
    ├── MCP Servers
    └── Other Agents
             │
             ▼
       External Systems
```

Cada integración incorpora:

- Nuevos activos.
- Nuevas identidades.
- Nuevos permisos.
- Nuevas dependencias.
- Nuevos Trust Boundaries.
- Nuevas rutas de ataque.
- Nuevos posibles efectos secundarios.

El modelo continúa siendo importante, pero deja de ser el único componente relevante.

En muchos sistemas, el mayor riesgo no se encuentra en la generación del modelo, sino en la combinación de:

```text
Untrusted Input
       +
Broad Permissions
       +
Sensitive Tools
       +
Persistent Memory
       +
Insufficient Validation
       =
High-Impact Agentic Risk
```

---

## ⚠️ El modelo no es el sistema de seguridad

Uno de los errores arquitectónicos más importantes consiste en delegar decisiones críticas de seguridad al propio modelo.

Por ejemplo:

```text
"El modelo decidirá si el usuario está autorizado."

"El modelo detectará si la instrucción es peligrosa."

"El modelo evitará utilizar la herramienta incorrectamente."

"El prompt indica que no debe revelar información."
```

Estas decisiones pueden aportar una capa adicional de defensa, pero no deberían reemplazar controles deterministas y verificables.

Un modelo probabilístico no debería ser el único responsable de:

- Autorizar acciones.
- Validar identidad.
- Aplicar segregación entre tenants.
- Proteger secretos.
- Hacer cumplir límites financieros.
- Decidir qué recursos puede modificar un usuario.
- Evitar operaciones destructivas.
- Garantizar la integridad de una transacción.

Las políticas críticas deberían aplicarse mediante componentes diseñados específicamente para ello:

- Identity Providers.
- Policy Engines.
- Authorization Services.
- API Gateways.
- Tool Brokers.
- Sandboxes.
- Schema Validation.
- Transaction Limits.
- Approval Workflows.
- Network and workload controls.

> **El modelo puede proponer una acción.**
>
> **La arquitectura debe decidir si esa acción está permitida.**

---

## 🔐 Capability Is Not Authorization

Este es uno de los principios centrales de todo el repositorio:

> **Que un agente pueda realizar técnicamente una acción no significa que esté autorizado a ejecutarla.**

Una herramienta disponible representa una **capability**.

El permiso para utilizarla representa **authority**.

Una operación concreta permitida dentro de un contexto representa una **authorized action**.

Estos conceptos no deben confundirse.

```text
Technical Capability
          ≠
Granted Permission
          ≠
Delegated Authority
          ≠
Authorized Action
```

Consideremos un agente conectado a una herramienta de correo electrónico.

La herramienta podría permitir:

- Leer mensajes.
- Buscar correos.
- Crear borradores.
- Enviar mensajes.
- Eliminar correos.
- Modificar reglas.
- Descargar archivos adjuntos.

Que la integración soporte todas estas operaciones no significa que un agente deba recibir acceso a todas ellas.

Un agente diseñado para resumir correos probablemente necesite:

```text
Read Messages
Search Messages
```

No necesariamente:

```text
Send Messages
Delete Messages
Modify Mailbox Rules
```

El diseño seguro exige separar:

- Lo que la herramienta puede hacer.
- Lo que el agente necesita hacer.
- Lo que el usuario autorizó.
- Lo que la política permite.
- Lo que se aprueba para esa ejecución particular.

---

## 🧭 User Intent Is Not Sufficient Authorization

Un sistema agéntico interpreta instrucciones humanas expresadas mediante lenguaje natural.

Pero una instrucción no siempre constituye una autorización válida.

```text
User Intent
     ≠
Agent Interpretation
     ≠
Verified Identity
     ≠
Granted Authority
     ≠
Authorized Action
```

Esta diferencia resulta crítica.

Un usuario puede:

- Expresarse de forma ambigua.
- Solicitar una acción para la que no posee permisos.
- Ser engañado por contenido externo.
- Operar desde una sesión comprometida.
- Desconocer el impacto completo de una acción.
- Delegar involuntariamente más autoridad de la necesaria.

A su vez, el agente puede:

- Interpretar incorrectamente el objetivo.
- Elegir una herramienta no prevista.
- Construir parámetros incorrectos.
- Exceder el alcance solicitado.
- Reutilizar información de otra sesión.
- Confundir datos con instrucciones.
- Delegar una tarea sin preservar las restricciones originales.

Por este motivo, la autorización debe verificarse en el momento de ejecutar la acción y sobre el recurso concreto afectado.

No debería inferirse únicamente de una conversación.

---

## 🧩 Instructions and Data Share the Same Channel

En el software tradicional suele existir una separación relativamente clara entre:

- Código.
- Configuración.
- Datos.

En aplicaciones basadas en LLM, instrucciones y datos pueden coexistir dentro del mismo contexto.

Un agente puede procesar simultáneamente:

- System prompts.
- Mensajes del usuario.
- Documentos recuperados.
- Resultados de herramientas.
- Contenido web.
- Memoria histórica.
- Mensajes de otros agentes.
- Metadata operativa.

Desde la perspectiva del modelo, todo esto puede presentarse como tokens dentro de una misma ventana de contexto.

```text
Trusted Instructions
         +
User Input
         +
Retrieved Documents
         +
Tool Output
         +
Agent Memory
         =
Model Context
```

Esto crea un problema de seguridad fundamental:

> **El contenido no confiable puede intentar comportarse como una instrucción.**

Por este motivo, el sistema debe conservar y hacer cumplir distinciones que el modelo por sí solo puede no preservar de manera confiable:

- Origen.
- Propietario.
- Nivel de confianza.
- Tenant.
- Sensibilidad.
- Propósito.
- Autoridad asociada.
- Tiempo de vida.
- Permisos de uso.

Una arquitectura segura no debería tratar todo el contexto como información equivalente.

---

## 🧱 Los Trust Boundaries se multiplican

Los sistemas agénticos suelen atravesar numerosos límites de confianza.

```text
User
  │
  ▼
Agent
  │
  ├── Model Provider
  ├── Memory Store
  ├── Vector Database
  ├── MCP Server
  ├── External API
  ├── Enterprise System
  └── Other Agent
```

Cada conexión puede implicar un cambio de:

- Identidad.
- Autoridad.
- Propiedad.
- Administración.
- Región.
- Tenant.
- Política.
- Nivel de confianza.
- Clasificación de datos.
- Responsabilidad.

Un límite de confianza no es únicamente una frontera de red.

También aparece cuando:

- Un agente utiliza una identidad diferente.
- Los datos pasan a un proveedor externo.
- Una herramienta opera bajo otro dominio administrativo.
- Un agente delega una tarea.
- La información se guarda en memoria persistente.
- Un documento externo ingresa al contexto.
- Una acción afecta recursos de otro usuario o tenant.

El diseño debe identificar estas fronteras explícitamente.

Lo que se considera confiable dentro de un componente no debería heredarse automáticamente en el siguiente.

---

## 🎯 La autonomía es un multiplicador de riesgo

La autonomía no es un valor binario.

Un sistema no es simplemente “autónomo” o “no autónomo”.

Puede poseer diferentes niveles de autonomía según:

- La tarea.
- La herramienta.
- El usuario.
- El entorno.
- El impacto.
- La sensibilidad de los datos.
- La reversibilidad de la acción.

Por ejemplo:

| Acción | Autonomía razonable |
|---|---|
| Resumir un documento público | Alta |
| Crear un borrador de correo | Media/alta |
| Enviar un correo externo | Supervisada |
| Modificar permisos | Baja |
| Ejecutar una transferencia | Aprobación explícita |
| Eliminar infraestructura | Aprobación fuerte y control independiente |

El riesgo aumenta cuando se combinan:

- Alta autonomía.
- Permisos amplios.
- Acciones irreversibles.
- Memoria persistente.
- Datos sensibles.
- Supervisión limitada.
- Delegación automática.
- Baja observabilidad.

```text
Agentic Risk
    increases with
        │
        ├── Autonomy
        ├── Authority
        ├── Action Impact
        ├── Persistence
        ├── Delegation Depth
        └── Blast Radius
```

La autonomía debe asignarse de forma proporcional al riesgo de la acción.

---

## 🛡️ Least Privilege no siempre es suficiente

El principio de **Least Privilege** continúa siendo fundamental, pero en sistemas agénticos conviene complementarlo con **Least Agency**.

### Least Privilege

El agente recibe únicamente los permisos mínimos necesarios.

### Least Agency

El agente recibe únicamente el grado de libertad, capacidad de decisión y autonomía necesarios para completar su tarea.

Un agente puede operar con permisos técnicamente limitados y, aun así, poseer demasiada capacidad de decisión.

Por ejemplo, podría tener permiso para enviar correos únicamente desde una cuenta específica, pero poder elegir libremente:

- Los destinatarios.
- El contenido.
- La cantidad de mensajes.
- La frecuencia.
- Los archivos adjuntos.

Least Agency agrega restricciones sobre:

- Qué decisiones puede tomar.
- Qué herramientas puede seleccionar.
- Qué parámetros puede definir.
- Cuántos pasos puede ejecutar.
- Qué acciones requieren aprobación.
- Qué objetivos puede perseguir.
- Cuándo debe detenerse.

```text
Least Privilege
    limits permissions.

Least Agency
    limits autonomous decision space.
```

Ambos principios deben aplicarse conjuntamente.

---

## 🔁 El riesgo aparece en la composición

Un componente puede parecer seguro de forma aislada y producir un sistema inseguro al combinarse con otros.

Por ejemplo:

```text
Model
  +
Memory
  +
Email Tool
  +
Calendar Tool
  +
Enterprise Search
  +
Broad OAuth Token
  =
New Attack Paths
```

Cada componente puede cumplir sus requisitos individuales y, aun así, la composición puede introducir:

- Confused Deputy scenarios.
- Privilege escalation.
- Cross-tool data leakage.
- Unauthorized delegation.
- Indirect Prompt Injection.
- Context contamination.
- Cascading actions.
- Identity confusion.
- Excessive agency.

Por este motivo, revisar únicamente componentes individuales no resulta suficiente.

La seguridad debe evaluarse sobre:

- El workflow completo.
- La cadena de autoridad.
- Los flujos de datos.
- Las secuencias de herramientas.
- Los estados intermedios.
- Las interacciones entre agentes.
- Los efectos acumulativos.

> **La seguridad del sistema no equivale a la suma de la seguridad de sus componentes.**

---

## 💥 Del error local al impacto sistémico

En una aplicación tradicional, un error puede permanecer limitado al componente donde ocurre.

En un sistema agéntico, una decisión incorrecta puede propagarse.

```text
Poisoned Document
       │
       ▼
Incorrect Retrieval
       │
       ▼
Manipulated Context
       │
       ▼
Incorrect Plan
       │
       ▼
Tool Invocation
       │
       ▼
Persistent Memory Update
       │
       ▼
Future Agent Decisions
```

El impacto puede volverse sistémico cuando:

- La información se guarda como memoria.
- Otros agentes consumen el resultado.
- La acción modifica un sistema compartido.
- La decisión se utiliza como evidencia futura.
- El proceso ejecuta múltiples pasos sin validación.
- La autoridad se delega a otros componentes.

El análisis debe considerar tanto el impacto inmediato como la propagación futura.

---

## 🔍 Security, Safety y Reliability no son equivalentes

Un sistema puede producir daño sin que exista un atacante.

También puede resistir ataques y, aun así, comportarse incorrectamente.

Por ejemplo:

- Una alucinación puede generar una acción peligrosa.
- Una instrucción ambigua puede provocar una operación no deseada.
- Un error de recuperación puede duplicar una transacción.
- Un loop puede consumir recursos indefinidamente.
- Una decisión correcta puede ejecutarse sobre el recurso equivocado.

Esto obliga a analizar varias propiedades:

| Propiedad | Pregunta |
|---|---|
| Security | ¿Puede un actor adversario manipular o comprometer el sistema? |
| Safety | ¿Puede el sistema generar daño incluso sin intención adversaria? |
| Reliability | ¿Se comporta consistentemente bajo condiciones esperadas? |
| Resilience | ¿Puede resistir y recuperarse cuando algo falla? |
| Privacy | ¿Los datos se utilizan y protegen adecuadamente? |
| Trustworthiness | ¿El sistema reúne las propiedades necesarias para ser utilizado responsablemente? |

Agentic Security se concentra en comportamiento adversarial, pero debe integrarse con estas disciplinas.

---

## 🏗️ Los controles deben vivir alrededor del modelo

Los prompts y guardrails pueden aportar valor, pero no deberían representar la única barrera entre una decisión del modelo y una acción crítica.

Una arquitectura robusta incorpora controles en diferentes niveles:

```text
User Request
     │
     ▼
Identity Verification
     │
     ▼
Intent and Scope Validation
     │
     ▼
Agent Planning
     │
     ▼
Policy Evaluation
     │
     ▼
Tool Authorization
     │
     ▼
Parameter Validation
     │
     ▼
Human Approval if Required
     │
     ▼
Constrained Execution
     │
     ▼
Result Validation
     │
     ▼
Audit and Monitoring
```

Los controles deberían ser:

- Independientes del prompt cuando sea posible.
- Deterministas para decisiones críticas.
- Específicos por herramienta.
- Específicos por recurso.
- Específicos por usuario y tenant.
- Observables.
- Revocables.
- Verificables.
- Probados bajo escenarios adversarios.

---

## 👤 Human-in-the-Loop no es un control automático

Incorporar una aprobación humana no garantiza que el sistema sea seguro.

Una aprobación puede fallar si:

- La persona no comprende la acción.
- La interfaz oculta detalles importantes.
- Se presentan demasiadas solicitudes.
- El usuario aprueba por hábito.
- La descripción fue generada por el mismo modelo comprometido.
- No se muestra el recurso exacto afectado.
- No se explica si la acción es reversible.
- La aprobación es demasiado amplia o permanente.

Un buen approval gate debería mostrar:

- La acción concreta.
- El recurso afectado.
- La identidad utilizada.
- Los parámetros relevantes.
- El origen de la solicitud.
- El impacto esperado.
- La reversibilidad.
- El alcance de la autorización.
- El tiempo de validez.

> **Human-in-the-Loop es efectivo solamente cuando el humano dispone de contexto, autoridad y tiempo suficiente para tomar una decisión informada.**

---

## 📝 La observabilidad forma parte del modelo de seguridad

En sistemas capaces de actuar, registrar solamente errores técnicos resulta insuficiente.

Debe ser posible reconstruir:

- Qué objetivo recibió el agente.
- Qué identidad inició la operación.
- Qué contexto fue utilizado.
- Qué documentos se recuperaron.
- Qué plan fue generado.
- Qué herramientas se consideraron.
- Qué políticas fueron evaluadas.
- Qué acción se autorizó.
- Qué parámetros se enviaron.
- Qué aprobación se obtuvo.
- Qué resultado devolvió la herramienta.
- Qué información se guardó en memoria.
- Qué agente recibió una delegación.

La observabilidad debe permitir responder:

```text
Who requested it?
Under which identity?
With what authority?
Using which context?
Why was this action selected?
Which policy allowed it?
What changed?
Can it be reversed?
```

Sin esta evidencia, investigar incidentes, validar controles o asignar accountability resulta extremadamente difícil.

---

## ♻️ Diseñar para contención, revocación y recuperación

Ningún sistema debería asumir que todos sus controles funcionarán siempre.

Un diseño maduro también responde:

- ¿Cómo se detiene el agente?
- ¿Cómo se revocan sus tokens?
- ¿Cómo se deshabilitan herramientas?
- ¿Cómo se aísla una memoria comprometida?
- ¿Cómo se invalidan decisiones derivadas de datos contaminados?
- ¿Cómo se revierte una acción?
- ¿Cómo se limita el Blast Radius?
- ¿Cómo se preserva evidencia?
- ¿Cómo se reanuda la operación de forma segura?

Los controles de recuperación no deben diseñarse después del incidente.

Deben formar parte de la arquitectura inicial.

Ejemplos:

- Kill switches.
- Circuit breakers.
- Credential revocation.
- Tool disablement.
- Transaction rollback.
- Memory quarantine.
- Versioned policies.
- Immutable audit trails.
- Scoped incident containment.
- Safe degradation modes.

---

## 🧠 El modelo mental de Agentic Security

El análisis de seguridad no debería comenzar preguntando:

> **¿Cómo bloqueamos Prompt Injection?**

Debería comenzar mucho antes:

```text
Business Objective
        │
        ▼
Agentic Use Case
        │
        ▼
Assets and Identities
        │
        ▼
Goals, Capabilities and Authority
        │
        ▼
Architecture and Trust Boundaries
        │
        ▼
Threat and Failure Scenarios
        │
        ▼
Likelihood and Impact
        │
        ▼
Security and Safety Objectives
        │
        ▼
Controls and Design Patterns
        │
        ▼
Validation and Red Teaming
        │
        ▼
Monitoring and Incident Response
        │
        ▼
Residual Risk
```

Cada etapa responde una pregunta diferente.

| Etapa | Pregunta |
|---|---|
| Business Objective | ¿Qué resultado necesita la organización? |
| Agentic Use Case | ¿Por qué se necesita un agente? |
| Assets | ¿Qué posee valor? |
| Identities | ¿Quién participa y bajo qué identidad? |
| Capabilities | ¿Qué puede hacer técnicamente el sistema? |
| Authority | ¿Qué tiene permitido hacer? |
| Trust Boundaries | ¿Dónde cambia la confianza o el control? |
| Threat Scenarios | ¿Cómo podría fallar o ser manipulado? |
| Risk | ¿Qué probabilidad e impacto tendría? |
| Objectives | ¿Qué propiedades deben preservarse? |
| Controls | ¿Cómo se reduce el riesgo? |
| Validation | ¿Cómo se comprueba que los controles funcionan? |
| Monitoring | ¿Cómo se detecta comportamiento anómalo? |
| Response | ¿Cómo se contiene y recupera el sistema? |

---

## 🏛️ Architecture Perspective

> **Un sistema agéntico no debería recibir más autoridad que la necesaria para completar la acción actualmente autorizada.**

Evitaría diseños donde:

```text
One Agent
   │
   ├── Reads all enterprise data
   ├── Writes to production databases
   ├── Sends external communications
   ├── Modifies infrastructure
   ├── Creates credentials
   └── Delegates to arbitrary agents
```

Preferiría separar responsabilidades:

```text
Orchestrator
     │
     ├── Read-Only Research Agent
     ├── Drafting Agent
     ├── Transaction Validation Service
     ├── Restricted Tool Broker
     └── Human Approval Gate
```

Esta separación permite:

- Aplicar privilegios específicos.
- Limitar la propagación.
- Auditar decisiones.
- Revocar capacidades individuales.
- Reducir el Blast Radius.
- Introducir controles independientes.
- Evitar que un único compromiso controle todo el workflow.

La arquitectura debe tratar autonomía y autoridad como recursos sensibles.

---

## 💡 Pensar como un Agentic Security Engineer

Cuando un Agentic Security Engineer revisa una solución, no se limita a preguntar:

> **¿Tiene guardrails?**

Analiza cuestiones como:

### Sobre el caso de uso

- ¿Por qué la solución necesita autonomía?
- ¿Una automatización determinista resolvería el problema?
- ¿Qué valor aporta el agente?
- ¿Cuál es el peor resultado posible?

### Sobre los activos

- ¿Qué información procesa?
- ¿Qué secretos puede alcanzar?
- ¿Qué acciones de negocio puede ejecutar?
- ¿Qué memoria conserva?
- ¿Qué evidencia debe preservarse?

### Sobre identidad y autoridad

- ¿Bajo qué identidad opera?
- ¿Actúa como el usuario o como un servicio?
- ¿Cómo obtiene sus permisos?
- ¿Puede delegarlos?
- ¿Cuándo caducan?
- ¿Cómo se revocan?

### Sobre las herramientas

- ¿Qué herramientas están disponibles?
- ¿Cuáles pueden producir efectos externos?
- ¿La autorización se verifica por acción y recurso?
- ¿Los parámetros se validan fuera del modelo?
- ¿Existen límites transaccionales?

### Sobre los datos y el contexto

- ¿Qué entradas son no confiables?
- ¿Se preserva la procedencia?
- ¿Cómo se separan instrucciones y datos?
- ¿Puede un documento modificar el comportamiento?
- ¿Qué información puede persistir en memoria?

### Sobre la supervisión

- ¿Qué decisiones requieren aprobación?
- ¿La persona recibe contexto suficiente?
- ¿Puede detenerse el workflow?
- ¿Las acciones son reversibles?

### Sobre la resiliencia

- ¿Qué ocurre si el modelo falla?
- ¿Qué ocurre si una herramienta es comprometida?
- ¿Qué ocurre si la memoria es contaminada?
- ¿Cómo se limita el Blast Radius?
- ¿Cómo se investiga un incidente?

Estas preguntas transforman la revisión desde una evaluación superficial del modelo hacia un análisis completo del sistema.

---

## ❌ Cambios de mentalidad necesarios

### De Prompt Security a System Security

```text
Antes:
¿Cómo evitamos que el modelo responda algo incorrecto?

Ahora:
¿Cómo impedimos que una decisión incorrecta produzca una acción no autorizada?
```

### De Capability a Explicit Authority

```text
Antes:
La herramienta está disponible.

Ahora:
La acción debe autorizarse para este usuario,
este recurso, este propósito y este momento.
```

### De Guardrails a Defense in Depth

```text
Antes:
El prompt dice que no debe hacerlo.

Ahora:
La arquitectura impide que pueda hacerlo sin autorización.
```

### De Logging a Decision Traceability

```text
Antes:
Registramos errores.

Ahora:
Podemos reconstruir objetivos, contexto,
decisiones, políticas, herramientas y acciones.
```

### De Human Approval a Informed Approval

```text
Antes:
El usuario presiona "Aceptar".

Ahora:
El usuario comprende exactamente qué se ejecutará,
sobre qué recurso, con qué identidad y con qué impacto.
```

### De Least Privilege a Least Agency

```text
Antes:
Reducimos permisos.

Ahora:
Reducimos permisos, autonomía, herramientas,
alcance, duración y capacidad de delegación.
```

### De Prevention a Resilience

```text
Antes:
Intentamos evitar todos los fallos.

Ahora:
Prevenimos, detectamos, contenemos,
revocamos, revertimos y aprendemos.
```

---

## 📌 Principios iniciales

Los siguientes principios guiarán el resto del repositorio:

1. **Capability is not authorization.**
2. **Treat all external content as untrusted.**
3. **Separate instructions from data.**
4. **Do not use the model as the only policy enforcement point.**
5. **Apply Least Privilege and Least Agency.**
6. **Authorize every sensitive action at execution time.**
7. **Preserve identity, scope and provenance across delegation.**
8. **Require informed approval for high-impact or irreversible actions.**
9. **Constrain tools independently of prompts.**
10. **Minimize persistent memory.**
11. **Make decisions and actions observable.**
12. **Design for revocation and containment.**
13. **Prefer reversible actions.**
14. **Limit Blast Radius.**
15. **Validate the complete workflow, not only the model.**
16. **Treat security as a lifecycle responsibility.**
17. **Assume components can be compromised.**
18. **Manage residual risk explicitly.**

Estos principios serán desarrollados en profundidad en:

- [Autonomy, Agency and Delegation](05-Autonomy-Agency-and-Delegation.md)
- [Agentic Trust Boundaries](07-Agentic-Trust-Boundaries.md)
- [Agentic Security Controls](11-Agentic-Security-Controls.md)
- [Agentic Security Engineering Principles](12-Agentic-Security-Engineering-Principles.md)

---

## 🔗 Relación con el resto del repositorio

Este documento establece el modelo mental utilizado por todas las secciones posteriores.

```text
Agentic Security Mindset
        │
        ├── Architecture
        ├── Assets and Trust Boundaries
        ├── Threat Modeling
        ├── Agentic Threats
        ├── Identity and Authorization
        ├── Tool and Action Security
        ├── MCP Security
        ├── Memory and Context Security
        ├── RAG and Data Security
        ├── Multi-Agent Security
        ├── Security Controls
        ├── Secure Design Patterns
        ├── Validation and Red Teaming
        ├── Observability and Detection
        └── Incident Response
```

Los conceptos introducidos aquí aparecerán de forma recurrente:

- **Authority** en Identity and Authorization.
- **Capability restriction** en Tool and Action Security.
- **Provenance** en Memory, RAG y MCP Security.
- **Delegation** en Multi-Agent Security.
- **Blast Radius** en Secure Design Patterns.
- **Traceability** en Observability.
- **Revocation and containment** en Incident Response.
- **Residual Risk** en Governance and Risk.

---

## 📌 Key Takeaways

- Agentic Security protege sistemas completos, no solamente modelos o prompts.
- El riesgo aumenta cuando una salida del modelo puede transformarse en una acción externa.
- La capacidad técnica de una herramienta no constituye autorización.
- La intención expresada por el usuario no reemplaza la verificación de identidad, scope y permisos.
- Los modelos no deberían ser el único mecanismo de autorización o aplicación de políticas.
- Instrucciones y datos pueden coexistir dentro del contexto, por lo que su origen y nivel de confianza deben preservarse.
- La autonomía, la persistencia, la delegación y la irreversibilidad aumentan el riesgo.
- Least Privilege debe complementarse con Least Agency.
- La composición de componentes seguros puede crear workflows inseguros.
- Los controles críticos deben aplicarse fuera y alrededor del modelo.
- La aprobación humana solo es efectiva cuando es informada y específica.
- La observabilidad debe permitir reconstruir objetivos, decisiones, autoridad y acciones.
- El sistema debe diseñarse para contención, revocación, reversión y recuperación.
- El riesgo residual debe analizarse y aceptarse explícitamente.

---

## 📚 Referencias

Este documento sintetiza principios de seguridad, riesgo y diseño derivados de fuentes oficiales y comunitarias reconocidas. No reproduce literalmente ninguna de ellas.

### OWASP GenAI Security Project

- [OWASP Agentic Security Initiative](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- [Agentic AI — Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [Securing Agentic Applications Guide 1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/)
- [Multi-Agentic System Threat Modeling Guide](https://genai.owasp.org/resource/multi-agentic-system-threat-modeling-guide-v1-0/)
- [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)

### NIST

- [Artificial Intelligence Risk Management Framework — AI RMF 1.0](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST AI Resource Center](https://airc.nist.gov/)
- [AI Risks and Trustworthiness](https://airc.nist.gov/airmf-resources/airmf/3-sec-characteristics/)
- [NIST SP 800-218 — Secure Software Development Framework](https://csrc.nist.gov/pubs/sp/800/218/final)
- [NIST SP 800-160 Vol. 1 Rev. 1 — Systems Security Engineering](https://csrc.nist.gov/pubs/sp/800/160/v1/r1/final)

### MITRE

- [MITRE ATLAS](https://atlas.mitre.org/)

### Model Context Protocol

- [MCP Specification](https://modelcontextprotocol.io/specification/)
- [MCP Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)
- [Understanding Authorization in MCP](https://modelcontextprotocol.io/docs/tutorials/security/authorization)

Las fuentes específicas utilizadas para desarrollar amenazas, controles, patrones y técnicas serán citadas también en los documentos correspondientes.

---

> **Un sistema agéntico seguro no es aquel que nunca se equivoca.**
>
> **Es aquel cuya arquitectura limita qué puede hacer, verifica bajo qué identidad y autoridad actúa, preserva la procedencia de sus instrucciones y datos, y permite detectar, contener, revocar o revertir sus acciones cuando algo sale mal.**
