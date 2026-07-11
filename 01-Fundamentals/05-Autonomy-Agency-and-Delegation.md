# 🤖 Autonomy, Agency and Delegation

> Los sistemas agénticos no representan únicamente una nueva forma de utilizar modelos de lenguaje.
>
> Representan una nueva forma de distribuir autoridad dentro de un sistema.
>
> Comprender cómo un agente adquiere capacidades, bajo qué identidad actúa, qué decisiones puede tomar y cómo delega acciones constituye uno de los pilares fundamentales de la seguridad en sistemas agénticos.

---

# 🎯 Objetivo

Comprender cómo los conceptos de **Capability**, **Permission**, **Authority**, **Agency**, **Autonomy** y **Delegation** se relacionan entre sí y por qué representan la base para diseñar sistemas agénticos seguros.

Al finalizar este capítulo el lector debería ser capaz de:

- Diferenciar claramente capacidad, permiso y autoridad.
- Comprender qué significa realmente que un agente tenga *agency*.
- Analizar distintos niveles de autonomía.
- Entender cómo se delega autoridad entre usuarios, agentes y herramientas.
- Identificar riesgos asociados a la delegación de acciones.
- Diseñar arquitecturas donde la autonomía permanezca controlada.
- Aplicar principios como Least Privilege y Progressive Autonomy en sistemas agénticos.

Más importante aún, el lector debería comenzar a analizar un agente desde la misma perspectiva utilizada por un arquitecto de seguridad.

No preguntando:

> *¿Qué puede hacer este agente?*

Sino preguntando:

> **¿Quién le permitió hacerlo, bajo qué autoridad, durante cuánto tiempo y con qué límites?**

---

# 📖 Introducción

Durante décadas, la seguridad de aplicaciones tradicionales estuvo basada principalmente en dos conceptos:

- **Identidad**
- **Autorización**

Un usuario iniciaba sesión.

El sistema verificaba su identidad.

Posteriormente determinaba qué recursos podía utilizar.

La lógica de autorización resultaba relativamente directa.

```text
User
   │
Authenticate
   │
Authorize
   │
Execute
```

La llegada de aplicaciones basadas en **Large Language Models** introdujo un nuevo elemento.

El modelo comenzó a participar en la toma de decisiones.

Inicialmente esas decisiones se limitaban a generar contenido.

Pero los sistemas agénticos modifican completamente ese paradigma.

Hoy un agente puede:

- seleccionar herramientas;
- construir planes;
- consultar múltiples sistemas;
- delegar subtareas;
- ejecutar acciones sobre infraestructura;
- actualizar bases de datos;
- enviar correos;
- interactuar con APIs;
- coordinar otros agentes.

En consecuencia, el problema deja de ser exclusivamente:

> **¿Qué usuario está autenticado?**

Y pasa a convertirse en otra pregunta mucho más compleja.

> **¿Qué parte del proceso de decisión estamos delegando al agente y qué mecanismos continúan bajo control determinista?**

Esta diferencia parece sutil.

En realidad representa uno de los mayores cambios conceptuales introducidos por la inteligencia artificial moderna.

---

# 🏗️ La autoridad ya no pertenece únicamente al usuario

En una aplicación tradicional, la mayoría de las decisiones relevantes eran implementadas directamente por código escrito por desarrolladores.

Por ejemplo.

```text
Usuario
    │
    ▼
Backend
    │
    ▼
Business Logic
    │
    ▼
Database
```

La lógica estaba completamente definida.

Cada paso había sido diseñado previamente.

Los sistemas agénticos introducen un nuevo participante.

```text
Usuario
    │
    ▼
Agent Runtime
    │
    ▼
LLM
    │
    ▼
Plan
    │
    ▼
Tool
    │
    ▼
Sistema externo
```

Ahora existe un componente capaz de decidir dinámicamente:

- qué herramienta utilizar;
- qué información consultar;
- qué orden seguir;
- cuándo finalizar;
- qué acción ejecutar a continuación.

Es importante observar que el agente no crea autoridad por sí mismo.

La autoridad continúa originándose en la organización.

Lo que cambia es **cómo esa autoridad se distribuye dentro de la arquitectura**.

---

# ⚠️ El mayor error conceptual

Uno de los errores más frecuentes consiste en asumir que un agente "tiene permisos".

En realidad, un agente puede disponer de varias propiedades diferentes que suelen confundirse.

Por ejemplo:

- conocer una herramienta;
- poder invocarla;
- estar autorizado para utilizarla;
- decidir cuándo utilizarla;
- ejecutar acciones utilizando credenciales delegadas;
- actuar bajo la identidad de otro principal.

Estas propiedades no representan el mismo concepto.

Sin embargo, muchas publicaciones utilizan indistintamente términos como:

- Capability
- Permission
- Authority
- Agency
- Autonomy

Como si fueran equivalentes.

No lo son.

Cada uno responde una pregunta completamente diferente.

| Concepto | Pregunta que responde |
|----------|-----------------------|
| Capability | ¿Qué sabe hacer el sistema? |
| Permission | ¿Qué tiene permitido hacer? |
| Authority | ¿En nombre de quién puede hacerlo? |
| Agency | ¿Puede decidir por sí mismo cuál acción ejecutar? |
| Autonomy | ¿Cuánto puede actuar sin intervención humana? |

Comprender estas diferencias constituye uno de los pilares de Agentic Security.

---

# 🧩 ¿Por qué estos conceptos suelen confundirse?

Existen varias razones.

La primera es histórica.

Durante muchos años, las aplicaciones tradicionales no necesitaban distinguir entre capacidad y autonomía.

Si una API podía eliminar usuarios, normalmente esa capacidad estaba asociada directamente al backend.

El software ejecutaba exactamente aquello para lo que había sido programado.

No existía un componente capaz de decidir dinámicamente si esa acción debía realizarse.

Con la aparición de agentes, el mismo sistema puede:

- evaluar un objetivo;
- construir un plan;
- seleccionar herramientas;
- decidir el siguiente paso;
- modificar el plan inicial;
- delegar tareas;
- ejecutar acciones.

En consecuencia aparecen nuevas preguntas.

Por ejemplo.

- ¿El agente **puede** utilizar una herramienta?
- ¿El agente **debería** utilizarla?
- ¿Quién decidió otorgarle acceso?
- ¿Con qué límites?
- ¿Puede delegar esa autoridad?
- ¿Puede actuar utilizando la identidad del usuario?
- ¿Puede actuar utilizando su propia identidad?
- ¿Puede actuar cuando el usuario ya no está presente?

Estas preguntas simplemente no existían en gran parte del software tradicional.

---

# 🏛️ La diferencia entre ejecutar y decidir

Existe otra diferencia aún más importante.

En ingeniería de software clásica, el comportamiento del sistema era predominantemente determinista.

El desarrollador escribía una secuencia de instrucciones.

```text
if (balance > amount)
       approveTransfer();
else
       rejectTransfer();
```

El programa ejecutaba exactamente ese flujo.

Un agente funciona de otra manera.

```text
Goal
   │
   ▼
Reason
   │
   ▼
Choose Action
   │
   ▼
Execute
```

La lógica deja de estar completamente especificada por código.

Parte de la decisión pasa a depender del razonamiento del agente.

Precisamente por ello aparecen conceptos como:

- Agency
- Autonomy
- Delegation

No describen capacidades técnicas.

Describen **cómo se distribuye la toma de decisiones dentro del sistema**.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto revisa un sistema tradicional suele formular preguntas como:

- ¿Quién accede a la base de datos?
- ¿Qué permisos posee?
- ¿Qué endpoint utiliza?
- ¿Existe autenticación?

En un sistema agéntico las preguntas cambian.

Por ejemplo:

- ¿Qué decisiones puede tomar el agente sin supervisión?
- ¿Qué herramientas puede seleccionar?
- ¿Qué autoridad recibe?
- ¿Puede actuar fuera del objetivo original?
- ¿Puede delegar trabajo a otro agente?
- ¿Cómo se limita esa delegación?
- ¿Quién conserva la responsabilidad final?
- ¿Cómo se revoca esa autoridad?

Estas preguntas constituyen el punto de partida para comprender la seguridad de cualquier sistema basado en agentes.

Los próximos apartados desarrollarán cada uno de estos conceptos en profundidad, comenzando por una distinción fundamental que suele pasarse por alto incluso en documentación técnica especializada:

> **Capability no es Permission. Permission no es Authority. Authority no es Agency. Y Agency no implica necesariamente Autonomy.**

---

# 🧩 Capability vs Permission vs Authority

Uno de los errores conceptuales más frecuentes al diseñar sistemas agénticos consiste en utilizar indistintamente términos como **Capability**, **Permission** y **Authority**.

En muchas conversaciones técnicas es habitual escuchar afirmaciones como:

- "El agente tiene permisos para borrar archivos."
- "Este agente tiene autoridad sobre la base de datos."
- "Le dimos capacidad para enviar correos."

Aunque parezcan equivalentes, describen propiedades completamente diferentes.

Confundirlas conduce a diseños inseguros, revisiones de arquitectura incompletas y modelos de amenazas incorrectos.

Desde la perspectiva de Agentic Security, comprender estas diferencias resulta tan importante como comprender la diferencia entre autenticación y autorización en una aplicación tradicional.

---

# 🏗️ Una analogía sencilla

Imaginemos un empleado dentro de una empresa.

Ese empleado sabe conducir un automóvil.

Eso representa una **Capability**.

La empresa le entrega un vehículo corporativo para visitar clientes.

Eso representa un **Permission**.

Finalmente, el gerente le autoriza a firmar contratos en representación de la organización.

Eso representa **Authority**.

Observemos que las tres propiedades son independientes.

Una persona puede:

- saber conducir pero no tener permiso para utilizar un vehículo corporativo;
- tener permiso para conducir un vehículo sin poder firmar contratos;
- poseer autoridad para aprobar gastos sin conocer cómo conducir.

Lo mismo ocurre con un agente.

---

# 📖 Capability

Una **Capability** representa aquello que un agente **es capaz de hacer desde un punto de vista funcional**.

Describe habilidades.

No autorización.

No confianza.

No identidad.

Simplemente capacidades técnicas.

Por ejemplo.

Un agente puede ser capaz de:

- utilizar una herramienta;
- resumir documentos;
- traducir idiomas;
- consultar una API;
- generar código;
- analizar imágenes;
- interpretar archivos PDF;
- planificar tareas;
- invocar otro agente;
- consultar una base de conocimiento.

Todas estas representan capacidades.

No implican que el agente deba utilizarlas.

Ni que pueda utilizarlas en cualquier contexto.

---

## Una capacidad no implica autorización

Supongamos un agente con una herramienta denominada:

```text
DeleteCustomer()
```

El hecho de que la herramienta exista dentro del sistema únicamente indica que el agente posee una capacidad potencial.

Todavía desconocemos:

- quién puede utilizarla;
- cuándo;
- sobre qué clientes;
- bajo qué condiciones;
- con qué aprobación;
- utilizando qué identidad.

La capacidad representa únicamente la existencia de una función disponible.

---

## Pensar en capacidades como herramientas

Una buena forma de visualizar una Capability consiste en pensar en una caja de herramientas.

```text
Agent

Capabilities

✓ Read Files

✓ Search Database

✓ Send Email

✓ Execute SQL

✓ Create Ticket

✓ Query CRM
```

La caja de herramientas describe **lo que el agente sabe hacer**.

No describe **qué está autorizado a hacer**.

---

# 🔑 Permission

Una **Permission** representa una autorización específica para utilizar una capacidad sobre determinados recursos.

Mientras que una Capability responde:

> **¿Qué sabe hacer el agente?**

Una Permission responde:

> **¿Qué tiene permitido hacer?**

La diferencia parece pequeña.

En realidad cambia completamente el modelo de seguridad.

---

## Un agente puede tener una capacidad que nunca pueda utilizar

Por ejemplo.

```text
Capability

Delete File
```

Pero las políticas de autorización pueden indicar:

```text
Permission

Read Only
```

En consecuencia.

```text
Capability

✓ Delete File

Permission

✗ Delete File
```

La capacidad existe.

La autorización no.

---

## Las Permissions siempre dependen del contexto

Una Permission rara vez es absoluta.

Generalmente depende de varios factores.

Por ejemplo.

- identidad;
- recurso;
- acción;
- propietario;
- tenant;
- horario;
- clasificación;
- propósito;
- riesgo;
- ubicación;
- estado del workflow.

Por ese motivo, una autorización moderna suele responder preguntas como:

> ¿Puede este agente modificar este recurso, perteneciente a este usuario, en este momento y para este propósito?

No simplemente:

> ¿Puede modificar archivos?

---

## Ejemplo

Un agente de Recursos Humanos puede poseer la capacidad de consultar información de empleados.

Sin embargo.

Las políticas pueden establecer que únicamente pueda consultar empleados pertenecientes a su propia región.

```text
Capability

Read Employee Records

Permission

Read Employees

WHERE Region == LATAM
```

La autorización restringe la capacidad.

No la reemplaza.

---

# 👤 Authority

La **Authority** representa el derecho de actuar **en nombre de alguien**.

Este concepto resulta mucho menos conocido que los anteriores.

Sin embargo, constituye uno de los pilares de la seguridad en sistemas distribuidos.

Mientras que una Permission responde:

> **¿Está permitido realizar esta acción?**

La Authority responde:

> **¿Con qué identidad y bajo qué legitimidad puede realizarse?**

---

## La autoridad siempre posee un origen

Ningún agente crea autoridad por sí mismo.

Siempre la recibe desde alguna fuente.

Por ejemplo.

- un usuario;
- una organización;
- un servicio;
- una política;
- una delegación;
- una identidad de workload.

En consecuencia.

La autoridad siempre puede rastrearse.

```text
Organization

↓

User

↓

Application

↓

Agent

↓

Tool

↓

External API
```

Cada nivel representa una cadena de autoridad.

---

## Authority no significa privilegios ilimitados

Supongamos un asistente personal.

Puede tener autorización para:

- consultar el calendario;
- responder correos;
- programar reuniones.

Eso no significa que pueda:

- despedir empleados;
- modificar nóminas;
- transferir dinero;
- aprobar contratos.

La autoridad siempre posee límites.

---

# 🏛️ Authority vs Permission

Muchas veces ambos conceptos parecen equivalentes.

No lo son.

Imaginemos el siguiente escenario.

Una API verifica correctamente que una operación esté permitida.

La autorización responde:

```text
Permit
```

Todavía falta responder otra pregunta.

> **¿Quién realizará esa operación?**

Puede ejecutarla:

- el usuario;
- un agente;
- un servicio;
- una identidad compartida;
- una cuenta administrativa.

Aunque la acción sea exactamente la misma, el nivel de riesgo cambia completamente.

La identidad utilizada modifica:

- la trazabilidad;
- la responsabilidad;
- el alcance del incidente;
- la posibilidad de revocar permisos;
- el Blast Radius.

Por ello, en sistemas agénticos modernos la autorización y la autoridad deben analizarse conjuntamente.

---

# 🔄 Cómo se relacionan

Una forma sencilla de visualizar estos conceptos consiste en pensar en una secuencia.

```text
Capability

↓

Permission

↓

Authority

↓

Execution
```

O bien.

```text
Can the agent do it?

↓

Is the agent allowed to do it?

↓

On whose behalf will it do it?

↓

Execute
```

Cada pregunta agrega una nueva capa de decisión.

Eliminar cualquiera de ellas simplifica la arquitectura.

Pero también reduce considerablemente su nivel de seguridad.

---

# 💡 Ejemplo completo

Supongamos un agente encargado de gestionar tickets de soporte.

Posee la siguiente herramienta.

```text
Close Ticket
```

### Capability

El agente conoce la herramienta.

```text
✓ Close Ticket
```

---

### Permission

La política indica.

```text
Puede cerrar únicamente tickets pertenecientes a su equipo.
```

---

### Authority

La operación será ejecutada utilizando una identidad de servicio con permisos limitados.

No utilizando la identidad del administrador del sistema.

---

El resultado es una arquitectura mucho más segura.

```text
Capability

↓

Permission

↓

Scoped Authority

↓

Controlled Execution
```

Cada componente cumple una responsabilidad diferente.

---

# ⚠️ Errores comunes

Uno de los patrones inseguros más frecuentes consiste en asumir que, si un agente posee una herramienta, automáticamente debería poder utilizarla.

Esto suele conducir a arquitecturas donde:

- todas las herramientas permanecen disponibles permanentemente;
- todos los agentes utilizan la misma identidad privilegiada;
- no existe separación entre capacidad y autorización;
- las políticas se implementan únicamente mediante prompts;
- las herramientas carecen de validación independiente.

En estos escenarios, cualquier error de razonamiento puede transformarse inmediatamente en una acción sobre sistemas críticos.

---

# 🏛️ Architecture Perspective

> **Las capacidades describen lo que un agente sabe hacer.**

> **Los permisos describen cuándo puede hacerlo.**

> **La autoridad describe en nombre de quién lo hará.**

Los tres conceptos son independientes.

Y precisamente por esa independencia pueden controlarse de manera separada.

Una arquitectura madura evita concentrar estas tres responsabilidades dentro del mismo componente.

En lugar de ello:

- el **Tool Registry** define las capacidades disponibles;
- el **Policy Engine** determina qué acciones están permitidas;
- el **Identity Provider** y los mecanismos de delegación determinan bajo qué autoridad se ejecutarán;
- el **Tool Broker** aplica finalmente la decisión antes de alcanzar el recurso.

Este principio reduce el Blast Radius, mejora la trazabilidad y permite revocar autoridad sin necesidad de modificar las capacidades del agente.

---

# 📌 Key Ideas

Antes de continuar con los conceptos de **Agency** y **Autonomy**, conviene recordar una idea fundamental.

```text
Capability
≠
Permission
≠
Authority
```

Un agente puede:

- poseer una capacidad sin autorización para utilizarla;
- estar autorizado para utilizar una capacidad únicamente bajo determinadas condiciones;
- ejecutar una acción utilizando una autoridad distinta de la identidad del usuario que inició la solicitud.

Comprender esta separación constituye uno de los principios fundamentales del diseño seguro de sistemas agénticos modernos y servirá como base para analizar, en las siguientes secciones, cómo un agente adquiere **Agency**, cómo incrementa su **Autonomy** y cómo la **Delegation** modifica la distribución de autoridad dentro de una arquitectura.


---

# 🤖 ¿Qué es Agency?

Hasta este punto hemos visto que un agente puede poseer capacidades, permisos y autoridad.

Sin embargo, todavía falta comprender un concepto que diferencia radicalmente a los sistemas agénticos del software tradicional:

> **La capacidad de decidir.**

Ese concepto se conoce como **Agency**.

Aunque suele traducirse simplemente como *agencia*, esa traducción no refleja completamente su significado dentro de la ingeniería de sistemas.

Desde la perspectiva de Agentic Security, la **Agency** representa el grado en que un sistema puede seleccionar por sí mismo qué acción ejecutar para alcanzar un objetivo, sin que cada paso haya sido definido explícitamente por un desarrollador.

En otras palabras.

La Agency describe la capacidad de un sistema para **tomar decisiones operativas dentro de un conjunto de restricciones**.

---

# 📖 Agency no significa "hacer cualquier cosa"

Uno de los errores más comunes consiste en asociar la palabra *agency* con libertad absoluta.

No es así.

Un agente nunca debería actuar de manera completamente libre.

Siempre opera dentro de límites definidos por:

- su arquitectura;
- sus herramientas;
- las políticas de seguridad;
- la autoridad delegada;
- los recursos disponibles;
- los objetivos establecidos por el usuario.

La Agency no elimina esos límites.

Simplemente determina cuánto espacio de decisión existe dentro de ellos.

---

# 🏗️ Del software determinista al razonamiento dinámico

En una aplicación tradicional, el flujo de ejecución suele estar completamente definido.

```text
Request
   │
Validate
   │
Business Logic
   │
Database
   │
Response
```

El desarrollador conoce exactamente qué ocurrirá en cada paso.

No existe margen para que el sistema decida una estrategia diferente.

Por el contrario, un sistema agéntico puede enfrentarse a un objetivo abierto.

Por ejemplo.

> "Analizá los registros de los últimos siete días e investigá cualquier actividad que pueda indicar un compromiso de credenciales."

No existe una única secuencia correcta para resolver esta tarea.

El agente deberá decidir, por ejemplo:

- qué fuentes consultar;
- qué herramientas utilizar;
- en qué orden hacerlo;
- cuándo detener la investigación;
- cuándo solicitar información adicional;
- cuándo escalar el incidente.

En este escenario, el desarrollador no programó cada decisión.

Programó un entorno donde el agente puede construir su propio camino para alcanzar el objetivo.

Ese espacio de decisión es precisamente lo que denominamos **Agency**.

---

# 🧠 Agency implica seleccionar, no únicamente ejecutar

Una herramienta no posee Agency.

Una API tampoco.

Una función tradicional simplemente ejecuta instrucciones.

Por ejemplo.

```text
deleteUser(id)
```

La función elimina un usuario.

No decide cuándo hacerlo.

No analiza si resulta conveniente.

No evalúa alternativas.

No modifica su estrategia.

Un agente, en cambio, puede enfrentarse a múltiples caminos posibles.

