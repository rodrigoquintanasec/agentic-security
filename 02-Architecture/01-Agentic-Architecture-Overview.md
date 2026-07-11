# 🏗️ Agentic Architecture Overview

> Antes de proteger un sistema agéntico es necesario comprender cómo está construido. Una arquitectura segura comienza entendiendo los componentes que participan en el ciclo de vida de un agente y cómo interactúan entre sí.

---

# 🎯 Objetivo

Comprender la arquitectura general de un sistema agéntico moderno, identificar sus principales componentes y desarrollar un modelo mental que sirva como base para los capítulos posteriores del repositorio.

---

# 📖 Introducción

Durante muchos años, las aplicaciones tradicionales siguieron una arquitectura relativamente simple.

Un usuario enviaba una solicitud.

La aplicación procesaba esa solicitud.

Consultaba una base de datos.

Y devolvía una respuesta.

```text
User
   │
   ▼
Application
   │
   ▼
Database
   │
   ▼
Response
```

La lógica de negocio estaba completamente definida por el código escrito por los desarrolladores.

Cada solicitud seguía un flujo relativamente determinístico.

---

Los sistemas agénticos cambian completamente este paradigma.

Un agente no solamente ejecuta código.

También puede:

- interpretar objetivos;
- razonar;
- planificar múltiples pasos;
- seleccionar herramientas;
- consultar memoria;
- recuperar conocimiento;
- colaborar con otros agentes;
- decidir cuál será la siguiente acción.

Como consecuencia, la arquitectura deja de ser un flujo lineal y pasa a convertirse en un ecosistema de componentes que colaboran continuamente.

Comprender esta arquitectura constituye el primer paso para diseñar sistemas seguros.

---

# 🏛️ ¿Qué es una arquitectura agéntica?

Una **Agentic Architecture** representa la organización de todos los componentes que permiten que un agente inteligente perciba información, razone, tome decisiones y ejecute acciones para alcanzar un objetivo.

No describe un producto específico.

No describe un framework.

Describe un conjunto de responsabilidades que aparecen prácticamente en cualquier sistema moderno basado en agentes.

Esto significa que, independientemente de utilizar OpenAI Agents SDK, LangGraph, CrewAI, Semantic Kernel, Google ADK o cualquier otra plataforma, la mayoría de las arquitecturas comparten elementos similares.

---

# 🧩 Componentes principales

Aunque cada implementación presenta diferencias, la mayoría de los sistemas agénticos incorporan componentes como los siguientes.

- Usuario
- Agente
- Modelo de IA (LLM)
- Planificador
- Memoria
- Fuentes de conocimiento
- Herramientas
- Orquestador
- Motor de políticas
- Sistemas externos

Cada uno cumple una responsabilidad diferente dentro del proceso de ejecución.

---

# 🌐 Una vista de alto nivel

Podemos representar una arquitectura agéntica moderna de forma simplificada.

```text
                    User
                      │
                      ▼
                 Agent Runtime
                      │
      ┌───────────────┼────────────────┐
      │               │                │
      ▼               ▼                ▼
  Planner         Memory         Policy Engine
      │               │                │
      ▼               ▼                ▼
   LLM           Knowledge        Authorization
      │
      ▼
 Tool Broker
      │
 ┌────┼──────────────┬───────────────┐
 │    │              │               │
CRM GitHub      Kubernetes      Email
      │
      ▼
   Response
```

Este diagrama no representa un producto concreto.

Representa una arquitectura de referencia.

Su objetivo consiste en mostrar cómo distintos componentes colaboran para completar una tarea.

---

# 👤 El Usuario

Toda ejecución comienza con un objetivo.

Ese objetivo normalmente proviene de un usuario.

Por ejemplo.

- "Generá un informe de vulnerabilidades."

- "Analizá este incidente."

- "Desplegá una nueva aplicación."

El usuario no indica necesariamente cómo realizar la tarea.

Únicamente describe el resultado esperado.

A partir de ese momento, el agente comienza a planificar.

---

# 🤖 El Agente

El agente constituye el componente central de la arquitectura.

Su responsabilidad consiste en transformar un objetivo de negocio en una secuencia organizada de acciones.

El agente no ejecuta directamente todas las operaciones.

Coordina el resto de los componentes.

Durante este proceso puede:

- consultar memoria;
- recuperar conocimiento;
- invocar herramientas;
- solicitar nuevas decisiones al modelo;
- actualizar el contexto.

En otras palabras.

El agente actúa como el director de la ejecución.

---