```text
Goal

↓

Evaluate

↓

Choose Strategy

↓

Select Tool

↓

Execute

↓

Observe

↓

Continue or Stop
```

La ejecución continúa siendo importante.

Pero el elemento distintivo es la selección de la estrategia.

---

# 🎯 Agency siempre necesita un objetivo

Un agente no toma decisiones en el vacío.

Siempre existe un objetivo que orienta su comportamiento.

Por ejemplo.

- responder preguntas;
- investigar una alerta;
- preparar un informe;
- reservar un vuelo;
- actualizar una base de datos;
- clasificar documentos;
- resolver un incidente.

El objetivo funciona como una restricción de alto nivel.

Dentro de ese objetivo, el agente puede decidir cómo avanzar.

Fuera de ese objetivo, cualquier acción representa una desviación.

Por este motivo, uno de los principales desafíos de Agentic Security consiste en garantizar que el agente continúe persiguiendo el objetivo autorizado durante toda la ejecución.

---

# ⚠️ Más Agency implica mayor responsabilidad

La Agency aporta flexibilidad.

Pero también incrementa el riesgo.

Un sistema capaz de decidir entre múltiples estrategias también puede:

- seleccionar una herramienta inadecuada;
- utilizar datos incorrectos;
- interpretar mal el objetivo;
- continuar trabajando cuando debería detenerse;
- combinar herramientas de forma inesperada;
- generar efectos secundarios no previstos.

En consecuencia.

Cuanto mayor sea el espacio de decisión del agente, mayor deberá ser el nivel de supervisión arquitectónica.

La Agency nunca debería crecer más rápido que los controles encargados de gobernarla.

---

# 🧩 Agency no es Autonomy

Estos términos suelen utilizarse como sinónimos.

Sin embargo, representan propiedades distintas.

Un agente puede poseer Agency y, al mismo tiempo, requerir aprobación humana para ejecutar cada acción.

En ese caso.

Puede decidir qué desea hacer.

Pero no puede hacerlo por sí mismo.

Por ejemplo.

```text
Goal

↓

Agent selects best action

↓

Human Approval

↓

Execution
```

El agente posee capacidad de decisión.

Pero no posee libertad para ejecutar.

En cambio.

Un agente completamente autónomo puede:

- seleccionar la acción;
- ejecutarla;
- continuar con la siguiente tarea;
- modificar el plan;
- finalizar el proceso.

Todo ello sin intervención humana.

La diferencia entre ambos conceptos será desarrollada en la próxima sección.

---

# 🏢 La Agency cambia la superficie de ataque

En Application Security tradicional, la mayor parte del análisis gira alrededor de:

- entradas;
- validaciones;
- autenticación;
- autorización;
- lógica de negocio.

Los sistemas agénticos agregan un nuevo elemento.

La lógica de decisión.

Ahora un atacante no necesariamente busca modificar una función.

Puede intentar influir sobre el razonamiento del agente.

Por ejemplo.

- alterando el contexto;
- contaminando la memoria;
- manipulando documentos recuperados mediante RAG;
- modificando descripciones de herramientas;
- introduciendo instrucciones ocultas;
- desviando el objetivo inicial.

En otras palabras.

El atacante intenta modificar **la decisión**, no únicamente **la ejecución**.

Esta diferencia explica por qué técnicas como **Prompt Injection**, **Indirect Prompt Injection** y **Goal Hijacking** resultan especialmente relevantes en sistemas agénticos.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un ingeniero de seguridad analiza un sistema tradicional suele preguntar:

> **¿Qué puede ejecutar este proceso?**

En un sistema agéntico aparece una pregunta anterior.

> **¿Qué decisiones puede tomar este agente antes de ejecutar cualquier acción?**

Responder esta pregunta permite identificar:

- qué grado de Agency posee;
- qué riesgos introduce;
- qué decisiones requieren validación adicional;
- qué acciones deberían permanecer bajo control determinista.

En consecuencia, el objetivo de Agentic Security no consiste en eliminar la Agency.

Consiste en garantizar que esa capacidad de decisión permanezca alineada con los objetivos, las políticas y las restricciones definidas por la organización.

---

# 🏛️ Architecture Perspective

> **La Agency no consiste en ejecutar acciones. Consiste en seleccionar acciones.**

Esta diferencia modifica profundamente el diseño de una arquitectura segura.

Una arquitectura madura evita que el agente decida simultáneamente:

- cuál es el objetivo;
- qué herramientas utilizar;
- qué permisos aplicar;
- qué autoridad emplear;
- si una acción está autorizada.

En lugar de ello, distribuye esas responsabilidades entre componentes especializados.

```text
Goal
   │
   ▼
Agent (Agency)
   │
Proposed Action
   │
   ▼
Policy Engine
   │
Authorization
   │
   ▼
Tool Broker
   │
Controlled Execution
```

El agente continúa tomando decisiones.

Pero la arquitectura conserva el control sobre aquellas que poseen consecuencias de seguridad.

---

# 📌 Cierre de la primera sección

Hasta este punto hemos construido las bases necesarias para comprender cómo se distribuye la autoridad dentro de un sistema agéntico.

Hemos visto que:

- una **Capability** describe lo que un agente es técnicamente capaz de hacer;
- una **Permission** determina qué acciones tiene permitidas;
- una **Authority** establece bajo qué identidad y legitimidad actuará;
- la **Agency** define cuánto puede decidir por sí mismo antes de ejecutar una acción.

Todavía falta responder una pregunta fundamental.

> **¿Cuánto puede actuar un agente sin intervención humana?**

Responder esa pregunta nos llevará al siguiente concepto: **Autonomy**.

Comprender la diferencia entre **Agency** y **Autonomy** constituye uno de los pilares para diseñar agentes capaces de colaborar con las personas sin perder el control sobre sus acciones.

---
---
---
---


---

# 🧭 What is Autonomy?

Hasta este punto hemos analizado cómo un agente adquiere capacidades, permisos, autoridad y la capacidad de tomar decisiones dentro de un objetivo determinado.

Sin embargo, todavía queda responder una pregunta fundamental.

> **¿Cuánto puede actuar un agente sin intervención humana?**

Responder esta pregunta nos lleva a uno de los conceptos más importantes —y también más malinterpretados— de los sistemas agénticos modernos: **Autonomy**.

Mientras que la **Agency** describe la capacidad de un agente para seleccionar acciones y construir estrategias, la **Autonomy** describe el grado en que esas decisiones pueden ejecutarse sin supervisión externa.

Aunque ambos conceptos suelen utilizarse como sinónimos en artículos, conferencias e incluso documentación técnica, representan propiedades diferentes del sistema y tienen implicancias muy distintas desde la perspectiva de la seguridad.

Comprender correctamente esta diferencia permite diseñar agentes más seguros, definir límites claros para su comportamiento y evitar que la capacidad de decisión se transforme en una pérdida de control.

---

# 🎯 ¿Qué es Autonomy?

Desde la perspectiva de Agentic Security, la **Autonomy** representa el grado de independencia operativa que posee un agente para ejecutar acciones y avanzar hacia un objetivo sin requerir intervención humana durante cada paso del proceso.

En otras palabras.

Mientras la Agency responde:

> **¿Puede el agente decidir cuál es la mejor acción?**

La Autonomy responde:

> **¿Puede ejecutar esa decisión por sí mismo?**

Esta diferencia parece pequeña.

En realidad, cambia completamente el modelo de seguridad de un sistema.

Un agente puede ser extremadamente inteligente y capaz de planificar una estrategia compleja.

Sin embargo, si necesita aprobación humana antes de ejecutar cada acción, su nivel de autonomía continúa siendo reducido.

Por el contrario, un agente que puede ejecutar herramientas, modificar información y continuar avanzando sin intervención posee un grado mucho mayor de autonomía, incluso si sus capacidades de razonamiento son más limitadas.

---

# 🧠 Autonomía no significa independencia absoluta

Uno de los errores más frecuentes consiste en asociar la autonomía con la idea de un sistema completamente libre.

Desde una perspectiva de ingeniería, esa interpretación resulta incorrecta.

Ningún sistema empresarial debería operar completamente al margen de restricciones.

Incluso un agente altamente autónomo continúa sujeto a diferentes mecanismos de control.

Por ejemplo:

- políticas de autorización;
- límites de tiempo;
- límites de presupuesto;
- restricciones de herramientas;
- controles de red;
- aprobaciones para operaciones críticas;
- límites de recursos;
- políticas regulatorias;
- mecanismos de auditoría;
- procedimientos de revocación.

En consecuencia, la autonomía nunca elimina los controles de seguridad.

Simplemente reduce la cantidad de intervenciones humanas necesarias durante la ejecución de una tarea.

---

# 🏗️ Pensar en autonomía como un continuo

Tradicionalmente, muchas personas clasifican los sistemas utilizando únicamente dos categorías.

```text
Autónomo

No autónomo
```

Este enfoque resulta demasiado simplista para describir arquitecturas modernas.

En realidad, la autonomía representa un **continuo**, donde diferentes componentes del sistema pueden poseer distintos niveles de independencia.

Por ejemplo.

Un agente podría:

- decidir qué documentos consultar;
- seleccionar qué herramienta utilizar;
- resumir información automáticamente;

pero requerir aprobación humana antes de:

- eliminar registros;
- ejecutar comandos administrativos;
- realizar transferencias financieras;
- modificar infraestructura crítica.

En este escenario, el agente no es completamente autónomo ni completamente manual.

Posee una autonomía parcial, cuidadosamente limitada por la arquitectura.

Esta idea será desarrollada con mayor profundidad en la próxima sección dedicada a los **Degrees of Autonomy**.

---

# 📦 La autonomía depende del contexto

Un aspecto importante de la autonomía es que no representa una propiedad fija del agente.

El mismo agente puede operar con distintos niveles de autonomía dependiendo del contexto en el que se encuentre.

Por ejemplo.

Durante un entorno de desarrollo puede tener permitido:

- crear recursos temporales;
- ejecutar pruebas;
- modificar configuraciones de laboratorio.

Sin embargo, exactamente el mismo agente podría requerir aprobación humana para realizar esas mismas acciones en un entorno de producción.

Del mismo modo, una tarea de bajo impacto, como clasificar documentos, puede ejecutarse completamente de forma automática.

En cambio, una operación relacionada con información financiera o infraestructura crítica probablemente deba incorporar mecanismos adicionales de supervisión.

Esto demuestra que la autonomía no depende únicamente de las capacidades del agente.

Depende también del riesgo asociado a la tarea que intenta realizar.

---

# 🎯 La autonomía siempre debe estar alineada con el riesgo

Uno de los principios más importantes de Agentic Security consiste en evitar que todos los agentes posean el mismo nivel de autonomía.

La autonomía debe ser proporcional al impacto potencial de las acciones que el agente puede ejecutar.

Por ejemplo.

Un agente encargado de resumir documentación técnica puede operar prácticamente sin supervisión.

En cambio, un agente capaz de modificar reglas de firewall, aprobar pagos o desplegar infraestructura debería encontrarse sujeto a controles considerablemente más estrictos.

Esta relación puede resumirse de la siguiente manera.

```text
Mayor impacto

↓

Mayor supervisión

↓

Menor autonomía
```

Y, de forma inversa.

```text
Menor impacto

↓

Menor necesidad de supervisión

↓

Mayor autonomía posible
```

Este principio aparece de manera consistente en marcos modernos de gestión del riesgo y constituye una extensión natural de conceptos como **Least Privilege**, **Zero Trust** y **Defense in Depth** aplicados al mundo de los agentes inteligentes.

---

# ⚠️ La autonomía amplía la superficie de riesgo

Cada decisión que deja de requerir intervención humana representa también una nueva responsabilidad para la arquitectura.

Cuando un agente adquiere mayor autonomía, aparecen preguntas adicionales.

Por ejemplo.

- ¿Cómo se limita su alcance?
- ¿Qué ocurre si interpreta incorrectamente un objetivo?
- ¿Cómo se revoca una ejecución en curso?
- ¿Qué sucede si una herramienta devuelve información inesperada?
- ¿Cómo se evita que continúe actuando sobre datos incorrectos?
- ¿Quién responde por las acciones realizadas?
- ¿Cómo puede reconstruirse toda la secuencia de decisiones?

Estas preguntas muestran que aumentar la autonomía no consiste únicamente en permitir que el agente haga más cosas.

Implica diseñar mecanismos capaces de mantener el control incluso cuando el ser humano deja de intervenir en cada paso.

En consecuencia, el crecimiento de la autonomía siempre debe ir acompañado por un fortalecimiento equivalente de los controles de seguridad.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza un sistema tradicional suele preguntar:

> **¿Qué permisos posee este proceso?**

En un sistema agéntico aparece una pregunta diferente.

> **¿Hasta qué punto puede seguir actuando este agente una vez que comenzó la ejecución?**

Responder esta pregunta permite comprender:

- el verdadero nivel de autonomía del sistema;
- los mecanismos de supervisión existentes;
- los límites de actuación;
- los posibles escenarios de pérdida de control.

La autonomía no debería medirse únicamente por la cantidad de acciones que un agente puede ejecutar.

Debe evaluarse considerando también:

- las restricciones que permanecen activas;
- la facilidad para detener la ejecución;
- la posibilidad de auditar sus decisiones;
- la capacidad de revocar autoridad cuando sea necesario.

Desde la perspectiva de Agentic Security, un agente verdaderamente seguro no es aquel que posee la mayor autonomía.

Es aquel cuya autonomía permanece cuidadosamente gobernada por la arquitectura.

---
---
---
---

---

# 🔄 Agency vs Autonomy

Hasta este punto hemos introducido dos conceptos que suelen aparecer juntos en prácticamente toda conversación sobre sistemas agénticos.

- **Agency**
- **Autonomy**

En muchos artículos técnicos, publicaciones comerciales e incluso presentaciones de productos, ambos términos suelen utilizarse como si describieran exactamente la misma propiedad.

Sin embargo, desde una perspectiva de ingeniería, representan conceptos diferentes.

Comprender esta diferencia constituye uno de los primeros pasos para diseñar arquitecturas seguras.

Mientras que la **Agency** describe la capacidad del agente para **tomar decisiones**, la **Autonomy** describe el grado en que esas decisiones pueden **ejecutarse sin intervención humana**.

Aunque ambos conceptos están relacionados, ninguno implica necesariamente al otro.

---

# 🧠 Agency responde "¿Qué decisión tomar?"

La Agency aparece cuando el sistema deja de ejecutar únicamente una secuencia fija de instrucciones y comienza a evaluar alternativas.

Un agente con Agency puede, por ejemplo:

- interpretar un objetivo;
- construir un plan;
- elegir entre distintas herramientas;
- decidir el orden de ejecución;
- adaptar su estrategia según nueva información;
- detener una tarea cuando considera que fue completada.

En otras palabras.

La Agency introduce un componente de razonamiento.

El sistema ya no ejecuta únicamente instrucciones escritas por un desarrollador.

También participa en la construcción de la estrategia necesaria para alcanzar un objetivo.

---

# 🚀 Autonomy responde "¿Puede ejecutar esa decisión?"

Una vez que el agente tomó una decisión aparece una segunda pregunta.

> **¿Puede ejecutarla inmediatamente?**

Si la respuesta es sí, el nivel de autonomía aumenta.

Si la respuesta es no y requiere aprobación humana, validación externa o una política adicional, la autonomía disminuye.

La Agency ocurre antes de la ejecución.

La Autonomy determina qué sucede después de esa decisión.

---

# 🏗️ Un agente puede tener mucha Agency y poca Autonomy

Imaginemos un asistente de infraestructura encargado de administrar servidores.

El agente recibe el siguiente objetivo.

> "Investigar por qué el servidor dejó de responder."

El agente puede:

- analizar métricas;
- revisar logs;
- consultar documentación;
- generar hipótesis;
- identificar el servicio afectado;
- proponer reiniciar el servidor.

Todo esto requiere una importante capacidad de razonamiento.

Sin embargo.

Antes de reiniciar el servidor aparece una ventana de aprobación.

```text
Goal

↓

Reason

↓

Choose Action

↓

Human Approval

↓

Execute
```

El agente posee una Agency elevada.

Pero su Autonomy continúa siendo reducida.

---

# 🏗️ Un agente puede tener poca Agency y mucha Autonomy

También puede ocurrir exactamente lo contrario.

Supongamos un agente extremadamente simple cuya única función consiste en ejecutar automáticamente un backup cada noche.

No construye planes.

No evalúa alternativas.

No interpreta objetivos complejos.

Simplemente ejecuta siempre la misma secuencia.

```text
02:00

↓

Run Backup

↓

Verify

↓

Store Logs
```

Su Agency es prácticamente inexistente.

Sin embargo.

Su Autonomy es muy alta porque ejecuta la tarea sin intervención humana.

Este ejemplo demuestra que ambos conceptos son independientes.

---

# 📊 Comparación

| Agency | Autonomy |
|---------|----------|
| Describe la capacidad de decidir. | Describe la capacidad de actuar sin supervisión. |
| Se relaciona con el razonamiento. | Se relaciona con la ejecución. |
| Evalúa alternativas. | Ejecuta alternativas elegidas. |
| Construye estrategias. | Lleva esas estrategias a la práctica. |
| Puede existir sin ejecución automática. | Puede existir incluso con razonamiento mínimo. |
| Incrementa la complejidad del proceso de decisión. | Incrementa la independencia operativa. |

Una forma sencilla de recordarlo consiste en pensar que:

> **La Agency decide.**

> **La Autonomy actúa.**

---

# 🧩 Ambas propiedades pueden combinarse

Si representáramos ambos conceptos sobre dos ejes independientes, obtendríamos cuatro escenarios posibles.

```text
                   AUTONOMY

              Baja            Alta

Agency
Alta      Analiza pero     Agente avanzado
          requiere         capaz de decidir
          aprobación       y ejecutar

Baja      Automatización   Automatización
          manual           completamente
                           automática
```

Cada cuadrante presenta riesgos diferentes.

No existe un cuadrante universalmente correcto.

Todo depende del contexto del negocio y del nivel de riesgo aceptable.

---

# ⚠️ Errores comunes

La confusión entre Agency y Autonomy suele conducir a decisiones de diseño incorrectas.

Uno de los errores más frecuentes consiste en asumir que reducir la autonomía elimina automáticamente los riesgos.

No es cierto.

Un agente que requiere aprobación para cada acción puede seguir tomando decisiones incorrectas.

Simplemente necesitará que un humano las apruebe.

Del mismo modo.

Incrementar la autonomía tampoco implica necesariamente que el agente sea más inteligente.

Puede ejecutar automáticamente una tarea muy simple sin poseer prácticamente ninguna capacidad de razonamiento.

Otro error frecuente consiste en utilizar prompts como único mecanismo para limitar la autonomía.

Las instrucciones pueden orientar el comportamiento del modelo.

Sin embargo.

No reemplazan controles técnicos como:

- autorización;
- políticas;
- aislamiento;
- límites de herramientas;
- validación de parámetros;
- aprobación humana;
- observabilidad.

La autonomía debe gobernarse mediante arquitectura.

No únicamente mediante instrucciones al modelo.

---

# 🔒 ¿Por qué la Autonomy cambia el modelo de seguridad?

La seguridad tradicional estaba orientada principalmente a controlar quién podía acceder a un recurso.

La pregunta típica era:

> **¿Está autorizado este usuario?**

Los sistemas agénticos agregan una nueva dimensión.

Ahora también debemos responder:

> **¿Cuánto puede seguir actuando este agente una vez autorizada la primera acción?**

Esta diferencia tiene consecuencias importantes.

Cuando aumenta la autonomía aparecen nuevos desafíos relacionados con:

- propagación de errores;
- ejecución encadenada de herramientas;
- acciones irreversibles;
- cambios de contexto;
- desviaciones del objetivo original;
- uso prolongado de credenciales;
- dificultad para detener procesos en curso.

En consecuencia.

El problema deja de ser únicamente controlar el acceso.

También pasa a consistir en controlar la continuidad de la ejecución.

---

# 🏛️ Architecture Perspective

Una arquitectura madura procura separar claramente el razonamiento del mecanismo que autoriza y ejecuta acciones.

```text
Goal
   │
   ▼
Agent
(Agency)
   │
   ▼
Proposed Action
   │
   ▼
Policy Engine
   │
   ▼
Human Approval (when required)
   │
   ▼
Tool Broker
   │
   ▼
Execution
```

En este modelo.

El agente conserva la capacidad de decidir.

Pero la arquitectura mantiene el control sobre la ejecución.

Este principio reduce significativamente el riesgo de que una decisión incorrecta produzca inmediatamente consecuencias sobre sistemas críticos.

La autonomía deja de depender únicamente del comportamiento del modelo y pasa a estar gobernada por mecanismos independientes, verificables y auditables.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un ingeniero de seguridad evalúa un sistema agéntico no debería preguntar únicamente:

> **¿Qué tan inteligente es este agente?**

También debería preguntarse:

- ¿Qué decisiones puede tomar por sí mismo?
- ¿Cuáles requieren aprobación?
- ¿Puede modificar su propio plan?
- ¿Puede continuar actuando durante horas?
- ¿Puede invocar nuevas herramientas?
- ¿Puede iniciar otros agentes?
- ¿Puede aumentar su propia autoridad?
- ¿Qué mecanismo puede detenerlo inmediatamente?

Responder estas preguntas permite comprender el verdadero nivel de autonomía de una arquitectura.

La inteligencia del modelo constituye únicamente una parte del sistema.

La seguridad depende de cómo esa inteligencia interactúa con las políticas, las identidades, las herramientas y los mecanismos de control.

---

# 📌 Cierre de la sección

Hasta este punto hemos diferenciado dos propiedades fundamentales de cualquier sistema agéntico.

La **Agency** determina la capacidad del agente para seleccionar estrategias y tomar decisiones.

La **Autonomy** determina el grado en que esas decisiones pueden ejecutarse sin intervención humana.

Comprender ambas resulta esencial para diseñar arquitecturas seguras.

Sin embargo.

La autonomía no representa una propiedad binaria.

Un agente no es simplemente "autónomo" o "no autónomo".

Existen múltiples niveles de autonomía, cada uno con diferentes beneficios, riesgos y mecanismos de supervisión.

Precisamente esos niveles serán desarrollados en la siguiente sección dedicada a los **Degrees of Autonomy**.

---
---
---
---

---

# 📊 Degrees of Autonomy

Hasta este punto hemos definido qué es la **Autonomy** y cómo se diferencia de la **Agency**.

Sin embargo, todavía queda resolver una idea que suele simplificarse en exceso.

Con frecuencia se habla de agentes "autónomos" como si la autonomía fuera una propiedad binaria.

Es decir.

```text
Autonomous

or

Not Autonomous
```

Desde una perspectiva de ingeniería, esta clasificación resulta insuficiente.

La autonomía no representa un estado.

Representa un **espectro**.

Comprender ese espectro constituye uno de los principios más importantes para diseñar sistemas agénticos seguros.

---

# 🧠 ¿Por qué la autonomía no es binaria?

Pensemos por un momento en un automóvil.

No todos los vehículos poseen exactamente el mismo nivel de automatización.

Algunos únicamente ayudan al conductor a mantenerse dentro del carril.

Otros controlan automáticamente la velocidad.

Otros pueden estacionarse solos.

Y algunos prototipos incluso pueden desplazarse sin intervención humana bajo determinadas condiciones.

Todos poseen cierto grado de autonomía.

Pero claramente no poseen el mismo nivel.

Con los agentes ocurre exactamente lo mismo.

Dos sistemas pueden utilizar el mismo modelo de lenguaje y, sin embargo, diferir enormemente en la cantidad de decisiones que pueden ejecutar sin supervisión.

Por ese motivo, clasificar un agente simplemente como "autónomo" deja de ser útil cuando comenzamos a diseñar arquitecturas reales.

---

# 🏗️ La autonomía es un continuo

En lugar de pensar en dos estados, resulta mucho más útil imaginar una escala de independencia operativa.

```text
No Autonomy
      │
      ▼
Assisted
      │
      ▼
Supervised
      │
      ▼
Semi-Autonomous
      │
      ▼
Highly Autonomous
      │
      ▼
Fully Autonomous
```

Cada incremento dentro de esa escala modifica la distribución de responsabilidades entre:

- el usuario;
- el agente;
- la arquitectura;
- los mecanismos de control.

A medida que aumenta la autonomía, el sistema deja de depender progresivamente de decisiones humanas durante la ejecución.

Como consecuencia, la arquitectura debe asumir nuevas responsabilidades relacionadas con:

- supervisión;
- contención;
- autorización;
- observabilidad;
- recuperación;
- auditoría.

En otras palabras.

La autonomía nunca puede analizarse de forma aislada.

Siempre debe evaluarse junto con los controles que la gobiernan.

---

# 🎯 La autonomía no depende únicamente del modelo

Uno de los errores más frecuentes consiste en asumir que un modelo más avanzado implica automáticamente un agente más autónomo.

Esto no es cierto.

La autonomía no depende exclusivamente de las capacidades del modelo.

Depende principalmente del diseño de la arquitectura.

Por ejemplo.

Dos organizaciones pueden utilizar exactamente el mismo modelo de lenguaje.

Sin embargo.

La primera arquitectura puede obligar al agente a solicitar aprobación humana antes de cada acción.

La segunda puede permitir que el agente:

- consulte sistemas internos;
- invoque herramientas;
- modifique registros;
- ejecute múltiples acciones consecutivas.

El modelo es idéntico.

La autonomía no.

Esto demuestra que la autonomía constituye una propiedad del **sistema**, no únicamente del modelo.

---

# 🧩 Múltiples dimensiones de autonomía

Otro aspecto importante consiste en comprender que la autonomía no afecta únicamente a la ejecución.

Un agente puede poseer distintos grados de independencia según la actividad que esté realizando.

Por ejemplo.

Puede tener autonomía para:

- planificar tareas.

Pero no para:

- ejecutarlas.

O bien.

Puede ejecutar herramientas automáticamente.

Pero no crear nuevas tareas.

También puede:

- consultar información libremente;

pero requerir autorización para modificar datos.

Incluso puede generar recomendaciones completamente autónomas mientras mantiene restricciones estrictas sobre cualquier acción que produzca efectos permanentes.

Por este motivo, la autonomía debería analizarse considerando distintas dimensiones del comportamiento del agente.

Algunas de las más habituales son:

- autonomía para planificar;
- autonomía para seleccionar herramientas;
- autonomía para ejecutar acciones;
- autonomía para persistir información;
- autonomía para delegar trabajo;
- autonomía para modificar objetivos;
- autonomía para interactuar con otros agentes.

Cada una introduce riesgos diferentes y, por lo tanto, requiere mecanismos de control específicos.

---

# 🏛️ Más autonomía implica una arquitectura diferente

En sistemas tradicionales, muchas decisiones permanecen concentradas en el usuario.

El software simplemente ejecuta aquello que el usuario solicita.

```text
User

↓

Action

↓

Application

↓

Result
```

En un sistema agéntico la situación cambia.

```text
User

↓

Goal

↓

Agent

↓

Plan

↓

Multiple Actions

↓

Result
```

La diferencia fundamental consiste en que el usuario ya no define cada paso.

Define un objetivo.

El agente completa el resto.

A medida que aumenta la autonomía, el número de decisiones tomadas automáticamente también aumenta.

Como consecuencia.

La arquitectura debe incorporar nuevos mecanismos para garantizar que esas decisiones permanezcan dentro del comportamiento esperado.

---

# ⚠️ La autonomía aumenta la responsabilidad del diseño

Permitir que un agente actúe de forma más independiente no consiste únicamente en eliminar aprobaciones humanas.

Significa aceptar que determinadas decisiones dejarán de ser revisadas manualmente.

Por ello, cada incremento de autonomía debería ir acompañado por preguntas como:

- ¿Qué ocurre si el agente interpreta incorrectamente el objetivo?
- ¿Qué herramientas puede utilizar?
- ¿Puede detenerse inmediatamente?
- ¿Qué acciones son reversibles?
- ¿Qué límites continúan activos durante toda la ejecución?
- ¿Cómo se detecta un comportamiento inesperado?
- ¿Quién conserva la responsabilidad final?

Estas preguntas no aparecen cuando el agente únicamente genera texto.

Comienzan a ser críticas cuando el agente puede actuar sobre sistemas reales.

---

# 💡 Pensar como un Agentic Security Engineer

Una pregunta frecuente en proyectos de IA consiste en:

> **¿Queremos que el agente sea completamente autónomo?**

Desde la perspectiva de Agentic Security, esa pregunta suele ser incorrecta.

La pregunta adecuada sería:

> **¿Qué nivel de autonomía necesita realmente esta tarea y qué controles deben acompañarlo?**

La autonomía no representa un objetivo.

Representa una decisión de diseño.

Y como cualquier decisión de arquitectura, debe justificarse en función del riesgo, del impacto sobre el negocio y de la capacidad de gobernar el comportamiento del sistema.

Precisamente por ello, antes de hablar de mecanismos de supervisión o delegación, resulta necesario comprender los distintos **niveles de autonomía** que puede adoptar un agente.

En las siguientes secciones analizaremos una clasificación práctica que permite evaluar cómo evoluciona la independencia operativa de un agente desde sistemas completamente asistidos hasta arquitecturas capaces de ejecutar múltiples acciones de forma autónoma.

---
---
---
---

---

# 📈 A Practical Autonomy Model

Aunque actualmente no existe un estándar universal que defina niveles oficiales de autonomía para agentes de IA, distintas organizaciones como **NIST**, **OWASP**, **OpenAI**, **Microsoft**, **Anthropic** y **Google** coinciden en una idea fundamental:

> **La autonomía debe entenderse como un continuo y no como una propiedad binaria.**

Con el objetivo de facilitar el análisis de arquitecturas agénticas, en este repositorio utilizaremos un modelo práctico compuesto por seis niveles de autonomía.

Este modelo no pretende reemplazar ningún estándar oficial.

Su propósito consiste en proporcionar un lenguaje común para evaluar cuánto poder de decisión y ejecución posee un agente dentro de un sistema.

Cada nivel incrementa gradualmente:

- la independencia operativa;
- la complejidad arquitectónica;
- la necesidad de mecanismos de supervisión;
- el impacto potencial de un error.

A medida que aumenta la autonomía, también aumenta la responsabilidad de la arquitectura para mantener el sistema bajo control.

---

# 🟢 Level 0 — No Autonomy

En este nivel el agente no ejecuta ninguna acción.

Su función consiste únicamente en generar información para un usuario.

Por ejemplo.

- responder preguntas;
- resumir documentos;
- traducir texto;
- explicar código;
- generar documentación.

El usuario continúa tomando absolutamente todas las decisiones.

```text
User

↓

Question

↓

Agent

↓

Answer

↓

User decides what to do
```

El agente no interactúa con herramientas.

No modifica información.

No ejecuta procesos.

No posee autoridad operativa.

Su participación termina cuando genera la respuesta.

---

## Riesgos principales

Aunque el nivel de autonomía es mínimo, todavía existen riesgos relevantes.

Por ejemplo.

- alucinaciones;
- información incorrecta;
- sesgos;
- exposición de datos sensibles;
- generación de contenido inseguro.

En este nivel los riesgos afectan principalmente la calidad de la información.

Todavía no existen consecuencias directas sobre sistemas externos.

---

## Controles habituales

Los controles suelen centrarse en:

- protección del contexto;
- filtrado de información;
- validación humana;
- clasificación de datos;
- guardrails de entrada y salida;
- observabilidad.

---

# 🟡 Level 1 — Assisted Autonomy

En este nivel el agente comienza a participar activamente en la toma de decisiones.

Sin embargo.

Continúa siendo el usuario quien ejecuta las acciones.

Por ejemplo.

El agente puede:

- investigar un incidente;
- proponer consultas SQL;
- recomendar configuraciones;
- generar comandos;
- preparar respuestas;
- sugerir cambios de infraestructura.

Pero el usuario decide si esas acciones serán ejecutadas.

```text
Goal

↓

Agent Analysis

↓

Recommendation

↓

Human Decision

↓

Execution
```

El agente deja de limitarse a responder preguntas.

Comienza a colaborar activamente con el usuario.

---

## Riesgos principales

En este nivel aparece un nuevo desafío.

Las recomendaciones incorrectas pueden ser ejecutadas por personas que confían excesivamente en el agente.

Este fenómeno suele conocerse como:

> **Automation Bias**

Es decir.

La tendencia humana a aceptar recomendaciones automáticas sin analizarlas críticamente.

En consecuencia.

Aunque el agente todavía no actúe por sí mismo, una recomendación errónea puede producir consecuencias importantes.

---

## Controles habituales

Resulta recomendable incorporar mecanismos como:

- explicación de razonamiento;
- referencias utilizadas;
- trazabilidad;
- revisión humana;
- validación técnica antes de la ejecución.

La transparencia comienza a ser tan importante como la precisión.

---

# 🟠 Level 2 — Supervised Execution

En este nivel el agente ya puede ejecutar acciones.

Sin embargo.

Cada acción requiere una aprobación explícita antes de producir efectos sobre el entorno.

Por ejemplo.

Un agente puede:

- preparar una modificación;
- seleccionar la herramienta adecuada;
- construir parámetros;
- generar una solicitud;
- presentar un plan completo.

Pero antes de ejecutar aparece una instancia de aprobación.

```text
Goal

↓

Agent Plan

↓

Selected Action

↓

Human Approval

↓

Execution
```

La diferencia con el nivel anterior resulta importante.

En el **Level 1** el usuario ejecutaba manualmente la acción.

En el **Level 2** es el propio agente quien ejecuta.

Pero únicamente después de recibir autorización.

---

## Riesgos principales

Los principales riesgos dejan de estar relacionados únicamente con la recomendación.

Ahora aparecen desafíos adicionales.

Por ejemplo.

- aprobaciones automáticas;
- fatiga del operador;
- aprobación de acciones mal comprendidas;
- diferencias entre lo aprobado y lo ejecutado;
- sustitución de parámetros.

En otras palabras.

La seguridad comienza a depender tanto del proceso de aprobación como del comportamiento del agente.

---

## Controles habituales

Las arquitecturas maduras suelen incorporar:

- Human-in-the-Loop;
- aprobación contextual;
- visualización exacta de la acción;
- validación de parámetros;
- autorización independiente;
- expiración de aprobaciones;
- registro completo de auditoría.

Una buena aprobación no consiste únicamente en mostrar un botón de "Aceptar".

Debe permitir comprender exactamente:

- qué hará el agente;
- sobre qué recurso;
- utilizando qué identidad;
- con qué impacto.

---

# 🏛️ Primeras conclusiones

Los tres primeros niveles comparten una característica importante.

El ser humano continúa conservando el control directo sobre las acciones con impacto operativo.

La diferencia radica en cuánto trabajo intelectual delega al agente.

```text
Level 0

The agent informs.

↓

Level 1

The agent recommends.

↓

Level 2

The agent prepares and executes
only after explicit approval.
```

Hasta este punto, el riesgo principal continúa estando asociado a la calidad del razonamiento y a la interacción entre el agente y el operador.

Sin embargo.

A partir del siguiente nivel ocurre un cambio mucho más profundo.

El agente deja de depender de una aprobación para cada acción individual y comienza a ejecutar tareas completas dentro de un alcance previamente autorizado.

Es precisamente allí donde la arquitectura debe empezar a incorporar mecanismos mucho más sofisticados relacionados con:

- **Scoped Delegation**;
- **Least Privilege**;
- **Policy Enforcement**;
- **Identity Propagation**;
- **Blast Radius**;
- **Revocation**;
- **Observability**.

Estos conceptos serán desarrollados en la siguiente sección, donde analizaremos los **Levels 3, 4 y 5**, correspondientes a arquitecturas verdaderamente autónomas.


---
---
---
---

---

# 🔴 Level 3 — Scoped Autonomous Execution

A partir del **Level 3** se produce uno de los cambios más importantes dentro de un sistema agéntico.

El agente ya no necesita aprobación para cada acción individual.

En cambio, recibe una **autorización limitada** para actuar de forma autónoma dentro de un alcance previamente definido.

Por ejemplo.

Un agente SOC podría recibir la siguiente instrucción.

> *"Durante la próxima hora podés investigar todas las alertas críticas relacionadas con phishing y aislar automáticamente los endpoints únicamente si el nivel de confianza supera el 95%."*

En este escenario el agente posee libertad para:

- consultar múltiples fuentes;
- correlacionar eventos;
- seleccionar herramientas;
- ejecutar acciones;
- repetir el proceso tantas veces como resulte necesario.

Sin embargo.

Toda esa autonomía continúa limitada por restricciones previamente definidas.

```text
Goal

↓

Agent

↓

Policy Engine

↓

Allowed Scope

↓

Autonomous Execution
```

Este enfoque representa uno de los modelos más utilizados en arquitecturas empresariales modernas.

---

## Riesgos principales

A partir de este nivel aparecen nuevos desafíos.

Por ejemplo.

- desviación progresiva del objetivo;
- uso inesperado de herramientas;
- errores acumulativos;
- ejecución de múltiples acciones incorrectas;
- abuso de credenciales delegadas;
- propagación automática de errores.

El problema deja de ser únicamente una acción equivocada.

Ahora un error puede repetirse decenas o cientos de veces antes de ser detectado.

---

## Controles habituales

Una arquitectura madura suele incorporar mecanismos como:

- Scope Validation;
- Policy Enforcement;
- Tool Allowlists;
- Rate Limiting;
- Kill Switch;
- Observabilidad continua;
- Auditoría completa;
- Identity Propagation.

El objetivo no consiste en impedir la autonomía.

Consiste en mantenerla permanentemente gobernada.

---

# 🔵 Level 4 — Adaptive Autonomy

En este nivel el agente no solamente ejecuta acciones.

También puede adaptar su estrategia a medida que obtiene nueva información.

Por ejemplo.

Durante una investigación puede descubrir que:

- el incidente afecta más sistemas;
- existe otra herramienta más adecuada;
- necesita consultar nuevas fuentes;
- debe cambiar el orden del plan inicial.

Todo ello ocurre sin intervención humana.

```text
Goal

↓

Reason

↓

Execute

↓

Observe

↓

Adapt Plan

↓

Continue
```

La autonomía deja de limitarse a ejecutar un flujo previamente preparado.

Ahora el propio agente modifica su estrategia durante la ejecución.

Este comportamiento resulta extremadamente poderoso.

Pero también incrementa considerablemente la complejidad de la arquitectura.

---

## Riesgos principales

La adaptación continua puede producir efectos difíciles de anticipar.

Por ejemplo.

- cambios inesperados de estrategia;
- consumo excesivo de recursos;
- ejecución prolongada;
- utilización de herramientas inicialmente no previstas;
- desviación del propósito original;
- acumulación de decisiones locales que generan un resultado global incorrecto.

Estos riesgos hacen que la observabilidad cobre una importancia mucho mayor que en arquitecturas tradicionales.

---

## Controles habituales

Resulta recomendable incorporar:

- Goal Validation;
- Runtime Guardrails;
- Tool Policies;
- Budget Limits;
- Time Limits;
- Memory Validation;
- Runtime Monitoring;
- Explainability;
- Approval Gates para operaciones críticas.

El agente puede modificar su estrategia.

Pero no debería modificar los límites impuestos por la arquitectura.

---

# 🟣 Level 5 — Fully Autonomous Systems

El nivel máximo de autonomía representa sistemas capaces de:

- recibir objetivos generales;
- construir planes;
- ejecutar herramientas;
- colaborar con otros agentes;
- modificar su estrategia;
- completar tareas complejas;
- finalizar el trabajo sin intervención humana.

En teoría.

```text
Goal

↓

Planning

↓

Execution

↓

Observation

↓

Adaptation

↓

Completion
```

Aunque técnicamente este nivel resulta posible, hoy continúa siendo poco frecuente dentro de entornos empresariales críticos.

No porque la tecnología no lo permita.

Sino porque el riesgo asociado suele superar los beneficios en la mayoría de los escenarios.

En sectores como:

- banca;
- salud;
- infraestructura crítica;
- defensa;
- energía;
- administración pública;

la intervención humana continúa siendo un requisito esencial para muchas operaciones.

---

## Riesgos principales

En este nivel aparecen prácticamente todos los desafíos de Agentic Security.

Por ejemplo.

- pérdida completa del contexto;
- Goal Drift;
- Prompt Injection indirecto;
- abuso de herramientas;
- escalamiento de privilegios;
- Confused Deputy;
- Privilege Amplification;
- uso indebido de identidades;
- cadenas de delegación;
- errores acumulativos;
- ampliación del Blast Radius.

En consecuencia.

Una arquitectura completamente autónoma requiere mecanismos de gobierno considerablemente más sofisticados que una aplicación tradicional.

---

# 📊 Comparación de niveles

| Nivel | El agente decide | El agente ejecuta | Supervisión humana |
|--------|------------------|-------------------|--------------------|
| **0** | No | No | Total |
| **1** | Parcialmente | No | Total |
| **2** | Sí | Sí, con aprobación | Muy alta |
| **3** | Sí | Sí, dentro de un alcance autorizado | Moderada |
| **4** | Sí y adapta su estrategia | Sí | Supervisión continua |
| **5** | Sí | Sí | Mínima o inexistente |

---

# 🎯 Elegir el nivel correcto de autonomía

Uno de los errores más frecuentes consiste en asumir que el objetivo siempre debe ser alcanzar el mayor nivel de autonomía posible.

Desde una perspectiva de ingeniería, esa decisión rara vez resulta adecuada.

El nivel correcto depende de factores como:

- impacto para el negocio;
- criticidad del activo;
- consecuencias de un error;
- requisitos regulatorios;
- reversibilidad de las acciones;
- capacidad de auditoría;
- madurez de la organización.

Un agente encargado de resumir documentación puede operar con un nivel de autonomía muy superior al de un agente responsable de modificar reglas de firewall o aprobar pagos.

La autonomía debe responder al riesgo.

No al entusiasmo por la automatización.

---

# 🏛️ Architecture Perspective

Una arquitectura segura no incrementa la autonomía eliminando controles.

Incrementa la autonomía **redistribuyendo responsabilidades** entre componentes especializados.