# 🧠 El Modelo de IA

El modelo representa el motor de razonamiento.

Es responsable de tareas como.

- interpretar instrucciones;
- generar planes;
- analizar información;
- producir respuestas;
- decidir el siguiente paso.

Es importante destacar que el modelo no conoce directamente el entorno.

No ejecuta herramientas.

No accede a bases de datos.

No modifica infraestructura.

Todas esas acciones dependen de otros componentes de la arquitectura.

---

# 📝 El Planificador

Muchos sistemas incorporan un componente especializado en planificación.

Su función consiste en dividir un objetivo complejo en una secuencia ordenada de tareas.

Por ejemplo.

```text
Objetivo

↓

Consultar vulnerabilidades

↓

Correlacionar activos

↓

Priorizar riesgos

↓

Generar informe
```

La planificación permite que el agente resuelva problemas complejos sin depender de una única interacción con el modelo.

---

# 🧠 La Memoria

Los agentes necesitan recordar información.

Dependiendo de la arquitectura pueden existir distintos tipos de memoria.

Por ejemplo.

- contexto de la conversación;
- estado de ejecución;
- memoria de largo plazo;
- memoria semántica.

La memoria permite mantener continuidad entre diferentes decisiones.

---

# 📚 El Conocimiento

No toda la información proviene del modelo.

Con frecuencia el agente consulta fuentes externas.

Por ejemplo.

- documentación;
- wikis;
- bases de conocimiento;
- RAG;
- manuales internos;
- políticas corporativas.

Estas fuentes complementan el conocimiento del modelo y permiten generar respuestas más precisas.

---

# 🔧 Las Herramientas

Las herramientas representan la capacidad del agente para interactuar con el mundo exterior.

Por ejemplo.

- APIs;
- bases de datos;
- sistemas de tickets;
- GitHub;
- Kubernetes;
- correo electrónico;
- plataformas cloud.

Mientras el modelo razona, las herramientas ejecutan acciones reales.

---

# 🎼 El Orquestador

En arquitecturas complejas suele existir un componente encargado de coordinar la interacción entre todos los elementos.

Este componente puede administrar.

- flujo de ejecución;
- estado;
- planificación;
- memoria;
- herramientas;
- agentes especializados.

El orquestador permite construir sistemas donde múltiples agentes colaboran para alcanzar un mismo objetivo.

---

# 🏗️ La arquitectura como un sistema vivo

Una característica importante de los sistemas agénticos consiste en que la ejecución no sigue siempre el mismo camino.

Por ejemplo.

```text
Goal

↓

Think

↓

Tool

↓

Observe

↓

Think

↓

Memory

↓

Think

↓

Another Tool

↓

Finish
```

Cada ejecución puede recorrer un camino diferente.

Esto convierte a la arquitectura en un sistema dinámico.

No en un flujo fijo de procesamiento.

---

# 💡 Pensar como un Security Architect

Antes de analizar amenazas o implementar controles, un arquitecto intenta comprender cómo fluye la información dentro del sistema.

Preguntas habituales incluyen.

- ¿Dónde comienza la ejecución?
- ¿Quién toma las decisiones?
- ¿Qué componentes mantienen estado?
- ¿Qué elementos ejecutan acciones?
- ¿Qué partes razonan?
- ¿Qué partes únicamente almacenan información?
- ¿Qué componentes interactúan con sistemas externos?

Comprender estas relaciones resulta esencial para interpretar correctamente cualquier arquitectura basada en agentes.

---

# 📌 Key Ideas

Los sistemas agénticos ya no pueden entenderse como aplicaciones tradicionales.

En lugar de un flujo lineal de procesamiento, incorporan múltiples componentes especializados que colaboran continuamente para transformar objetivos en acciones.

Una arquitectura moderna suele incluir:

- un agente;
- un modelo de IA;
- un planificador;
- memoria;
- conocimiento;
- herramientas;
- un orquestador;
- sistemas externos.

Cada componente posee responsabilidades claramente diferenciadas y participa activamente en el ciclo de vida del agente.

Comprender esta arquitectura constituye el punto de partida para analizar cómo los agentes razonan, planifican, ejecutan acciones y, posteriormente, cómo pueden protegerse.

---

# 📚 References

- OpenAI Agents SDK Documentation
- Google Agent Development Kit (ADK)
- LangGraph Documentation
- CrewAI Documentation
- Microsoft Semantic Kernel
- Anthropic Building Effective Agents
- NIST AI Risk Management Framework (AI RMF)
- Google Secure AI Framework (SAIF)