```text
Business Goal
      │
      ▼
Agent
(Reasoning)
      │
      ▼
Policy Engine
      │
      ▼
Identity & Authorization
      │
      ▼
Tool Broker
      │
      ▼
Execution
      │
      ▼
Monitoring
      │
      ▼
Audit
```

En este modelo.

El agente conserva la capacidad de razonar y planificar.

Pero nunca controla por sí mismo:

- la autorización;
- la identidad;
- las políticas;
- la auditoría;
- los límites operativos.

La autonomía deja de ser una propiedad del modelo y pasa a convertirse en una propiedad gobernada por toda la arquitectura.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando una organización afirma:

> **"Queremos un agente completamente autónomo."**

Un ingeniero de Agentic Security normalmente responde con otras preguntas.

- ¿Autónomo para hacer exactamente qué?
- ¿Sobre qué activos?
- ¿Durante cuánto tiempo?
- ¿Con qué identidad?
- ¿Bajo qué políticas?
- ¿Quién puede detenerlo?
- ¿Qué ocurre si interpreta mal el objetivo?
- ¿Cuál es el Blast Radius máximo aceptable?
- ¿Cómo se revoca su autoridad?
- ¿Cómo se reconstruyen todas sus decisiones?

Estas preguntas reflejan un cambio profundo de mentalidad.

La discusión deja de centrarse únicamente en las capacidades del modelo.

Pasa a centrarse en la capacidad de la arquitectura para gobernar esas capacidades de manera segura.

---

# 📌 Cierre de la sección

Comprender los distintos niveles de autonomía permite analizar un sistema agéntico mucho más allá de la simple pregunta *"¿es autónomo?"*.

Cada incremento de autonomía modifica:

- el modelo de riesgo;
- la superficie de ataque;
- los mecanismos de supervisión;
- las políticas de autorización;
- la necesidad de observabilidad;
- la responsabilidad de la arquitectura.

En consecuencia, la autonomía no debería considerarse un objetivo por sí misma.

Debe entenderse como una decisión de diseño que debe justificarse en función del riesgo, del contexto del negocio y de la capacidad de mantener el control operativo del sistema.

Una vez comprendido cómo evoluciona la autonomía, el siguiente paso consiste en analizar cómo esa capacidad de actuación se transfiere entre usuarios, agentes y servicios.

Ese proceso recibe un nombre fundamental dentro de Agentic Security:

> **Delegation**.


---
---
---
---

---

# 👥 Human Oversight

Hasta este punto hemos visto que un agente puede poseer distintos niveles de **Agency** y **Autonomy**.

Sin embargo, todavía queda una pregunta fundamental.

> **¿Quién supervisa al agente mientras toma decisiones y ejecuta acciones?**

Responder esta pregunta constituye uno de los pilares de **Agentic Security**.

A medida que los agentes adquieren mayor autonomía, disminuye la cantidad de decisiones revisadas manualmente.

Como consecuencia, la supervisión deja de ser una actividad opcional y pasa a convertirse en un componente esencial de la arquitectura.

En otras palabras.

No basta con decidir cuánto puede hacer un agente.

También es necesario definir **quién mantiene el control cuando el agente comienza a actuar**.

---

# 🎯 ¿Qué es Human Oversight?

Desde la perspectiva de Agentic Security, **Human Oversight** representa el conjunto de mecanismos mediante los cuales una persona mantiene la capacidad de supervisar, intervenir, limitar o detener el comportamiento de un agente inteligente.

El objetivo no consiste en reemplazar al agente.

Tampoco consiste en revisar manualmente cada decisión.

Su propósito es garantizar que las acciones del agente permanezcan alineadas con:

- los objetivos del negocio;
- las políticas de seguridad;
- los requisitos regulatorios;
- los límites de autoridad definidos por la organización.

En consecuencia, la supervisión humana no debe entenderse como una señal de desconfianza hacia el agente.

Debe entenderse como un mecanismo de gobierno (**governance**) que permite mantener el control incluso cuando la ejecución ha sido parcialmente automatizada.

---

# 🏛️ ¿Por qué Human Oversight es importante?

Durante décadas, la seguridad de aplicaciones estuvo basada en una idea relativamente sencilla.

Los sistemas ejecutaban exactamente aquello para lo que habían sido programados.

Si existía un comportamiento inesperado, normalmente era consecuencia de:

- un error de programación;
- una vulnerabilidad;
- un fallo operativo.

Los sistemas agénticos introducen un escenario diferente.

Ahora el agente puede:

- interpretar objetivos;
- construir planes;
- seleccionar herramientas;
- modificar la secuencia de ejecución;
- adaptar su estrategia durante el proceso.

Esto significa que parte del comportamiento deja de estar completamente determinado por el código.

Como consecuencia, la supervisión también debe evolucionar.

Ya no alcanza con verificar únicamente el resultado final.

En muchos casos resulta necesario comprender:

- cómo llegó el agente a esa decisión;
- qué información utilizó;
- qué herramientas invocó;
- qué alternativas descartó;
- por qué eligió una acción determinada.

La supervisión pasa de controlar únicamente la ejecución a supervisar también el proceso de razonamiento.

---

# ⚠️ Mayor autonomía requiere mayor gobernanza

Existe una relación directa entre el nivel de autonomía de un agente y la necesidad de mecanismos de supervisión.

A medida que aumenta la independencia operativa del agente, también aumenta la responsabilidad de la arquitectura para garantizar que esa autonomía permanezca dentro de límites aceptables.

Podemos representarlo de forma simplificada.

```text
Mayor Autonomy
        │
        ▼
Mayor Responsibility
        │
        ▼
More Governance
        │
        ▼
Better Oversight
```

Este principio aparece de forma consistente en marcos modernos como:

- NIST AI Risk Management Framework (AI RMF)
- Google Secure AI Framework (SAIF)
- Microsoft Responsible AI
- OWASP Agentic Security Initiative

Aunque cada uno utiliza terminología diferente, todos coinciden en una idea central.

> **La autonomía nunca debería crecer más rápido que la capacidad de supervisarla.**

---

# 🧩 Human Oversight no significa revisar todo

Uno de los errores más frecuentes consiste en pensar que la supervisión humana implica aprobar manualmente cada acción del agente.

Este enfoque resulta poco escalable.

Además, elimina gran parte del beneficio que aporta la automatización.

Una arquitectura madura busca un equilibrio entre:

- autonomía;
- eficiencia;
- gobernanza;
- seguridad.

Por ejemplo.

No todas las acciones poseen el mismo nivel de riesgo.

Resulta razonable permitir que un agente:

- clasifique documentos;
- genere resúmenes;
- consulte información pública;
- organice tareas.

Sin necesidad de intervención humana.

Sin embargo.

Acciones como:

- eliminar información;
- modificar infraestructura;
- transferir dinero;
- aprobar cambios regulatorios;
- revocar accesos;
- ejecutar comandos administrativos;

probablemente requieran un nivel mucho mayor de supervisión.

En consecuencia.

La supervisión no debería depender del agente.

Debe depender del **riesgo asociado a la acción**.

---

# 🎯 Supervisar decisiones, no únicamente resultados

En sistemas tradicionales resulta relativamente sencillo auditar una operación.

Por ejemplo.

```text
User

↓

Delete File

↓

Completed
```

En un sistema agéntico la secuencia puede ser considerablemente más compleja.

```text
Goal

↓

Reasoning

↓

Planning

↓

Tool Selection

↓

Execution

↓

Observation

↓

Next Decision
```

Si únicamente registramos el resultado final, perdemos información crítica.

Una supervisión efectiva debería permitir reconstruir preguntas como:

- ¿Qué objetivo recibió el agente?
- ¿Qué contexto utilizó?
- ¿Qué herramientas evaluó?
- ¿Qué decisión tomó?
- ¿Qué política autorizó esa acción?
- ¿Qué identidad utilizó?
- ¿Cuál fue el resultado?

Este enfoque mejora significativamente la capacidad de auditoría, investigación y respuesta ante incidentes.

---

# 🟢 Human-in-the-Loop (HITL)

El modelo más conservador de supervisión se conoce como **Human-in-the-Loop**.

En este enfoque, el agente puede analizar información, construir un plan y preparar una acción.

Sin embargo.

No puede ejecutarla sin una aprobación explícita por parte de una persona.

```text
Goal
   │
   ▼
Agent Analysis
   │
   ▼
Proposed Action
   │
   ▼
Human Approval
   │
   ▼
Execution
```

El ser humano permanece dentro del flujo de decisión.

Cada acción relevante requiere una confirmación antes de producir efectos sobre el entorno.

Este modelo suele utilizarse cuando:

- el impacto potencial es elevado;
- existen requisitos regulatorios;
- las acciones son difíciles de revertir;
- el nivel de confianza sobre el agente aún es limitado.

---

# 💡 Ventajas del Human-in-the-Loop

Entre sus principales beneficios se encuentran:

- reduce el riesgo de acciones irreversibles;
- facilita el cumplimiento regulatorio;
- incrementa la confianza en el sistema;
- permite detectar errores de razonamiento antes de la ejecución;
- favorece la adopción gradual de agentes dentro de organizaciones tradicionales.

Además.

Representa un excelente punto de partida para organizaciones que comienzan a incorporar agentes inteligentes en procesos críticos.

---

# ⚠️ Limitaciones del Human-in-the-Loop

Aunque resulta muy seguro, este enfoque también presenta desafíos.

Por ejemplo.

- disminuye la velocidad de ejecución;
- aumenta la carga operativa sobre los revisores;
- puede generar fatiga de aprobación (*approval fatigue*);
- reduce parte del beneficio asociado a la automatización.

Si cada acción requiere aprobación manual, existe el riesgo de que los operadores comiencen a aceptar solicitudes de forma rutinaria sin analizarlas en profundidad.

Paradójicamente, un exceso de aprobaciones puede disminuir la calidad de la supervisión.

Por este motivo, Human-in-the-Loop no debería utilizarse indiscriminadamente para todas las tareas.

Debe reservarse para aquellas acciones cuyo impacto justifique realmente la intervención humana.

---

# 💡 Pensar como un Agentic Security Engineer

Un ingeniero de Agentic Security rara vez pregunta:

> **¿El agente necesita aprobación humana?**

En cambio, suele formular preguntas mucho más específicas.

- ¿Qué acciones justifican una aprobación?
- ¿Qué nivel de riesgo presentan?
- ¿Quién debería aprobarlas?
- ¿Cómo se registra esa aprobación?
- ¿Cuánto tiempo permanece válida?
- ¿Qué ocurre si el contexto cambia antes de ejecutar la acción?
- ¿Puede revocarse una aprobación previamente otorgada?

Responder estas preguntas permite transformar una simple aprobación manual en un verdadero mecanismo de gobierno sobre el comportamiento del agente.

---
---
---
---

---

# 🟡 Human-on-the-Loop (HOTL)

A medida que las organizaciones adquieren mayor confianza en sus agentes, suele producirse un cambio en el modelo de supervisión.

En lugar de aprobar manualmente cada acción, el agente comienza a ejecutar determinadas tareas por sí mismo mientras una persona mantiene la capacidad de observar el proceso e intervenir cuando resulte necesario.

Este enfoque se conoce como **Human-on-the-Loop (HOTL)**.

En este modelo, el ser humano deja de participar en cada decisión individual y pasa a desempeñar un rol de supervisión continua.

```text
Goal
   │
   ▼
Agent
   │
   ▼
Execution
   │
   ▼
Monitoring
   │
   ▼
Human Intervention (if necessary)
```

El agente ejecuta.

El humano supervisa.

Y únicamente interviene cuando detecta un comportamiento inesperado o una situación que excede los límites establecidos.

---

# 🎯 ¿Cuándo utilizar Human-on-the-Loop?

Este modelo suele resultar apropiado cuando:

- el nivel de confianza sobre el agente es elevado;
- las acciones son parcialmente reversibles;
- existen mecanismos sólidos de observabilidad;
- el impacto potencial es moderado;
- la organización necesita automatizar procesos repetitivos.

Por ejemplo.

Un agente SOC puede:

- investigar alertas;
- enriquecer indicadores;
- consultar múltiples fuentes;
- correlacionar eventos;
- crear tickets automáticamente.

Mientras un analista supervisa la actividad desde un dashboard.

El analista no necesita aprobar cada consulta realizada por el agente.

Sin embargo.

Puede intervenir inmediatamente si detecta un comportamiento anómalo.

---

# ⚠️ Riesgos del Human-on-the-Loop

Aunque este modelo incrementa significativamente la productividad, también introduce nuevos desafíos.

Uno de ellos consiste en asumir que el operador siempre detectará un comportamiento incorrecto.

En la práctica esto no siempre ocurre.

Factores como:

- exceso de alertas;
- fatiga operativa;
- confianza excesiva en el agente;
- baja visibilidad;
- paneles poco claros;

pueden hacer que una acción incorrecta pase inadvertida.

Por este motivo, Human-on-the-Loop no elimina la necesidad de controles técnicos.

Simplemente modifica el momento en que interviene la persona.

---

# 🟣 Human-out-of-the-Loop (HOOTL)

El tercer modelo representa el mayor nivel de independencia operativa.

En un esquema **Human-out-of-the-Loop**, el agente puede completar una tarea sin intervención humana durante todo el ciclo de ejecución.

```text
Goal
   │
   ▼
Agent
   │
   ▼
Planning
   │
   ▼
Execution
   │
   ▼
Adaptation
   │
   ▼
Completion
```

En este escenario no existe una aprobación previa.

Tampoco una supervisión constante.

El sistema opera completamente dentro de los límites definidos por la arquitectura.

Es importante comprender que este modelo no implica ausencia de controles.

Implica ausencia de intervención humana durante la ejecución.

Los controles continúan existiendo.

Simplemente son implementados por mecanismos técnicos en lugar de revisiones manuales.

---

# 🏛️ ¿Cuándo puede ser apropiado?

Existen escenarios donde Human-out-of-the-Loop puede resultar razonable.

Por ejemplo.

- clasificación automática de documentos;
- análisis masivo de logs;
- enriquecimiento de inteligencia de amenazas;
- limpieza de datos;
- indexación de información;
- generación de reportes;
- monitoreo continuo.

En todos estos casos.

El impacto de una decisión incorrecta suele ser relativamente bajo o fácilmente reversible.

Sin embargo.

Cuando el agente interactúa con activos críticos, la situación cambia completamente.

Por ejemplo.

- aprobar préstamos;
- modificar infraestructura;
- administrar identidades;
- ejecutar cambios en producción;
- realizar transferencias financieras;
- modificar historias clínicas;
- revocar accesos privilegiados.

En estos escenarios, un modelo completamente autónomo suele resultar inadecuado salvo que existan controles extremadamente robustos.

---

# ⚠️ El mayor error: pensar que más autonomía siempre es mejor

Uno de los mensajes comerciales más frecuentes alrededor de la inteligencia artificial consiste en asociar una mayor autonomía con una mayor madurez tecnológica.

Desde la perspectiva de Agentic Security, esta idea resulta peligrosa.

El objetivo no consiste en eliminar completamente al ser humano.

El objetivo consiste en **ubicar al ser humano exactamente donde aporta mayor valor**.

En algunos procesos será necesario aprobar cada acción.

En otros bastará con supervisar.

Y en otros podrá delegarse completamente la ejecución.

La elección correcta depende del riesgo.

No del nivel de sofisticación del agente.

---

# 🎯 Elegir el modelo adecuado de supervisión

La elección entre HITL, HOTL y HOOTL no debería basarse únicamente en las capacidades del modelo.

Debe considerar múltiples factores.

Por ejemplo.

- criticidad del activo;
- impacto potencial de un error;
- posibilidad de revertir acciones;
- requisitos regulatorios;
- nivel de confianza sobre el agente;
- madurez de los controles existentes;
- capacidad de auditoría;
- tolerancia al riesgo de la organización.

Una forma sencilla de visualizar esta decisión consiste en pensar que la supervisión disminuye a medida que aumentan la confianza y los controles técnicos.

```text
Low Trust
High Impact
        │
        ▼
Human-in-the-Loop

↓

Human-on-the-Loop

↓

Human-out-of-the-Loop

        ▲
High Trust
Low Impact
```

La transición entre modelos no debería producirse de manera abrupta.

Debe formar parte de una estrategia gradual de adopción.

---

# 🏗️ Architecture Perspective

Una arquitectura madura evita depender exclusivamente del comportamiento del agente o de la intervención humana.

En cambio, distribuye la supervisión entre varios componentes.

```text
Business Goal
      │
      ▼
Agent
      │
      ▼
Policy Engine
      │
      ▼
Tool Broker
      │
      ▼
Execution
      │
      ▼
Monitoring
      │
      ▼
Logging & Audit
      │
      ▼
Human Oversight
```

En este modelo:

- el agente propone y ejecuta acciones;
- las políticas validan que esas acciones sean aceptables;
- los mecanismos de identidad determinan bajo qué autoridad actuar;
- los sistemas de monitoreo detectan comportamientos anómalos;
- las personas conservan la capacidad de intervenir cuando resulte necesario.

La supervisión deja de depender de un único mecanismo y pasa a convertirse en una propiedad de toda la arquitectura.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza un sistema agéntico, rara vez pregunta:

> **¿Tenemos Human-in-the-Loop?**

Las preguntas relevantes suelen ser otras.

- ¿Qué modelo de supervisión utiliza cada tarea?
- ¿Es posible cambiar dinámicamente entre HITL y HOTL?
- ¿Qué eventos obligan a volver a intervención humana?
- ¿Qué acciones nunca deberían ejecutarse sin aprobación?
- ¿Cómo se detiene un agente que actúa incorrectamente?
- ¿Quién recibe la primera alerta?
- ¿Cómo se reconstruye toda la secuencia de decisiones?

Responder estas preguntas permite diseñar sistemas donde la supervisión forma parte de la arquitectura y no únicamente del proceso operativo.

---

# 📌 Key Ideas

A medida que aumenta la autonomía de un agente, también aumenta la necesidad de mecanismos de supervisión adecuados.

Los tres modelos principales representan distintos niveles de intervención humana.

- **Human-in-the-Loop (HITL):** el ser humano aprueba las acciones antes de su ejecución.
- **Human-on-the-Loop (HOTL):** el agente ejecuta mientras una persona supervisa e interviene cuando resulta necesario.
- **Human-out-of-the-Loop (HOOTL):** el agente opera de forma autónoma dentro de los límites establecidos por la arquitectura.

Ninguno de estos modelos es universalmente mejor que los demás.

Cada uno responde a diferentes necesidades de negocio, niveles de riesgo y requisitos regulatorios.

La decisión correcta no consiste en maximizar la autonomía.

Consiste en encontrar el equilibrio adecuado entre **eficiencia, gobernanza y seguridad**.

Con estos conceptos ya es posible comprender cómo un agente decide, actúa y es supervisado.

El siguiente paso consiste en analizar cómo esa capacidad de actuación puede ser **delegada** entre usuarios, agentes y servicios, introduciendo nuevos desafíos relacionados con identidad, autoridad y control de privilegios.



---
---
---
---

---

# 📈 Progressive Autonomy

Hasta este punto hemos analizado qué es la autonomía, cómo evoluciona a través de distintos niveles y qué modelos de supervisión pueden aplicarse para mantener el control sobre un agente.

Sin embargo, todavía queda una pregunta crítica desde la perspectiva de la ingeniería.

> **¿Cómo debería una organización aumentar la autonomía de un agente de forma segura?**

La respuesta rara vez consiste en habilitar todas las capacidades desde el primer día.

De hecho, una de las mejores prácticas observadas en organizaciones con altos niveles de madurez consiste en exactamente lo contrario.

La autonomía debe construirse **de forma progresiva**, aumentando gradualmente el nivel de independencia del agente a medida que la organización desarrolla confianza en su comportamiento y fortalece los mecanismos de control.

Este enfoque recibe el nombre de **Progressive Autonomy**.

---

# 🎯 ¿Qué es Progressive Autonomy?

**Progressive Autonomy** es un principio de diseño según el cual un agente incrementa gradualmente su nivel de autonomía a medida que demuestra un comportamiento confiable dentro de límites previamente definidos.

En lugar de otorgar desde el comienzo la capacidad de ejecutar cualquier acción, la organización habilita nuevas responsabilidades de manera controlada.

Cada incremento representa una decisión consciente de arquitectura.

No una consecuencia natural de utilizar un modelo más avanzado.

---

# 🏗️ La autonomía debe ganarse

Un error frecuente consiste en pensar que un agente debería recibir inmediatamente todas las capacidades que la tecnología permite.

Por ejemplo.

Un agente recién desplegado podría:

- consultar bases de datos;
- modificar infraestructura;
- enviar correos;
- crear usuarios;
- ejecutar scripts;
- administrar recursos cloud.

Aunque técnicamente esto sea posible, desde una perspectiva de seguridad representa un riesgo innecesario.

Una arquitectura madura adopta una estrategia diferente.

```text
Observe

↓

Recommend

↓

Execute with Approval

↓

Limited Autonomous Execution

↓

Scoped Autonomy

↓

Adaptive Autonomy
```

Cada etapa requiere demostrar que el comportamiento del agente continúa siendo consistente antes de avanzar hacia la siguiente.

---

# 📊 Un modelo de evolución gradual

Podemos representar este crecimiento como una curva de madurez.

```text
Stage 1

Observation

↓

Stage 2

Recommendation

↓

Stage 3

Execution with Human Approval

↓

Stage 4

Execution within Defined Scope

↓

Stage 5

Adaptive Execution

↓

Stage 6

Operational Autonomy
```

Cada transición implica nuevas responsabilidades para la arquitectura.

Por ejemplo.

Durante las primeras etapas predominan:

- validaciones humanas;
- observación del comportamiento;
- ajustes de prompts;
- refinamiento de herramientas.

En etapas posteriores cobran mayor importancia:

- políticas dinámicas;
- observabilidad;
- auditoría;
- aislamiento;
- gestión de identidad;
- mecanismos de revocación.

---

# ⚠️ ¿Por qué avanzar gradualmente?

Los agentes inteligentes operan en entornos inherentemente dinámicos.

Interactúan con:

- modelos probabilísticos;
- herramientas externas;
- APIs;
- usuarios;
- documentos;
- bases de conocimiento;
- otros agentes.

En consecuencia, resulta prácticamente imposible anticipar todas las combinaciones posibles de comportamiento antes de poner el sistema en producción.

Una estrategia progresiva permite:

- observar comportamientos reales;
- detectar patrones inesperados;
- medir la precisión del razonamiento;
- evaluar la calidad de las decisiones;
- validar los controles existentes;
- reducir el Blast Radius durante las primeras etapas.

En otras palabras.

La organización aprende junto con el agente.

---

# 🔒 Progressive Autonomy y Risk Management

La autonomía nunca debería evolucionar únicamente en función de la confianza subjetiva sobre el agente.

Debe hacerlo considerando el riesgo.

Por ejemplo.

Un agente encargado de clasificar tickets probablemente pueda alcanzar rápidamente un alto nivel de autonomía.

En cambio.

Un agente responsable de administrar identidades privilegiadas debería evolucionar mucho más lentamente.

La autonomía debe aumentar únicamente cuando:

- los riesgos son conocidos;
- los controles fueron validados;
- los mecanismos de auditoría funcionan correctamente;
- la organización posee capacidad para responder ante incidentes.

Este enfoque resulta coherente con principios presentes en marcos como:

- NIST AI RMF
- NIST Risk Management Framework
- Microsoft Responsible AI
- Google Secure AI Framework (SAIF)

Todos ellos promueven una adopción gradual basada en evidencia y gestión del riesgo.

---

# 🛡️ Guardrails: el complemento de la autonomía

A medida que aumenta la autonomía, también debe aumentar la cantidad y calidad de los **Guardrails**.

Los Guardrails representan restricciones técnicas que limitan el comportamiento del agente sin eliminar completamente su capacidad de decisión.

Entre los más habituales encontramos:

- validación de parámetros;
- límites de herramientas;
- límites temporales;
- límites presupuestarios;
- restricciones de red;
- límites de memoria;
- políticas de autorización;
- listas de herramientas permitidas;
- restricciones sobre determinados activos.

El objetivo no consiste en reducir la inteligencia del agente.

Consiste en garantizar que esa inteligencia opere dentro de un entorno seguro.

---

# 🛑 Kill Switch

Una característica indispensable en agentes con niveles elevados de autonomía consiste en disponer de mecanismos capaces de detener inmediatamente la ejecución.

Este mecanismo suele conocerse como **Kill Switch**.

Su propósito consiste en interrumpir cualquier acción cuando:

- el agente abandona el objetivo autorizado;
- detectamos un comportamiento inesperado;
- una herramienta responde de forma anómala;
- cambia el contexto operativo;
- aparece una nueva política;
- ocurre un incidente de seguridad.

Un buen Kill Switch debería ser:

- inmediato;
- auditable;
- reversible cuando corresponda;
- independiente del razonamiento del agente.

En otras palabras.

El propio agente nunca debería ser responsable de decidir si debe detenerse.

---

# 🔄 Rollback

No todas las acciones ejecutadas por un agente son irreversibles.

Siempre que resulte posible, la arquitectura debería permitir deshacer operaciones recientes.

Por ejemplo.

- eliminar recursos temporales;
- revertir configuraciones;
- cancelar tareas;
- restaurar versiones anteriores;
- recuperar estados consistentes.

La posibilidad de realizar un **Rollback** reduce considerablemente el impacto de errores operativos.

Sin embargo.

No todas las acciones pueden revertirse.

Por ejemplo.

- transferencias bancarias;
- envío de información confidencial;
- publicación de datos;
- eliminación definitiva de registros.

En estos casos, la supervisión previa adquiere todavía mayor importancia.

---

# 🧪 Sandboxes

Una práctica ampliamente utilizada consiste en permitir que el agente experimente inicialmente dentro de entornos controlados.

Los **Sandboxes** permiten observar cómo razona el agente sin comprometer activos reales.

Por ejemplo.

Un agente puede:

- probar consultas;
- generar configuraciones;
- ejecutar comandos simulados;
- interactuar con APIs ficticias;
- validar planes de acción.

Solo cuando demuestra un comportamiento consistente se habilita progresivamente el acceso a entornos de mayor criticidad.

---

# 📊 Medir antes de ampliar

La evolución de la autonomía no debería basarse únicamente en percepciones.

Debe sustentarse en métricas objetivas.

Algunos indicadores útiles incluyen:

- porcentaje de tareas completadas correctamente;
- tasa de intervenciones humanas;
- cantidad de ejecuciones revertidas;
- errores por herramienta;
- tiempo promedio de ejecución;
- frecuencia de desviación del objetivo;
- cantidad de decisiones bloqueadas por políticas.

Estas métricas permiten evaluar si el agente realmente está preparado para asumir un mayor nivel de autonomía.

---

# 🏛️ Architecture Perspective

Una arquitectura madura incorpora mecanismos de crecimiento gradual desde el diseño inicial.

```text
Business Goal
        │
        ▼
Agent
        │
        ▼
Policy Engine
        │
        ▼
Guardrails
        │
        ▼
Approval Gates
        │
        ▼
Execution
        │
        ▼
Monitoring
        │
        ▼
Metrics
        │
        ▼
Increase or Reduce Autonomy
```

Obsérvese que la autonomía deja de ser una configuración estática.

Se convierte en una propiedad dinámica que puede aumentar o disminuir según el comportamiento observado.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando una organización solicita:

> **"Queremos un agente completamente autónomo."**

Un ingeniero de Agentic Security suele reformular el problema.

Las preguntas correctas son:

- ¿Qué autonomía necesita realmente esta tarea?
- ¿Qué evidencia demuestra que el agente está preparado para asumir más responsabilidad?
- ¿Qué controles acompañarán ese crecimiento?
- ¿Qué mecanismos permiten reducir nuevamente la autonomía?
- ¿Existe un Kill Switch independiente?
- ¿Qué acciones son irreversibles?
- ¿Cuál es el Blast Radius máximo aceptable?

Responder estas preguntas permite diseñar agentes cuya autonomía evoluciona de forma controlada y alineada con el riesgo del negocio.

---

# 📌 Key Ideas

La autonomía no debería habilitarse completamente desde el primer día.

Una estrategia de **Progressive Autonomy** permite que el agente adquiera mayor independencia únicamente cuando demuestra un comportamiento consistente y la arquitectura dispone de controles suficientes para gobernarlo.

En consecuencia.

Una organización madura no busca construir el agente más autónomo posible.

Busca construir el agente **más confiable posible**, incrementando gradualmente su autonomía sin perder nunca la capacidad de supervisarlo, limitarlo y detenerlo cuando resulte necesario.

Con estos conceptos ya hemos desarrollado los fundamentos relacionados con **Agency**, **Autonomy** y **Human Oversight**.

El siguiente paso consiste en analizar cómo esa capacidad de actuación puede transferirse entre usuarios, agentes y servicios mediante un concepto central en Agentic Security:

> **Delegation**

---
---
---
---


---

# 🤝 Delegation

Hasta este punto hemos analizado cómo un agente adquiere capacidades, desarrolla Agency y opera con distintos niveles de Autonomy.

Sin embargo, todavía existe una pregunta fundamental que determina la seguridad de cualquier sistema agéntico.

> **¿Quién le permitió al agente realizar esa acción?**

Esta pregunta marca el comienzo de uno de los conceptos más importantes de Agentic Security:

**Delegation**.

Durante décadas, los sistemas de información estuvieron construidos alrededor de una idea relativamente simple.

Un usuario autenticado interactuaba directamente con una aplicación.

La aplicación verificaba su identidad, comprobaba sus permisos y ejecutaba la acción solicitada.

```text
User
   │
Authenticate
   │
Authorize
   │
Execute
```

En un sistema agéntico moderno este modelo cambia por completo.

Ahora el usuario rara vez ejecuta directamente todas las acciones.

En cambio, define un objetivo.

El agente interpreta ese objetivo, construye un plan, selecciona herramientas y ejecuta múltiples operaciones en nombre del usuario.

```text
User
   │
Goal
   │
Agent
   │
Planning
   │
Tool Selection
   │
Execution
```

En consecuencia aparece una nueva pregunta.

> **¿Con qué autoridad está actuando realmente el agente?**

Responder correctamente esta pregunta constituye el núcleo de Agentic Security.

---

# 🎯 ¿Qué es Delegation?

Desde la perspectiva de Agentic Security, **Delegation** es el proceso mediante el cual una identidad transfiere de forma controlada parte de su autoridad a otra identidad para que pueda ejecutar acciones en su nombre.

Es importante observar que la delegación no implica transferir toda la autoridad.

Tampoco implica perder el control sobre ella.

Una delegación correctamente diseñada siempre especifica:

- quién delega;
- quién recibe la delegación;
- qué autoridad se transfiere;
- sobre qué recursos;
- durante cuánto tiempo;
- bajo qué restricciones;
- cómo puede revocarse.

En otras palabras.

La delegación representa un mecanismo de distribución de autoridad, no una cesión permanente de privilegios.

---

# 🏛️ La seguridad no delega tareas

Uno de los errores conceptuales más frecuentes consiste en afirmar que un agente "recibe una tarea".

Desde una perspectiva de seguridad, esa descripción resulta incompleta.

Lo que realmente recibe el agente no es únicamente una tarea.

Recibe la autoridad necesaria para ejecutar determinadas acciones relacionadas con esa tarea.

Por ejemplo.

Un usuario puede solicitar:

> "Investigá todas las alertas críticas generadas durante las últimas cuatro horas."

La tarea consiste en investigar.

Pero para llevarla a cabo el agente necesitará autoridad para:

- consultar el SIEM;
- acceder a registros;
- leer información del EDR;
- consultar inteligencia de amenazas;
- crear tickets;
- generar reportes.

La tarea representa el objetivo.

La autoridad representa el permiso operativo necesario para alcanzarlo.

Esta diferencia resulta fundamental.

En Agentic Security no analizamos únicamente qué hace el agente.

Analizamos **bajo qué autoridad puede hacerlo**.

---

# 🔑 Delegar no significa entregar privilegios ilimitados

Una delegación madura nunca transfiere toda la autoridad disponible.

Por el contrario.

Busca entregar únicamente la autoridad mínima necesaria para completar una tarea específica.

Supongamos el siguiente escenario.

Un administrador solicita al agente:

> "Generá un informe sobre usuarios inactivos."

El agente probablemente necesite:

- consultar Active Directory;
- leer atributos de usuarios;
- acceder a registros de autenticación.

No necesita:

- eliminar cuentas;
- modificar grupos;
- cambiar contraseñas;
- crear nuevos usuarios.

Aunque el usuario posea esos privilegios, la delegación no debería incluirlos.

En consecuencia.

Una delegación segura siempre responde a una pregunta muy concreta.

> **¿Cuál es la autoridad mínima necesaria para completar este objetivo?**

Este principio se encuentra estrechamente relacionado con **Least Privilege**, aunque aplicado al contexto de agentes inteligentes.

---

# 🔄 Delegation no reemplaza Authentication ni Authorization

Otro error habitual consiste en asumir que la delegación reemplaza los mecanismos tradicionales de seguridad.

En realidad, ocurre exactamente lo contrario.

La delegación depende de ellos.

Antes de delegar autoridad resulta necesario conocer:

- quién solicita la acción;
- qué identidad posee;
- qué permisos tiene actualmente;
- qué políticas aplican sobre esa identidad.

Solo después de responder estas preguntas puede decidirse qué parte de esa autoridad será delegada al agente.

Por este motivo, Delegation se construye sobre Authentication y Authorization.

Nunca los sustituye.

---

# 🧩 Delegation vs Authentication

Aunque ambos conceptos aparecen muy relacionados, responden preguntas completamente diferentes.

La autenticación responde:

> **¿Quién sos?**

La delegación responde:

> **¿En nombre de quién podés actuar?**

Por ejemplo.

Un agente puede autenticarse utilizando una identidad propia.

Sin embargo.

Esa identidad no implica automáticamente que pueda actuar representando a un usuario.

Para ello resulta necesario un mecanismo explícito de delegación.

En otras palabras.

Autenticarse demuestra identidad.

Delegar concede autoridad.

---

# 🧩 Delegation vs Authorization

La autorización responde otra pregunta.

> **¿Está permitida esta acción?**

La delegación responde algo distinto.

> **¿Quién autorizó al agente a ejecutar esa acción en su nombre?**

Por ejemplo.

Una política puede indicar que el agente tiene permitido consultar una base de datos.

Eso constituye una autorización.

Sin embargo.

Todavía falta responder:

- ¿qué usuario originó la solicitud?;
- ¿qué alcance tiene esa delegación?;
- ¿durante cuánto tiempo permanece válida?;
- ¿qué restricciones acompañan esa autoridad?.

Delegation y Authorization trabajan juntas.

Pero representan mecanismos diferentes.

---

# 🎯 Delegated Authority

El concepto central de esta sección es la **Delegated Authority**.

Toda acción ejecutada por un agente debería poder responder claramente las siguientes preguntas.

- ¿Quién originó la solicitud?
- ¿Quién delegó la autoridad?
- ¿Qué autoridad fue delegada?
- ¿Qué restricciones permanecen activas?
- ¿Qué políticas continúan aplicándose?
- ¿Cómo puede revocarse esa delegación?

Si alguna de estas preguntas no puede responderse, probablemente la arquitectura posea un problema de trazabilidad o gobierno.

---

# ⏳ Delegation Lifetime

Una delegación nunca debería considerarse permanente.

La autoridad delegada debe existir únicamente mientras resulte necesaria para completar el objetivo autorizado.

Por este motivo, toda delegación debería definir explícitamente su ciclo de vida.

Por ejemplo.

- una única operación;
- una sesión;
- una ventana temporal;
- una tarea específica;
- una aprobación puntual.

Una vez finalizado ese contexto, la autoridad debería expirar automáticamente.

Reducir la duración de la delegación disminuye significativamente la superficie de ataque y limita el impacto potencial ante el compromiso de una identidad o una herramienta.

---

# 🚧 Delegation Constraints

Una delegación segura no solamente limita el tiempo.

También limita el contexto.

Las restricciones pueden aplicarse sobre diferentes dimensiones.

Por ejemplo.

- herramientas permitidas;
- recursos autorizados;
- tipos de acciones;
- horario de ejecución;
- ubicación;
- entorno (desarrollo, pruebas o producción);
- presupuesto disponible;
- cantidad máxima de operaciones;
- clasificación de la información.

Estas restricciones permiten que el agente conserve capacidad de decisión sin adquirir autoridad ilimitada.

En consecuencia.

La delegación deja de ser una autorización genérica y pasa a convertirse en un contrato explícito entre la organización, el usuario y el agente.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto revisa una arquitectura agéntica no debería limitarse a preguntar:

> **¿Qué herramientas utiliza este agente?**

Las preguntas realmente importantes suelen ser otras.

- ¿Quién delegó esta autoridad?
- ¿Cuál es el alcance exacto de la delegación?
- ¿Qué acciones quedaron fuera de ese alcance?
- ¿Cuándo expira?
- ¿Puede revocarse inmediatamente?
- ¿Cómo se audita?
- ¿Qué ocurre si el agente intenta actuar fuera de los límites definidos?

Responder estas preguntas permite diseñar arquitecturas donde la autoridad permanece distribuida, controlada y completamente trazable.

Precisamente por ello, la delegación constituye uno de los pilares fundamentales de Agentic Security y el punto de partida para comprender conceptos como **Scoped Delegation**, **Delegation Chains**, **Agent Identity**, **Confused Deputy** y **Privilege Amplification**, que desarrollaremos en las siguientes secciones.


---
---
---
---


---

# 🎯 Delegation Scope

Hasta este punto hemos visto que la delegación consiste en transferir parte de una autoridad para que otra identidad pueda actuar en nuestro nombre.

Sin embargo, una delegación segura nunca debería responder únicamente a la pregunta:

> **¿Qué puede hacer el agente?**

También debe responder:

> **¿Hasta dónde puede hacerlo?**

Ese límite recibe el nombre de **Delegation Scope**.

El **Scope** representa el conjunto de restricciones que definen el alcance operativo de una delegación.

En otras palabras.

No describe únicamente las acciones permitidas.

Describe también los límites dentro de los cuales esas acciones continúan siendo válidas.

Una delegación sin Scope definido deja de ser una delegación.

Se convierte en una cesión de privilegios.

---

# 📦 ¿Qué puede limitar un Scope?

En sistemas agénticos modernos, el Scope puede restringirse sobre múltiples dimensiones simultáneamente.

Por ejemplo.

- Recursos.
- Herramientas.
- APIs.
- Tipos de operaciones.
- Tiempo.
- Usuarios.
- Entornos.
- Datos.
- Presupuesto.
- Cantidad máxima de ejecuciones.

Por ejemplo.

```text
Allowed

✓ Read CRM

✓ Create Ticket

✓ Query SIEM

Not Allowed

✗ Delete Users

✗ Create Admin Accounts

✗ Execute PowerShell

✗ Modify Firewall Rules
```

El agente conserva autonomía.

Pero únicamente dentro del alcance autorizado.

---

# 🏗️ Scope y Business Context

Una característica importante del Scope consiste en que depende completamente del contexto del negocio.

No existe un Scope universal.

Por ejemplo.

Un agente financiero puede tener permitido:

- consultar facturas;
- generar balances;
- validar pagos.

Mientras que un agente SOC puede tener permitido:

- consultar logs;
- aislar endpoints;
- bloquear indicadores.

Cada agente recibe únicamente la autoridad necesaria para cumplir su objetivo.

No más.

---

# 🔄 Scoped Delegation

Cuando una delegación incorpora límites explícitos hablamos de **Scoped Delegation**.

Este modelo constituye una de las mejores prácticas actuales para sistemas agénticos.

En lugar de delegar autoridad completa.

Se delega únicamente una fracción.

```text
User Authority

████████████████

Delegated Scope

████
```

La diferencia entre ambos conjuntos representa privilegios que nunca abandonan al usuario.

Este principio reduce significativamente el impacto potencial de un compromiso del agente.

---

# 👤 Agent Identity

Hasta ahora hemos hablado de delegar autoridad.

Sin embargo.

Toda autoridad necesita una identidad sobre la cual aplicarse.

Esto nos lleva a otro concepto fundamental.

**Agent Identity**.

Una identidad representa quién realiza una acción.

En arquitecturas modernas, un agente nunca debería operar como un proceso anónimo.

Cada agente debería poseer una identidad propia.

Esto permite:

- autenticar al agente;
- aplicar políticas;
- registrar auditoría;
- limitar privilegios;
- revocar accesos;
- rastrear todas las acciones realizadas.

Sin identidad no existe trazabilidad.

Y sin trazabilidad no existe gobierno.

---

# 🏛️ ¿Qué identidad debería utilizar un agente?

Existen varios modelos.

Por ejemplo.

### Actuar utilizando la identidad del usuario

```text
User

↓

Agent

↓

CRM
```

Toda acción aparece como realizada por el usuario.

Este modelo simplifica algunos escenarios.

Pero dificulta distinguir qué acciones fueron ejecutadas directamente por una persona y cuáles por un agente.

---

### Actuar utilizando una identidad propia

```text
User

↓

Delegation

↓

Agent Identity

↓

CRM
```

Aquí el agente posee una identidad independiente.

La autoridad proviene del usuario.

Pero la ejecución queda claramente registrada como realizada por el agente.

Este modelo suele resultar mucho más adecuado para auditoría y respuesta ante incidentes.

---

# 🔗 Delegation Chains

Los sistemas agénticos modernos rara vez poseen un único agente.

Con frecuencia encontramos arquitecturas donde varios agentes colaboran para alcanzar un objetivo.

Por ejemplo.

```text
User

↓

Coordinator Agent

↓

Research Agent

↓

Analysis Agent

↓

Reporting Agent
```

Cada salto representa una nueva delegación.

Y cada delegación introduce nuevas preguntas.

- ¿Qué autoridad recibió cada agente?
- ¿Puede volver a delegarla?
- ¿Puede ampliarla?
- ¿Puede modificarla?
- ¿Puede mantenerla una vez finalizada la tarea?

Responder estas preguntas constituye uno de los desafíos más importantes de Agentic Security.

---

# ⚠️ Riesgos de las cadenas de delegación

Cada nuevo salto incrementa la complejidad del modelo de confianza.

Por ejemplo.

```text
User

↓

Agent A

↓

Agent B

↓

Tool

↓

Cloud API
```

Si no existen restricciones claras, pueden aparecer problemas como:

- pérdida de contexto;
- delegaciones excesivas;
- ampliación involuntaria de privilegios;
- uso de identidades compartidas;
- imposibilidad de reconstruir quién originó realmente una acción.

Por este motivo, las cadenas de delegación deberían mantenerse tan cortas como resulte razonablemente posible.

---

# 🛡️ Delegation debería ser revocable

Una característica esencial de cualquier delegación segura consiste en que puede eliminarse inmediatamente.

Una delegación no representa un derecho permanente.

Representa una autorización temporal.

Por ejemplo.

Una organización debería poder revocar una delegación cuando:

- cambia el objetivo;
- finaliza la tarea;
- cambia el contexto;
- el agente presenta comportamiento inesperado;
- ocurre un incidente de seguridad;
- una identidad resulta comprometida.

En consecuencia.

Toda arquitectura debería incorporar mecanismos explícitos para invalidar autoridad previamente delegada.

---

# 🏛️ Architecture Perspective

Una arquitectura madura evita que la autoridad fluya libremente entre componentes.

En cambio.

Cada transición incorpora nuevos mecanismos de control.

```text
Business Goal
       │
       ▼
User
       │
Delegation
       │
       ▼
Agent Identity
       │
Policy Engine
       │
Scope Validation
       │
       ▼
Tool Broker
       │
       ▼
Execution
       │
       ▼
Audit Log
```

Obsérvese que la delegación nunca llega directamente hasta la herramienta.

Antes atraviesa distintos componentes encargados de validar:

- identidad;
- políticas;
- alcance;
- restricciones;
- autorización.

Este diseño permite que la autoridad permanezca gobernada durante toda la ejecución.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza una arquitectura con múltiples agentes, normalmente evita preguntar:

> **¿Qué puede hacer este agente?**

En su lugar suele formular preguntas como:

- ¿Qué autoridad recibió?
- ¿Quién se la delegó?
- ¿Puede volver a delegarla?
- ¿Cuál es el Scope máximo permitido?
- ¿Qué identidad utiliza?
- ¿Cómo se audita cada salto?
- ¿Cómo se revoca una delegación activa?
- ¿Cuál es el Blast Radius si esta identidad resulta comprometida?

Responder estas preguntas permite diseñar arquitecturas donde la autoridad permanece distribuida, limitada y completamente trazable.

---

# 📌 Key Ideas

La delegación constituye uno de los pilares fundamentales de Agentic Security.

Sin embargo, una delegación segura nunca consiste únicamente en permitir que un agente actúe en nombre de un usuario.

Debe definir explícitamente:

- qué autoridad se delega;
- sobre qué recursos;
- mediante qué identidad;
- dentro de qué Scope;
- durante cuánto tiempo;
- bajo qué restricciones;
- cómo puede auditarse;
- cómo puede revocarse.

Estos principios preparan el terreno para comprender algunos de los riesgos más importantes de los sistemas agénticos modernos.

En las próximas secciones analizaremos cómo errores en el diseño de la delegación pueden derivar en problemas como **Confused Deputy**, **Privilege Amplification**, **Blast Radius** y otros patrones que hoy representan algunos de los mayores desafíos de Agentic Security.


---
---
---
---

---

# 🔐 Least Privilege for Agents

Hasta este punto hemos visto que un agente puede recibir autoridad mediante mecanismos de **Delegation**.

Sin embargo, todavía queda responder una pregunta fundamental.

> **¿Cuánta autoridad debería recibir realmente un agente?**

Durante décadas, uno de los principios más importantes de la seguridad informática ha sido el **Principle of Least Privilege (PoLP)**.

Este principio aparece en numerosos marcos de referencia, entre ellos:

- NIST SP 800-53
- NIST SP 800-207 (Zero Trust)
- OWASP ASVS
- CIS Controls
- Microsoft Security Development Lifecycle (SDL)
- Google Secure AI Framework (SAIF)

Aunque originalmente fue concebido para usuarios, procesos y servicios, hoy constituye uno de los pilares fundamentales de **Agentic Security**.

La diferencia es que los agentes inteligentes toman decisiones, planifican acciones y utilizan herramientas dinámicamente.

Como consecuencia, aplicar Least Privilege en sistemas agénticos requiere un enfoque considerablemente más sofisticado que en aplicaciones tradicionales.

---

# 🎯 ¿Qué significa Least Privilege?

El **Principle of Least Privilege** establece que toda identidad debe poseer únicamente los privilegios mínimos necesarios para completar una tarea autorizada.

Ni más.

Ni menos.

En otras palabras.

Un agente debería disponer únicamente de la autoridad estrictamente necesaria para alcanzar el objetivo que le fue delegado.

No debería recibir permisos adicionales "por si acaso".

Tampoco privilegios permanentes para facilitar futuras tareas.

Cada privilegio innecesario aumenta la superficie de ataque y amplía el impacto potencial de un incidente.

---

# 🏛️ Least Privilege en aplicaciones tradicionales

En una aplicación convencional este principio resulta relativamente sencillo de aplicar.

Por ejemplo.

```text
Application

↓

Database

Read Only
```

Si la aplicación únicamente necesita consultar información, la cuenta utilizada para conectarse a la base de datos debería poseer permisos de lectura.

No necesita:

- eliminar tablas;
- crear usuarios;
- modificar configuraciones;
- ejecutar tareas administrativas.

El principio es simple.

Otorgar únicamente aquello que realmente será utilizado.

---

# 🤖 Least Privilege cambia en sistemas agénticos

Los agentes inteligentes modifican completamente este escenario.

¿Por qué?

Porque un agente no ejecuta siempre exactamente la misma secuencia de acciones.

Puede:

- seleccionar distintas herramientas;
- construir planes diferentes;
- adaptar estrategias;
- consultar nuevas fuentes;
- cambiar el orden de ejecución.

Esto significa que resulta imposible asignar privilegios estáticos pensando en un único flujo.

La autoridad debe adaptarse dinámicamente al contexto de la tarea.

Por ejemplo.

Un agente encargado de investigar un incidente puede necesitar:

- consultar logs;
- acceder al SIEM;
- leer información del EDR.

Pero no necesita:

- eliminar endpoints;
- modificar políticas;
- crear administradores;
- ejecutar scripts privilegiados.

Aunque esas herramientas existan dentro del entorno.

El agente nunca debería recibir autoridad sobre ellas si no forman parte del objetivo autorizado.

---

# ⚠️ El mayor error: "darle acceso a todo"

Uno de los errores más comunes en implementaciones tempranas consiste en asignar al agente una identidad altamente privilegiada para evitar problemas de autorización.

Por ejemplo.

```text
Administrator

↓

Agent

↓

Everything
```

Este enfoque simplifica el desarrollo.

Pero destruye completamente el modelo de seguridad.

Ahora cualquier error del agente puede afectar:

- bases de datos;
- infraestructura;
- identidades;
- secretos;
- servicios cloud;
- sistemas críticos.

En otras palabras.

El Blast Radius deja de depender del objetivo inicial y pasa a depender del privilegio máximo otorgado al agente.

---

# 📦 Least Privilege aplicado a herramientas

El Principle of Least Privilege no debería aplicarse únicamente a identidades.

También debería aplicarse a las herramientas disponibles para el agente.

Por ejemplo.

```text
Agent

Tools

✓ Read SIEM

✓ Read EDR

✓ Query CMDB

✗ Delete User

✗ Execute PowerShell

✗ Modify Firewall

✗ Rotate Secrets
```

Aunque el agente conozca la existencia de todas las herramientas.

No todas deberían encontrarse disponibles durante una misma tarea.

La selección de herramientas representa una forma adicional de limitar la autoridad efectiva del agente.

---

# 🎯 Privilegios orientados al objetivo

En sistemas agénticos modernos, los privilegios deberían derivarse del objetivo y no de la identidad permanente del agente.

Supongamos el siguiente objetivo.

> *"Generá un informe de vulnerabilidades críticas."*

Para completar esta tarea el agente necesita:

- consultar el scanner;
- acceder al inventario;
- leer información del CMDB;
- generar un documento.

No necesita:

- modificar vulnerabilidades;
- desplegar parches;
- reiniciar servidores;
- actualizar políticas.

La autoridad debería construirse específicamente alrededor del objetivo recibido.

No alrededor de todas las capacidades potenciales del agente.

---

# ⚙️ Dynamic Least Privilege

En aplicaciones tradicionales, los privilegios suelen permanecer relativamente estables.

En sistemas agénticos esto cambia.

Los privilegios pueden variar dinámicamente según:

- el objetivo;
- el usuario que originó la solicitud;
- el entorno;
- el riesgo;
- las herramientas seleccionadas;
- el momento de ejecución.

Este enfoque suele conocerse como **Dynamic Least Privilege**.

La autoridad deja de ser una configuración fija.

Se convierte en una propiedad calculada para cada contexto específico.

```text
Goal

↓

Risk Evaluation

↓

Required Capabilities

↓

Temporary Privileges

↓

Execution
```

Una vez finalizada la tarea.

Los privilegios desaparecen.

---

# ⏳ Just-in-Time Privileges

Una evolución natural del Least Privilege consiste en otorgar privilegios únicamente durante el tiempo estrictamente necesario.

Este enfoque recibe el nombre de **Just-in-Time (JIT) Privileges**.

En lugar de mantener permisos elevados permanentemente, la arquitectura concede autoridad únicamente cuando una acción concreta lo requiere.

Por ejemplo.

```text
Normal State

↓

Read Only

↓

Task Requires Elevated Action

↓

Temporary Authorization

↓

Execution

↓

Privileges Revoked
```

Este modelo reduce considerablemente la exposición frente a:

- robo de credenciales;
- abuso de herramientas;
- errores del agente;
- compromisos de identidad.

Además, se encuentra alineado con los principios modernos de **Zero Trust** y con las estrategias de administración de identidades privilegiadas (PAM).

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto revisa un agente inteligente, rara vez pregunta:

> **¿Qué permisos tiene?**

En cambio, suele formular preguntas como:

- ¿Qué privilegios necesita realmente para esta tarea?
- ¿Cuáles pueden eliminarse?
- ¿Qué herramientas deberían permanecer ocultas?
- ¿Qué privilegios pueden otorgarse temporalmente?
- ¿Qué acciones requieren una elevación explícita?
- ¿Qué ocurre si el agente interpreta mal el objetivo?

Responder estas preguntas permite construir arquitecturas donde la autoridad deja de ser permanente y pasa a convertirse en un recurso cuidadosamente administrado.

---

# 📌 Cierre de la sección

Aplicar **Least Privilege** en sistemas agénticos no consiste únicamente en reducir permisos.

Consiste en diseñar una arquitectura donde la autoridad evoluciona dinámicamente según el contexto, el objetivo y el riesgo asociado.

Este cambio representa una diferencia fundamental respecto de las aplicaciones tradicionales.

Mientras que antes bastaba con asignar permisos mínimos a un proceso, ahora debemos gobernar cómo un agente adquiere, utiliza y pierde autoridad durante todo su ciclo de razonamiento y ejecución.

En la siguiente sección profundizaremos este concepto analizando cómo esas restricciones se materializan mediante **Scoped Delegation**, **Delegation Constraints** y **Delegation Lifetime**, permitiendo que un agente conserve autonomía sin perder nunca los límites impuestos por la arquitectura.

---
---
---
---
---

---

# 🎯 Scoped Delegation (Advanced)

En las secciones anteriores vimos que la **Delegation** permite transferir parte de la autoridad de una identidad hacia un agente.

También analizamos que el **Principle of Least Privilege** limita la cantidad de privilegios que deberían delegarse.

Sin embargo, todavía existe una pregunta crítica.

> **¿Cómo evitamos que una delegación válida termine convirtiéndose en una fuente de privilegios excesivos?**

La respuesta se encuentra en uno de los principios más importantes de Agentic Security:

**Scoped Delegation**.

Mientras que Least Privilege determina **cuánta autoridad** recibe un agente, Scoped Delegation determina **hasta dónde puede utilizar esa autoridad**.

En otras palabras.

Least Privilege responde:

> **¿Qué puede hacer el agente?**

Scoped Delegation responde:

> **¿Bajo qué condiciones puede hacerlo?**

---

# 🏛️ ¿Qué es Scoped Delegation?

Scoped Delegation consiste en limitar explícitamente el contexto dentro del cual una autoridad delegada permanece válida.

No basta con indicar que un agente puede acceder a una API.

También resulta necesario definir:

- qué operaciones puede ejecutar;
- sobre qué recursos;
- durante cuánto tiempo;
- utilizando qué herramientas;
- bajo qué condiciones;
- dentro de qué entorno;
- para qué objetivo.

Una delegación segura siempre posee un alcance claramente definido.

Fuera de ese alcance, la autoridad deja automáticamente de ser válida.

---

# 🎯 El Scope representa un contrato de seguridad

Puede resultar útil imaginar el Scope como un contrato entre tres actores.

```text
Organization

↓

User

↓

Agent
```

La organización define las políticas.

El usuario delega parte de su autoridad.

El agente únicamente puede actuar dentro del alcance previamente acordado.

Ese alcance constituye el contrato de seguridad.

Modificar cualquiera de sus condiciones debería requerir una nueva autorización.

---

# 📦 Dimensiones del Scope

En sistemas agénticos modernos, el Scope rara vez depende de un único factor.

Normalmente combina múltiples restricciones simultáneamente.

Por ejemplo.

## Scope sobre recursos

```text
Allowed

Customer Database

Inventory

Threat Intelligence

Not Allowed

Financial Database

HR Database
```

---

## Scope sobre herramientas

```text
Allowed

Read CRM

Read SIEM

Generate Report

Blocked

Delete Records

Execute Scripts

Modify Policies
```

---

## Scope sobre operaciones

El agente puede:

- consultar;
- leer;
- resumir;
- clasificar.

Pero no puede:

- eliminar;
- modificar;
- aprobar;
- publicar.

---

## Scope temporal

La delegación permanece válida únicamente durante:

- una sesión;
- una investigación;
- una ventana horaria;
- una tarea específica.

Una vez finalizado ese contexto, la autoridad desaparece automáticamente.

---

## Scope sobre datos

El agente puede acceder únicamente a información clasificada como:

- Public
- Internal

Pero nunca a información:

- Confidential
- Restricted

---

## Scope sobre entorno

La autoridad puede limitarse exclusivamente a:

- Development
- Testing

Y bloquear automáticamente cualquier operación sobre:

- Production

---

# ⚙️ Scope dinámico

En arquitecturas maduras, el Scope no representa una configuración estática.

Puede construirse dinámicamente para cada ejecución.

Por ejemplo.

```text
User Goal

↓

Policy Evaluation

↓

Risk Analysis

↓

Dynamic Scope

↓

Delegation

↓

Execution
```

Esto permite que dos solicitudes aparentemente similares produzcan delegaciones completamente diferentes.

Supongamos los siguientes objetivos.

```text
Summarize Documentation
```

y

```text
Rotate Production Secrets
```

Aunque ambos sean ejecutados por el mismo agente.

Claramente requieren alcances muy distintos.

---

# ⏳ Delegation Lifetime

Otro componente esencial del Scope consiste en la duración de la delegación.

Una autoridad delegada nunca debería permanecer activa indefinidamente.

Toda delegación debería responder claramente la pregunta.

> **¿Cuándo deja de ser válida?**

Existen distintos modelos.

### One-shot Delegation

La autoridad desaparece inmediatamente después de ejecutar una única acción.

```text
Request

↓

Execute

↓

Delegation Ends
```

---

### Session Delegation

Permanece válida únicamente durante la sesión iniciada por el usuario.

Cuando la sesión finaliza.

La delegación también finaliza.

---

### Task-based Delegation

La autoridad permanece activa únicamente mientras exista una tarea específica.

```text
Incident Investigation

↓

Multiple Actions

↓

Incident Closed

↓

Delegation Revoked
```

---

### Time-based Delegation

La delegación expira automáticamente luego de un período determinado.

Por ejemplo.

- 15 minutos.
- 1 hora.
- 24 horas.

Nunca debería depender únicamente de que alguien recuerde eliminarla manualmente.

---

# 🚧 Delegation Constraints

El Scope determina el alcance.

Las **Constraints** determinan las reglas adicionales que limitan el comportamiento del agente.

Podemos imaginar estas restricciones como barreras de seguridad alrededor de la autoridad delegada.

Por ejemplo.

### Restricciones de presupuesto

El agente puede consumir como máximo:

- determinada cantidad de tokens;
- determinado costo;
- determinado número de consultas.

---

### Restricciones de herramientas

Puede utilizar únicamente herramientas previamente aprobadas.

Nunca herramientas descubiertas dinámicamente.

---

### Restricciones de frecuencia

Puede ejecutar como máximo:

- una acción;
- diez consultas;
- cinco modificaciones.

Después deberá solicitar nuevamente autorización.

---

### Restricciones geográficas

La delegación puede resultar válida únicamente desde determinadas regiones o zonas de red.

---

### Restricciones regulatorias

Determinadas acciones pueden requerir aprobación humana independientemente del nivel de autonomía del agente.

---

# 🛡️ Scope y Zero Trust

Uno de los principios fundamentales de Zero Trust establece:

> **Never trust. Always verify.**

En sistemas agénticos este principio continúa siendo válido.

Pero adquiere una nueva interpretación.

Ahora ya no basta con verificar únicamente la identidad.

También resulta necesario verificar continuamente que la acción permanezca dentro del Scope autorizado.

En consecuencia.

Cada herramienta invocada por el agente debería validar nuevamente:

- identidad;
- autoridad;
- alcance;
- políticas;
- contexto.

No únicamente durante la primera acción.

Sino durante toda la ejecución.

---

# 🏗️ Architecture Perspective

Una arquitectura madura evita delegaciones abiertas.

En cambio, cada ejecución atraviesa distintos mecanismos de validación.

```text
Business Goal
        │
        ▼
Risk Evaluation
        │
        ▼
Delegation Scope
        │
        ▼
Policy Engine
        │
        ▼
Constraints Validation
        │
        ▼
Temporary Authority
        │
        ▼
Tool Broker
        │
        ▼
Execution
```

Obsérvese que el Scope deja de ser un atributo del agente.

Pasa a convertirse en una propiedad dinámica construida por la arquitectura antes de cada ejecución.

Este enfoque reduce considerablemente el riesgo de abuso de autoridad y limita el impacto potencial de cualquier compromiso.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza una delegación, rara vez pregunta únicamente:

> **¿Qué puede hacer este agente?**

Las preguntas realmente importantes suelen ser:

- ¿Cuál es el Scope exacto?
- ¿Qué recursos quedaron fuera del Scope?
- ¿Cuándo expira?
- ¿Qué restricciones permanecen activas?
- ¿Qué herramientas nunca deberían formar parte de esta delegación?
- ¿Cómo se valida el Scope durante toda la ejecución?
- ¿Puede modificarse dinámicamente?
- ¿Qué ocurre si el contexto cambia mientras el agente continúa trabajando?

Responder estas preguntas permite construir arquitecturas donde la autoridad permanece constantemente limitada por el contexto y no únicamente por los privilegios iniciales.

---

# 📌 Cierre de la sección

Least Privilege reduce la cantidad de autoridad delegada.

Scoped Delegation garantiza que esa autoridad permanezca confinada dentro de límites claramente definidos.

Delegation Lifetime asegura que la autoridad desaparezca cuando deja de ser necesaria.

Delegation Constraints impiden que el agente utilice esa autoridad fuera del contexto autorizado.

Estos cuatro principios trabajan conjuntamente para transformar una simple delegación en un mecanismo de gobierno capaz de soportar arquitecturas agénticas modernas.

En la siguiente sección analizaremos cómo estos principios evolucionan cuando múltiples agentes comienzan a colaborar entre sí mediante **Delegation Chains**, dando lugar a nuevos desafíos relacionados con propagación de autoridad, límites de confianza y control de privilegios.

---
---
---
---

---

# 🔗 Delegation Chains (Advanced)

Hasta este punto hemos visto cómo un usuario puede delegar parte de su autoridad a un agente para que ejecute una tarea específica.

Sin embargo, las arquitecturas agénticas modernas rara vez están compuestas por un único agente.

A medida que los sistemas evolucionan, resulta cada vez más común encontrar arquitecturas donde un agente coordina el trabajo de otros agentes especializados.

Por ejemplo.

```text
User
   │
   ▼
Coordinator Agent
   │
   ├───────────────┐
   ▼               ▼
Research Agent   Analysis Agent
        │               │
        └──────┬────────┘
               ▼
        Reporting Agent
```

En este escenario aparece un nuevo desafío.

> **¿Cómo se propaga la autoridad a medida que participan múltiples agentes?**

Responder esta pregunta constituye uno de los problemas más importantes de Agentic Security.

---

# 🏛️ ¿Qué es una Delegation Chain?

Una **Delegation Chain** representa la secuencia de delegaciones mediante la cual una autoridad es transferida desde una identidad original hacia uno o más agentes que colaboran para completar un objetivo.

Cada nuevo salto introduce una nueva relación de confianza.

Por ejemplo.

```text
User

↓

Coordinator Agent

↓

Research Agent

↓

Threat Intelligence API
```

Aunque el usuario únicamente interactúe con el primer agente, la autoridad termina siendo utilizada por varios componentes diferentes.

En consecuencia.

Cada eslabón de la cadena debe preservar las restricciones impuestas por la delegación original.

---

# ⚠️ Cada salto modifica el modelo de confianza

En aplicaciones tradicionales la mayoría de las operaciones ocurren dentro de un mismo proceso.

En sistemas agénticos la situación cambia.

Cada agente puede:

- ejecutar herramientas distintas;
- utilizar identidades diferentes;
- operar sobre recursos diferentes;
- interactuar con nuevos servicios;
- iniciar nuevas conversaciones;
- delegar trabajo adicional.

Como consecuencia.

Cada salto modifica el modelo de confianza de la arquitectura.

Podemos imaginar la autoridad como un objeto que viaja entre componentes.

```text
Authority

↓

Agent A

↓

Agent B

↓

Tool

↓

Cloud Service
```

Si alguno de estos componentes amplía la autoridad recibida, la arquitectura pierde control sobre la ejecución.

---

# 🎯 La autoridad debe disminuir, nunca aumentar

Existe un principio ampliamente utilizado en seguridad distribuida.

> **La autoridad nunca debería crecer a medida que avanza una Delegation Chain.**

Cada agente debería recibir únicamente la autoridad mínima necesaria para completar la parte del trabajo que le corresponde.

Por ejemplo.

```text
User Authority

████████████████
        │
        ▼
Coordinator

████████
        │
        ▼
Research Agent

████
        │
        ▼
External Tool

██
```

Obsérvese que cada salto reduce el alcance de la autoridad.

Nunca lo incrementa.

Este principio limita enormemente el impacto potencial de un compromiso.

---

# 🚫 El peligro de la propagación automática

Un error frecuente consiste en permitir que un agente propague automáticamente toda la autoridad recibida hacia cualquier otro agente o herramienta.

Por ejemplo.

```text
User

↓

Agent

↓

Everything inherits everything
```

Aunque este modelo simplifica la implementación, también elimina cualquier separación de privilegios.

Ahora un único compromiso puede afectar toda la cadena.

En arquitecturas maduras ocurre exactamente lo contrario.

Cada nueva delegación requiere una evaluación independiente.

La autoridad no se hereda automáticamente.

Se reconstruye.

---

# 🧩 Trust Boundaries

Cada vez que una delegación atraviesa un nuevo componente aparece un nuevo **Trust Boundary**.

Por ejemplo.

```text
User

↓

Agent

──────────────

Internal API

──────────────

Cloud Service

──────────────

Third-party SaaS
```

Cada frontera representa un cambio en el dominio de confianza.

Y cada cambio requiere responder nuevamente preguntas como:

- ¿La identidad continúa siendo válida?
- ¿El Scope sigue siendo correcto?
- ¿Las políticas permanecen activas?
- ¿La autoridad continúa siendo necesaria?
- ¿La delegación sigue vigente?

Nunca debería asumirse que una autoridad válida en un dominio continúa siendo válida automáticamente en otro.

---

# 🌐 Cross-Agent Delegation

Uno de los escenarios más complejos aparece cuando un agente delega trabajo a otro agente completamente diferente.

Por ejemplo.

```text
SOC Agent

↓

Forensics Agent

↓

Threat Hunting Agent

↓

Reporting Agent
```

Cada uno posee:

- herramientas diferentes;
- objetivos diferentes;
- capacidades diferentes;
- riesgos diferentes.

En consecuencia.

No todos deberían recibir exactamente la misma autoridad.

Una buena práctica consiste en que cada agente reciba únicamente la autoridad necesaria para completar su parte específica del proceso.

No más.

---

# 🔍 Delegation Graphs

En arquitecturas sencillas resulta relativamente fácil comprender cómo fluye la autoridad.

Sin embargo.

En organizaciones grandes pueden existir cientos o miles de agentes colaborando simultáneamente.

En estos escenarios suele resultar útil representar la delegación mediante un **Delegation Graph**.

```text
User
 │
 ├────────────┐
 ▼            ▼
Agent A    Agent B
 │            │
 ▼            ▼
Tool A     Agent C
              │
              ▼
          Cloud API
```

Este tipo de representación permite responder preguntas fundamentales.

Por ejemplo.

- ¿Quién originó esta acción?
- ¿Qué agentes participaron?
- ¿Qué autoridad recibió cada uno?
- ¿Dónde terminó utilizándose esa autoridad?
- ¿Qué componente representa el mayor riesgo?

Los Delegation Graphs comienzan a convertirse en herramientas muy valiosas para auditoría, arquitectura y respuesta ante incidentes.

---

# ⚠️ Anti-patterns

Existen varios patrones de diseño que deberían evitarse.

## Shared Identity

Todos los agentes utilizan exactamente la misma identidad.

Resultado.

No existe trazabilidad.

---

## Unlimited Delegation

Un agente puede volver a delegar toda la autoridad recibida.

Resultado.

La autoridad crece sin control.

---

## Infinite Delegation

No existe un límite sobre la profundidad de la cadena.

Resultado.

La arquitectura pierde completamente el seguimiento de la autoridad.

---

## Static Long-lived Delegation

La autoridad permanece activa durante semanas o meses.

Resultado.

Incrementa enormemente la superficie de ataque.

---

## Hidden Delegation

Los usuarios desconocen completamente que el agente está delegando trabajo hacia otros agentes.

Resultado.

Disminuye la transparencia y la confianza del sistema.

---

# 🏛️ Architecture Perspective

Una arquitectura madura trata cada nueva delegación como una nueva decisión de seguridad.

```text
Business Goal
        │
        ▼
User
        │
Delegation
        │
        ▼
Coordinator Agent
        │
Policy Validation
        │
Scoped Delegation
        │
        ▼
Worker Agent
        │
Identity Verification
        │
        ▼
Tool Broker
        │
Authorization
        │
        ▼
Execution
        │
Audit
```

Obsérvese que ninguna delegación atraviesa la arquitectura de forma transparente.

Cada transición vuelve a verificar:

- identidad;
- políticas;
- Scope;
- restricciones;
- vigencia;
- autoridad.

La confianza nunca se propaga automáticamente.

Debe reconstruirse en cada salto.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto observa una arquitectura con múltiples agentes rara vez pregunta:

> **¿Cuántos agentes participan?**

Las preguntas importantes suelen ser otras.

- ¿Quién originó la autoridad?
- ¿Cuántas veces fue delegada?
- ¿Qué Scope conserva cada agente?
- ¿Puede un agente volver a delegar?
- ¿Qué límites existen sobre esa propagación?
- ¿Cómo se reconstruye toda la cadena durante una investigación?
- ¿Qué ocurre si uno de los agentes resulta comprometido?

Responder estas preguntas permite diseñar arquitecturas donde la colaboración entre agentes no implique una pérdida de control sobre la autoridad.

---

# 📌 Key Ideas

Las arquitecturas agénticas modernas ya no consisten en un único agente ejecutando una tarea.

Cada vez resulta más frecuente encontrar múltiples agentes especializados colaborando para alcanzar un mismo objetivo.

Esta colaboración introduce nuevas cadenas de delegación donde la autoridad viaja entre distintos componentes.

Una arquitectura segura debe garantizar que:

- la autoridad nunca aumente a medida que avanza la cadena;
- cada agente reciba únicamente el Scope necesario;
- cada salto vuelva a validar identidad y políticas;
- las delegaciones sean temporales y revocables;
- toda la cadena permanezca completamente auditable.

Comprender cómo fluye la autoridad entre múltiples agentes constituye uno de los pilares de Agentic Security.

Precisamente porque, cuando una cadena de delegación se diseña incorrectamente, aparecen algunos de los riesgos más importantes de esta disciplina, como **Confused Deputy**, **Privilege Amplification** y la expansión del **Blast Radius**, que desarrollaremos en las próximas secciones.


---
---
---
---

---

# 🎭 Confused Deputy

Hasta este punto hemos analizado cómo una organización puede delegar autoridad a un agente de forma controlada mediante principios como **Least Privilege**, **Scoped Delegation** y **Delegation Chains**.

Sin embargo, incluso una delegación correctamente diseñada puede producir resultados inseguros cuando el agente pierde la capacidad de distinguir **quién solicita una acción** de **con qué autoridad debería ejecutarla**.

Este problema recibe el nombre de **Confused Deputy** y representa uno de los riesgos arquitectónicos más importantes para los sistemas agénticos modernos.

Aunque el concepto fue descrito originalmente hace varias décadas dentro del ámbito de los sistemas operativos, hoy vuelve a cobrar enorme relevancia debido a la aparición de agentes capaces de actuar en nombre de usuarios, organizaciones y otros agentes.

---

# 🎯 ¿Qué es Confused Deputy?

Desde la perspectiva de Agentic Security, un **Confused Deputy** ocurre cuando un agente utiliza correctamente sus propios privilegios, pero los aplica para realizar una acción que beneficia a una identidad que nunca debió recibir esa autoridad.

En otras palabras.

El agente no está comprometido.

No está explotando una vulnerabilidad.

No está actuando con intención maliciosa.

Simplemente ha sido convencido para utilizar su autoridad en un contexto diferente al originalmente autorizado.

El problema no reside en el privilegio.

El problema reside en **cómo se utiliza ese privilegio**.

---

# 🏛️ El origen del problema

El término **Confused Deputy** fue introducido por el científico de la computación **Norman Hardy** en la década de 1980 para describir un escenario donde un programa con privilegios elevados realizaba accidentalmente acciones en beneficio de un usuario que no poseía dichos privilegios.

El programa actuaba correctamente.

La autoridad era legítima.

Sin embargo.

El contexto era incorrecto.

Décadas después.

El mismo problema aparece nuevamente en sistemas modernos.

Solo que ahora el "Deputy" ya no es únicamente un programa.

Puede ser:

- un agente inteligente;
- un asistente corporativo;
- un orquestador;
- un agente MCP;
- un sistema multiagente;
- una herramienta integrada mediante Tool Calling.

---

# 🤖 ¿Por qué reaparece con la IA?

Los agentes modernos poseen una característica que las aplicaciones tradicionales no tenían.

No ejecutan únicamente funciones.

Interpretan objetivos.

Construyen planes.

Seleccionan herramientas.

Y toman decisiones.

Esto significa que constantemente deben responder preguntas como:

- ¿Quién solicitó esta acción?
- ¿Estoy autorizado para realizarla?
- ¿Con qué identidad debería ejecutarla?
- ¿Qué alcance tiene la delegación?

Cuando estas respuestas dejan de estar claramente separadas aparece el riesgo de Confused Deputy.

---

# 📦 Un ejemplo sencillo

Supongamos un agente conectado al sistema de tickets de una organización.

El administrador posee permisos para cerrar cualquier incidente.

Un empleado común únicamente puede cerrar los tickets que él mismo creó.

Ahora imaginemos el siguiente escenario.

El empleado solicita al agente.

> "Cerrá el incidente #482."

El agente observa que posee autoridad suficiente para cerrar el ticket.

Y simplemente ejecuta la acción.

```text
Employee

↓

Agent

↓

Close Ticket

↓

Done
```

Aparentemente todo funciona correctamente.

Sin embargo.

El incidente pertenecía a otro equipo.

El agente utilizó su propia autoridad para beneficiar a un usuario que nunca debió poseer ese privilegio.

Ese es un Confused Deputy.

---

# 🧩 El agente no está comprometido

Este punto resulta extremadamente importante.

En un Confused Deputy el agente normalmente:

- autentica correctamente;
- autoriza correctamente;
- ejecuta correctamente.

El problema aparece porque utiliza una autoridad válida para resolver un objetivo que nunca debió aceptar.

Desde el punto de vista técnico.

Todo parece correcto.

Desde el punto de vista del negocio.

Se produjo una violación del modelo de autorización.

---

# 🔄 Confused Deputy en sistemas multiagente

El riesgo aumenta considerablemente cuando participan múltiples agentes.

Por ejemplo.

```text
User

↓

Coordinator Agent

↓

Research Agent

↓

Database Agent
```

El último agente posee permisos para consultar información confidencial.

Sin embargo.

Nunca verifica si el usuario original realmente estaba autorizado para acceder a esos datos.

Simplemente confía en el agente anterior.

En este escenario.

Toda la cadena de confianza queda comprometida.

---

# ⚠️ Confused Deputy y Tool Calling

Uno de los escenarios donde este problema aparece con mayor frecuencia consiste en el uso de herramientas externas.

Por ejemplo.

```text
User

↓

Agent

↓

CRM

↓

Payment API

↓

Cloud Storage
```

Si el agente selecciona automáticamente una herramienta utilizando únicamente sus propios privilegios, puede terminar ejecutando acciones completamente válidas desde el punto de vista técnico, pero completamente incorrectas desde la perspectiva del negocio.

Esto explica por qué muchas arquitecturas modernas separan:

- el razonamiento;
- la autorización;
- la identidad;
- la ejecución.

---

# 🔒 ¿Cómo prevenirlo?

No existe un único mecanismo.

Normalmente es necesario combinar varios principios arquitectónicos.

Entre ellos.

- Least Privilege.
- Scoped Delegation.
- Delegated Identity.
- Policy Enforcement.
- Context Validation.
- Fine-grained Authorization.
- Zero Trust.
- Continuous Authorization.

El objetivo consiste en garantizar que el agente nunca pueda utilizar una autoridad fuera del contexto para el cual fue delegada.

---

# 🏛️ Architecture Perspective

Una arquitectura madura evita que el agente decida por sí mismo qué autoridad utilizar.

En cambio.

Cada acción vuelve a validar el contexto.

```text
Business Goal
       │
       ▼
User
       │
Delegation
       │
       ▼
Agent
       │
Context Validation
       │
Policy Engine
       │
Identity Verification
       │
Authorization
       │
Tool Broker
       │
Execution
```

Obsérvese que la autoridad nunca se asume.

Debe reconstruirse y verificarse antes de cada acción relevante.

Este principio constituye uno de los pilares de Zero Trust aplicado a sistemas agénticos.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza un agente inteligente, rara vez pregunta:

> **¿Tiene permisos para ejecutar esta acción?**

Las preguntas realmente importantes son otras.

- ¿Quién originó la solicitud?
- ¿Con qué identidad se ejecutará?
- ¿La autoridad continúa siendo válida?
- ¿El Scope sigue siendo el mismo?
- ¿El usuario original podría realizar directamente esta operación?
- ¿El agente está utilizando privilegios propios o autoridad delegada?
- ¿Qué ocurriría si otro agente invocara exactamente esta misma herramienta?

Responder estas preguntas permite detectar escenarios donde una arquitectura aparentemente correcta podría convertirse en un **Confused Deputy**.

---

# 📌 Cierre de la sección

El problema del **Confused Deputy** no consiste en que un agente posea demasiados privilegios.

Consiste en que utiliza privilegios legítimos fuera del contexto para el cual fueron delegados.

En consecuencia.

La protección frente a este riesgo no depende únicamente del modelo de IA.

Depende principalmente de la arquitectura encargada de gobernar identidades, delegaciones, políticas y contexto.

Comprender este patrón resulta fundamental porque prepara el terreno para un riesgo aún más complejo.

En la siguiente sección analizaremos cómo una delegación incorrecta puede provocar que un agente adquiera progresivamente más autoridad de la que originalmente recibió, dando lugar al fenómeno conocido como **Privilege Amplification**.


---
---
---
---

---

# 📈 Privilege Amplification

En la sección anterior analizamos cómo un agente puede convertirse en un **Confused Deputy**, utilizando correctamente una autoridad legítima dentro de un contexto incorrecto.

Sin embargo, existe un riesgo aún más peligroso.

> **¿Qué ocurre cuando un agente termina obteniendo más autoridad de la que originalmente recibió?**

Este fenómeno se conoce como **Privilege Amplification** (también denominado *Privilege Escalation* cuando ocurre como consecuencia de una vulnerabilidad específica).

Desde la perspectiva de Agentic Security, representa uno de los mayores riesgos arquitectónicos asociados a la delegación, la autonomía y la colaboración entre múltiples agentes.

Mientras que el **Confused Deputy** utiliza incorrectamente una autoridad existente, **Privilege Amplification** implica que el agente termina actuando con un nivel de autoridad superior al originalmente autorizado.

---

# 🎯 ¿Qué es Privilege Amplification?

Desde la perspectiva de Agentic Security, **Privilege Amplification** ocurre cuando un agente obtiene, directa o indirectamente, privilegios adicionales que exceden el alcance de la autoridad originalmente delegada.

Es importante destacar que esta situación no siempre implica la explotación de una vulnerabilidad clásica.

Puede producirse simplemente como consecuencia de una arquitectura que permite combinar herramientas, identidades o delegaciones sin aplicar restricciones suficientes.

En otras palabras.

El problema no consiste únicamente en que el agente tenga permisos elevados.

El problema aparece cuando esos permisos aumentan progresivamente durante la ejecución sin una autorización explícita.

---

# 🏛️ ¿Cómo aparece este problema?

En sistemas tradicionales los privilegios suelen asignarse antes de ejecutar un proceso.

Una vez iniciada la aplicación, el conjunto de permisos permanece relativamente estable.

Los agentes inteligentes modifican completamente este escenario.

Durante una misma ejecución pueden:

- invocar nuevas herramientas;
- consultar diferentes APIs;
- colaborar con otros agentes;
- cambiar de contexto;
- utilizar nuevas identidades;
- solicitar autorizaciones adicionales;
- ejecutar múltiples tareas consecutivas.

Cada una de estas interacciones representa una oportunidad para que la autoridad aumente accidentalmente.

Por este motivo, la autoridad deja de ser una propiedad estática.

Se convierte en un flujo dinámico que debe ser gobernado continuamente.

---

# 📦 Un ejemplo sencillo

Supongamos un agente cuyo objetivo consiste en generar un informe sobre vulnerabilidades.

Inicialmente posee permisos únicamente para consultar información.

```text
Read Scanner

↓

Read CMDB

↓

Generate Report
```

Durante la ejecución descubre que necesita conocer el estado de determinados servidores.

Para ello consulta otro servicio.

Ese servicio devuelve credenciales con privilegios administrativos.

Ahora el agente puede:

- consultar información;
- modificar configuraciones;
- reiniciar servidores;
- crear nuevos usuarios.

Nada de esto formaba parte del objetivo original.

La autoridad aumentó durante la ejecución.

Ese incremento constituye una amplificación de privilegios.

---

# 🔄 Amplificación progresiva

Uno de los aspectos más peligrosos consiste en que la amplificación rara vez ocurre de manera instantánea.

Normalmente aparece como una secuencia de pequeños incrementos.

```text
User

↓

Read Access

↓

Agent

↓

Additional Tool

↓

Temporary Credential

↓

Administrative API

↓

Full Control
```

Cada paso parece razonable de forma aislada.

Sin embargo.

El resultado final es un agente que posee mucha más autoridad que la originalmente autorizada.

Este fenómeno suele pasar desapercibido cuando la arquitectura analiza únicamente cada acción individual y no la evolución completa de la autoridad.

---

# 🧩 Amplificación mediante Tool Calling

Los sistemas agénticos modernos utilizan con frecuencia herramientas externas.

Por ejemplo.

```text
Agent

↓

CRM

↓

Cloud Storage

↓

Identity Provider

↓

CI/CD Pipeline
```

Cada herramienta puede exponer nuevas capacidades.

Si el agente puede descubrir herramientas dinámicamente o reutilizar credenciales entre servicios, la autoridad puede crecer progresivamente.

Por ejemplo.

Una API aparentemente inocua podría devolver un token con permisos mucho más amplios que los necesarios.

A partir de ese momento, el agente ya no opera con la autoridad original.

Opera con la autoridad del nuevo servicio.

---

# 🔗 Amplificación en cadenas de delegación

Las **Delegation Chains** también pueden producir este problema.

Supongamos la siguiente arquitectura.

```text
User

↓

Coordinator Agent

↓

Operations Agent

↓

Cloud Agent
```

El usuario únicamente autorizó consultar recursos.

Sin embargo.

El último agente posee permisos para crear infraestructura.

Si la arquitectura no verifica el Scope original, el resultado será una ampliación efectiva de la autoridad.

Cada salto dentro de una Delegation Chain representa una oportunidad para que aparezca Privilege Amplification.

Por este motivo.

La autoridad nunca debería reconstruirse únicamente a partir de la identidad del agente actual.

Debe conservar permanentemente el contexto de la delegación original.

---

# ⚠️ ¿Por qué resulta tan peligroso?

La amplificación de privilegios incrementa simultáneamente varios riesgos.

Por ejemplo.

- aumenta el Blast Radius;
- dificulta la auditoría;
- rompe el Principle of Least Privilege;
- facilita movimientos laterales;
- incrementa el impacto de Prompt Injection;
- favorece ataques sobre herramientas;
- dificulta la revocación.

En consecuencia.

Un agente inicialmente inofensivo puede terminar convirtiéndose en un componente con capacidad para afectar activos críticos.

---

# 🛡️ Cómo prevenir Privilege Amplification

No existe un único control capaz de eliminar completamente este riesgo.

La prevención normalmente requiere combinar múltiples principios arquitectónicos.

## Least Privilege

El agente nunca debería recibir más autoridad de la estrictamente necesaria.

---

## Scoped Delegation

Toda autoridad debe permanecer limitada al objetivo autorizado.

---

## Continuous Authorization

Cada acción relevante debería volver a verificar:

- identidad;
- contexto;
- Scope;
- políticas;
- restricciones.

Nunca debería asumirse que una autorización inicial continúa siendo válida durante toda la ejecución.

---

## Identity Propagation

Cada herramienta debería conocer:

- quién originó la solicitud;
- qué autoridad fue delegada;
- cuáles eran las restricciones originales.

No únicamente la identidad del agente que realiza la llamada.

---

## Tool Isolation

Las herramientas deberían operar de forma aislada.

Una herramienta nunca debería otorgar automáticamente autoridad adicional a otra.

---

## Temporary Credentials

Siempre que resulte posible, las credenciales utilizadas por el agente deberían:

- ser temporales;
- poseer un Scope reducido;
- expirar automáticamente.

---

## Policy Enforcement

Las políticas deberían impedir explícitamente que un agente solicite privilegios superiores a los autorizados durante la delegación inicial.

---

# 🏛️ Architecture Perspective

Una arquitectura madura trata la autoridad como un recurso que nunca puede crecer espontáneamente.

```text
Business Goal
       │
       ▼
Delegation
       │
       ▼
Agent
       │
Current Scope
       │
Policy Validation
       │
Identity Propagation
       │
Tool Authorization
       │
Execution
```

Obsérvese que el agente nunca puede ampliar unilateralmente su autoridad.

Cada nueva capacidad requiere una validación independiente.

La autoridad únicamente puede mantenerse o reducirse.

Nunca aumentar automáticamente.

Este principio constituye uno de los pilares de Zero Trust aplicado a sistemas agénticos.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza una arquitectura con múltiples agentes rara vez pregunta:

> **¿Qué privilegios tiene este agente?**

Las preguntas realmente importantes suelen ser otras.

- ¿Puede adquirir nuevos privilegios durante la ejecución?
- ¿Quién autoriza ese incremento?
- ¿Puede una herramienta ampliar la autoridad del agente?
- ¿Puede otro agente delegarle nuevos permisos?
- ¿La autoridad actual continúa respetando el Scope original?
- ¿Cómo se detecta una amplificación de privilegios?
- ¿Cómo puede revertirse inmediatamente?

Responder estas preguntas permite detectar problemas arquitectónicos que normalmente permanecen ocultos durante las pruebas funcionales.

---

# 📌 Cierre de la sección

La **Privilege Amplification** representa uno de los riesgos más complejos de Agentic Security porque no depende únicamente de una vulnerabilidad puntual.

Surge cuando la arquitectura permite que la autoridad evolucione sin mantener una relación estricta con la delegación original.

Mientras que **Least Privilege** limita la autoridad inicial y **Scoped Delegation** define su alcance, la prevención de Privilege Amplification exige un control continuo sobre cómo esa autoridad se utiliza, se propaga y cambia durante toda la ejecución.

En la siguiente sección analizaremos las consecuencias prácticas de este fenómeno introduciendo un concepto estrechamente relacionado:

> **Blast Radius**, es decir, el impacto máximo que puede producir un agente cuando un error, una vulnerabilidad o una decisión incorrecta logra escapar de los límites impuestos por la arquitectura.


---
---
---
---

---

# 💥 Blast Radius

Hasta este punto hemos visto cómo una arquitectura incorrecta puede permitir que un agente utilice autoridad fuera de contexto (**Confused Deputy**) o adquiera progresivamente más privilegios de los originalmente delegados (**Privilege Amplification**).

Sin embargo, todavía queda una pregunta fundamental.

> **¿Cuál es el peor daño que podría producir este agente si algo sale mal?**

Responder esta pregunta constituye uno de los principios más importantes del diseño seguro.

Ese concepto recibe el nombre de **Blast Radius**.

Mientras que las vulnerabilidades describen cómo ocurre un incidente, el Blast Radius describe **hasta dónde puede llegar ese incidente**.

En otras palabras.

No todos los errores tienen el mismo impacto.

Dos agentes pueden sufrir exactamente el mismo problema.

La diferencia estará determinada por cuánto alcance posee cada uno.

---

# 🎯 ¿Qué es Blast Radius?

Desde la perspectiva de Agentic Security, el **Blast Radius** representa el alcance máximo de impacto que puede producir un agente cuando ocurre una falla de seguridad, un error de razonamiento, una vulnerabilidad o un abuso de autoridad.

El concepto proviene del ámbito militar.

La explosión de un artefacto posee un radio de daño determinado.

Todo aquello que se encuentre dentro de ese radio puede verse afectado.

En seguridad ocurre exactamente lo mismo.

Cada identidad, herramienta, privilegio o delegación define el radio máximo de impacto que puede alcanzar un incidente.

Cuanto mayor sea ese radio, mayor será el riesgo para la organización.

---

# 🏗️ El Blast Radius no depende únicamente del agente

Uno de los errores más frecuentes consiste en pensar que el Blast Radius depende exclusivamente del modelo de IA.

No es así.

Depende principalmente de la arquitectura.

Por ejemplo.

Dos organizaciones pueden utilizar exactamente el mismo modelo.

Sin embargo.

La primera permite que el agente:

- administre infraestructura;
- modifique identidades;
- acceda a secretos;
- ejecute comandos administrativos.

Mientras que la segunda únicamente permite:

- consultar documentación;
- generar reportes;
- clasificar información.

El modelo es exactamente el mismo.

El Blast Radius no.

Esto demuestra que el impacto potencial de un incidente depende mucho más de la arquitectura que de las capacidades del modelo.

---

# 📦 Un ejemplo práctico

Supongamos un agente encargado de responder preguntas sobre documentación técnica.

```text
User

↓

Agent

↓

Knowledge Base

↓

Answer
```

Ahora imaginemos que un atacante consigue modificar el contexto mediante un Prompt Injection.

El impacto probablemente se limite a respuestas incorrectas.

Comparemos ese escenario con otro.

```text
User

↓

Agent

↓

Identity Provider

↓

Cloud Platform

↓

Production
```

El mismo Prompt Injection ahora podría terminar creando usuarios privilegiados, modificando infraestructura o eliminando recursos críticos.

La vulnerabilidad es la misma.

El Blast Radius es completamente diferente.

---

# 📊 Factores que aumentan el Blast Radius

Diversos factores incrementan el impacto potencial de un incidente.

Por ejemplo.

## Exceso de privilegios

Un agente con permisos administrativos siempre posee un Blast Radius mayor que uno con permisos de solo lectura.

---

## Demasiadas herramientas

Cada nueva herramienta representa una nueva capacidad.

Y cada capacidad amplía el conjunto de acciones posibles.

---

## Delegaciones largas

Cuantos más agentes participan en una Delegation Chain, mayor resulta el número de componentes potencialmente afectados.

---

## Acceso a múltiples dominios

Un agente que puede interactuar con:

- correo electrónico;
- CRM;
- nube;
- repositorios;
- identidad;
- bases de datos;

posee un Blast Radius considerablemente mayor que otro especializado en una única función.

---

## Credenciales permanentes

Las identidades de larga duración incrementan notablemente el impacto potencial de un compromiso.

---

# 🛡️ Reduciendo el Blast Radius

Uno de los principios fundamentales de Agentic Security consiste en asumir que eventualmente ocurrirá algún error.

La pregunta deja de ser:

> **¿Puede fallar este agente?**

Y pasa a convertirse en:

> **¿Qué tan grave será ese fallo cuando ocurra?**

Entre las mejores prácticas para reducir el Blast Radius encontramos:

- Least Privilege.
- Scoped Delegation.
- Temporary Credentials.
- Identity Isolation.
- Tool Isolation.
- Segmentation.
- Environment Separation.
- Continuous Authorization.
- Runtime Policies.

Todos estos mecanismos buscan exactamente el mismo objetivo.

Reducir el impacto máximo de cualquier incidente.

---

# ⏹️ Revocation

Hasta ahora hemos visto cómo limitar la autoridad.

Sin embargo.

Toda arquitectura madura también debe responder otra pregunta.

> **¿Cómo detenemos inmediatamente a un agente cuando algo sale mal?**

La respuesta se encuentra en el concepto de **Revocation**.

Revocation representa la capacidad de retirar inmediatamente una autoridad previamente delegada.

No importa cuán correcta haya sido la delegación original.

Si el contexto cambia, la autoridad también debe poder cambiar.

---

# 🎯 ¿Cuándo debería revocarse una delegación?

Existen numerosos escenarios.

Por ejemplo.

- finalizó la tarea;
- cambió el objetivo;
- el usuario cerró la sesión;
- se detectó comportamiento anómalo;
- ocurrió un incidente de seguridad;
- cambió una política;
- expiró el tiempo autorizado;
- la identidad fue comprometida;
- el agente abandonó el Scope permitido.

En todos estos casos.

La arquitectura debería ser capaz de retirar inmediatamente la autoridad del agente.

---

# ⚠️ Revocar no significa apagar el agente

Otro error frecuente consiste en pensar que revocar autoridad implica detener completamente el sistema.

No necesariamente.

Un agente puede continuar razonando.

Puede continuar planificando.

Puede continuar analizando información.

Lo que cambia es su capacidad para ejecutar acciones.

Por ejemplo.

```text
Reasoning

✓ Allowed

Planning

✓ Allowed

Execution

✗ Revoked
```

Esta separación permite mantener productividad sin comprometer la seguridad.

---

# 🏛️ Architecture Perspective

Una arquitectura madura trata la autoridad como un recurso dinámico.

No como una propiedad permanente.

```text
Business Goal
       │
       ▼
Delegation
       │
       ▼
Policy Engine
       │
       ▼
Temporary Authority
       │
       ▼
Execution
       │
Continuous Monitoring
       │
       ▼
Revocation Engine
       │
       ▼
Authority Removed
```

Obsérvese que la autoridad permanece constantemente supervisada.

No basta con validarla al comienzo de la ejecución.

Debe poder retirarse inmediatamente cuando el contexto deja de ser válido.

Este principio resulta especialmente importante en arquitecturas con agentes de larga duración o múltiples cadenas de delegación.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza un agente inteligente, rara vez pregunta únicamente:

> **¿Puede ejecutar esta acción?**

Las preguntas realmente importantes suelen ser:

- ¿Cuál es el Blast Radius máximo?
- ¿Qué activos podrían verse afectados?
- ¿Qué ocurre si el agente interpreta incorrectamente el objetivo?
- ¿Qué herramientas incrementan innecesariamente ese impacto?
- ¿Qué mecanismos reducen el alcance del incidente?
- ¿Cómo se revoca inmediatamente la autoridad?
- ¿Qué acciones continúan siendo posibles después de la revocación?

Responder estas preguntas permite diseñar arquitecturas donde un error inevitable no se transforme automáticamente en un incidente catastrófico.

---

# 📌 Key Ideas

Los sistemas agénticos no pueden diseñarse suponiendo que nunca ocurrirá un fallo.

Deben diseñarse asumiendo exactamente lo contrario.

En ese contexto:

- **Confused Deputy** recuerda que la autoridad puede utilizarse fuera de contexto.
- **Privilege Amplification** demuestra que la autoridad puede crecer durante la ejecución.
- **Blast Radius** obliga a medir el impacto máximo de cualquier error.
- **Revocation** garantiza que la autoridad pueda retirarse inmediatamente cuando el contexto cambia.

Estos cuatro conceptos constituyen algunos de los pilares más importantes de Agentic Security porque desplazan el foco desde la simple protección del modelo hacia el verdadero objetivo de una arquitectura segura:

> **Mantener el control sobre la autoridad, incluso cuando los agentes toman decisiones y actúan de manera autónoma.**

Con esta sección finaliza el recorrido sobre **Autonomy**, **Agency** y **Delegation**.

A partir de estos principios resulta posible comprender cómo diseñar arquitecturas agénticas donde la inteligencia del agente permanezca permanentemente gobernada por políticas, identidades, límites de autoridad y mecanismos de control, alineando la autonomía con los principios clásicos de seguridad y con las nuevas necesidades de los sistemas basados en IA.

---
---
---
---

---

# 🏛️ Designing Secure Agentic Systems

A lo largo de este capítulo analizamos cómo los agentes adquieren capacidades, toman decisiones, reciben autoridad y ejecutan acciones en nombre de usuarios y organizaciones.

Aunque cada uno de estos conceptos puede estudiarse de manera individual, la verdadera seguridad de un sistema agéntico no depende de un único mecanismo.

Depende de la forma en que todos ellos interactúan dentro de una misma arquitectura.

Un agente seguro no es simplemente un agente con pocos privilegios.

Tampoco un agente con poca autonomía.

Un agente seguro es aquel cuya capacidad para razonar, decidir y actuar permanece permanentemente gobernada por políticas, identidades, restricciones y mecanismos de supervisión.

La seguridad deja de centrarse únicamente en proteger aplicaciones.

Comienza a centrarse en gobernar decisiones.

---

# 🎯 Secure Delegation Design Principles

Los principios desarrollados durante este capítulo pueden resumirse en una serie de reglas de diseño.

## 1. Delegate Authority, Never Trust

La autoridad nunca debe asumirse.

Debe delegarse explícitamente.

Toda acción ejecutada por un agente debería poder responder:

- ¿Quién originó esta solicitud?
- ¿Quién delegó la autoridad?
- ¿Cuál era el objetivo original?
- ¿Cuál es el Scope autorizado?

---

## 2. Authority Should Always Be Smaller Than Capability

Las capacidades representan todo aquello que un agente podría hacer.

La autoridad representa aquello que realmente tiene permitido hacer.

Nunca deberían coincidir completamente.

```text
Capabilities

████████████████████

Authority

██████
```

La diferencia entre ambos conjuntos constituye una de las barreras de seguridad más importantes de toda la arquitectura.

---

## 3. Every Delegation Must Expire

La autoridad nunca debería ser permanente.

Cada delegación debería responder claramente:

- ¿Cuándo comienza?
- ¿Cuándo finaliza?
- ¿Qué evento la invalida?

La mejor delegación es aquella que desaparece automáticamente cuando deja de ser necesaria.

---

## 4. Context Is Part of Authorization

Una autorización nunca debería depender únicamente de la identidad.

También debe considerar:

- contexto;
- objetivo;
- herramienta;
- entorno;
- riesgo;
- tiempo;
- usuario original.

La misma acción puede resultar válida en un contexto e insegura en otro.

---

## 5. Agents Should Never Increase Their Own Authority

Un agente puede:

- planificar;
- decidir;
- ejecutar.

Pero nunca debería ampliar unilateralmente la autoridad que recibió.

Todo incremento de privilegios debe requerir una nueva decisión de la arquitectura.

Nunca del propio agente.

---

## 6. Every Decision Should Be Observable

No basta con registrar las acciones.

También resulta necesario comprender:

- qué decidió el agente;
- por qué;
- utilizando qué contexto;
- mediante qué herramientas;
- bajo qué políticas.

La observabilidad constituye uno de los pilares fundamentales de Agentic Security.

---

## 7. Every Action Must Be Revocable

Toda arquitectura debería asumir que, tarde o temprano, será necesario detener un agente.

Por este motivo.

Toda autoridad debe poder retirarse inmediatamente.

No importa si el problema proviene de:

- un error;
- una vulnerabilidad;
- una herramienta;
- una política incorrecta;
- un Prompt Injection;
- un cambio de contexto.

La capacidad de revocar autoridad representa una propiedad esencial de cualquier arquitectura madura.

---

# 📋 Agentic Security Checklist

Antes de permitir que un agente interactúe con sistemas reales, resulta recomendable verificar aspectos como los siguientes.

## Identity

- [ ] El agente posee una identidad propia.
- [ ] La identidad resulta auditable.
- [ ] No comparte credenciales con otros agentes.

---

## Delegation

- [ ] La autoridad fue delegada explícitamente.
- [ ] Existe un Scope claramente definido.
- [ ] La delegación posee fecha de expiración.
- [ ] Puede revocarse inmediatamente.

---

## Least Privilege

- [ ] El agente únicamente posee los privilegios necesarios.
- [ ] Las herramientas innecesarias permanecen bloqueadas.
- [ ] Los privilegios elevados son temporales.

---

## Policies

- [ ] Existe un Policy Decision Point.
- [ ] Existe un Policy Enforcement Point.
- [ ] Las políticas se validan durante toda la ejecución.

---

## Human Oversight

- [ ] Existe un modelo de supervisión definido.
- [ ] Se documentó cuándo utilizar HITL, HOTL o HOOTL.
- [ ] Existen mecanismos para detener la ejecución.

---

## Observability

- [ ] Todas las acciones quedan registradas.
- [ ] Puede reconstruirse toda la cadena de delegación.
- [ ] Se registran las decisiones importantes del agente.

---

## Risk

- [ ] Se evaluó el Blast Radius.
- [ ] Se documentaron las Trust Boundaries.
- [ ] Se analizaron escenarios de Privilege Amplification.
- [ ] Se revisaron posibles Confused Deputies.

---

# 🏗️ Architecture Perspective

Una arquitectura madura distribuye las responsabilidades entre múltiples componentes especializados.

```text
Business Goal
        │
        ▼
User
        │
Authentication
        │
Authorization
        │
Delegation
        │
Scoped Delegation
        │
Agent
        │
Planning
        │
Policy Decision Point
        │
Policy Enforcement Point
        │
Identity Propagation
        │
Tool Broker
        │
Execution
        │
Monitoring
        │
Audit
        │
Revocation
```

Obsérvese que el agente no controla la arquitectura.

La arquitectura controla al agente.

La inteligencia permanece dentro del modelo.

La autoridad permanece fuera del modelo.

Esa separación constituye uno de los principios más importantes de Agentic Security.

---

# 💡 Think Like an Agentic Security Architect

Cuando un arquitecto analiza un sistema agéntico, rara vez comienza preguntando:

> **¿Qué modelo de IA vamos a utilizar?**

Las primeras preguntas suelen ser otras.

- ¿Dónde nace la autoridad?
- ¿Quién conserva el control?
- ¿Cómo viaja la identidad entre componentes?
- ¿Qué ocurre cuando participan múltiples agentes?
- ¿Qué políticas gobiernan las herramientas?
- ¿Qué decisiones requieren aprobación?
- ¿Qué acciones pueden revertirse?
- ¿Qué ocurre si un agente resulta comprometido?
- ¿Cuál es el Blast Radius máximo aceptable?
- ¿Cómo detenemos inmediatamente toda la ejecución?

Estas preguntas desplazan el foco desde el modelo hacia la arquitectura.

Y precisamente allí reside la esencia de Agentic Security.

---

# 📌 Key Takeaways

A lo largo de este capítulo desarrollamos un modelo completo para comprender cómo los agentes adquieren, utilizan y pierden autoridad.

Los conceptos pueden resumirse de la siguiente manera.

- **Capability** describe lo que un agente es técnicamente capaz de hacer.
- **Permission** determina qué acciones tiene permitidas.
- **Authority** establece bajo qué identidad puede actuar.
- **Agency** representa su capacidad para decidir.
- **Autonomy** determina cuánto puede ejecutar sin intervención humana.
- **Human Oversight** mantiene el control sobre la ejecución.
- **Delegation** distribuye autoridad entre identidades.
- **Least Privilege** limita esa autoridad.
- **Scoped Delegation** restringe su alcance.
- **Delegation Chains** gobiernan cómo fluye entre múltiples agentes.
- **Confused Deputy** evita utilizar autoridad fuera de contexto.
- **Privilege Amplification** impide que esa autoridad crezca durante la ejecución.
- **Blast Radius** limita el impacto máximo de un incidente.
- **Revocation** garantiza que toda autoridad pueda retirarse inmediatamente.

Ninguno de estos conceptos resulta suficiente por sí solo.

La seguridad emerge únicamente cuando todos ellos trabajan de forma coordinada dentro de una arquitectura diseñada para gobernar agentes inteligentes.

---

# 📚 References

Los conceptos desarrollados en este capítulo se encuentran alineados con principios ampliamente aceptados por la industria y sintetizan ideas presentes en distintos estándares, marcos de referencia y documentos técnicos.

- NIST AI Risk Management Framework (AI RMF 1.0)
- NIST SP 800-207 — Zero Trust Architecture
- NIST SP 800-53 Rev. 5 — Security and Privacy Controls for Information Systems and Organizations
- NIST SP 800-160 Vol. 1 — Systems Security Engineering
- OWASP Agentic Security Initiative
- OWASP Top 10 for LLM Applications
- OWASP Application Security Verification Standard (ASVS)
- Google Secure AI Framework (SAIF)
- Microsoft AI Security & Responsible AI
- Anthropic — Building Safe and Trustworthy AI Systems
- OpenAI Model Spec
- OAuth 2.0 / OAuth 2.1
- SPIFFE & SPIRE — Secure Identity for Distributed Systems
- MITRE ATLAS — Adversarial Threat Landscape for Artificial-Intelligence Systems
- Capability-Based Security (Norman Hardy y trabajos posteriores)

---

> **La inteligencia de un agente determina lo que puede lograr. La arquitectura de seguridad determina lo que realmente debería estar autorizado a hacer. Diseñar sistemas agénticos seguros consiste, precisamente, en mantener esa diferencia bajo control.**
