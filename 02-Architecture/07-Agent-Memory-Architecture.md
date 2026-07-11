# 🧠 Agent Memory Architecture

> La memoria constituye uno de los componentes que más diferencia a un agente inteligente de una aplicación tradicional. Mientras que una aplicación normalmente procesa una solicitud y descarta su estado, un agente puede conservar información, reutilizar experiencias anteriores y mantener continuidad entre distintas interacciones. Comprender cómo se organiza la memoria resulta fundamental para diseñar arquitecturas agénticas modernas.

---

# 🎯 Objetivo

Comprender qué es la memoria dentro de una arquitectura basada en agentes, cuáles son sus principales tipos y cómo cada uno contribuye al proceso de razonamiento, planificación y toma de decisiones.

---

# 📖 Introducción

En el capítulo anterior analizamos dos conceptos fundamentales:

- State
- Context

Ambos describen la información utilizada durante una ejecución.

Sin embargo, ninguno de ellos explica cómo un agente recuerda información entre distintas ejecuciones.

Aquí aparece un nuevo concepto.

La **Memory**.

La memoria permite que un agente conserve información durante distintos períodos de tiempo y la reutilice cuando resulte necesaria.

Sin memoria, cada nueva interacción comenzaría completamente desde cero.

---

# 🏛️ ¿Qué es la Memory?

La **Memory** representa el conjunto de mecanismos utilizados para almacenar, recuperar y reutilizar información más allá de una única decisión.

Su objetivo consiste en proporcionar continuidad.

Gracias a la memoria un agente puede:

- recordar conversaciones;
- reutilizar información previa;
- mantener preferencias del usuario;
- almacenar experiencias;
- recuperar conocimiento relevante;
- continuar tareas largas.

En otras palabras.

La memoria permite que el agente evolucione durante el tiempo.

---

# 📊 State, Context y Memory

Aunque estos conceptos se relacionan, representan responsabilidades diferentes.

| Concepto | Propósito |
|-----------|-----------|
| State | Describe el estado actual de la ejecución |
| Context | Representa toda la información disponible para decidir |
| Memory | Conserva información entre distintas decisiones o ejecuciones |

Podemos resumirlo así.

```text
Memory

↓

Context

↓

Reasoning

↓

Decision

↓

State Updated
```

La memoria alimenta el contexto.

El contexto alimenta el razonamiento.

El razonamiento modifica el estado.

---

# 🧩 Tipos de memoria

Las arquitecturas modernas suelen utilizar varios tipos de memoria.

Cada una resuelve un problema diferente.

---

# ⚡ Working Memory

También conocida como memoria de trabajo.

Contiene únicamente la información necesaria para la iteración actual.

Por ejemplo.

- variables temporales;
- resultados recientes;
- cálculos intermedios;
- herramientas ejecutadas.

Su duración suele ser muy corta.

Normalmente desaparece al finalizar la tarea.

---

# 💬 Conversation Memory

Almacena el historial de la conversación.

Permite que el agente recuerde.

- preguntas anteriores;
- respuestas generadas;
- aclaraciones del usuario;
- referencias realizadas durante el diálogo.

Gracias a esta memoria el usuario puede continuar una conversación sin repetir toda la información.

---

# 📚 Episodic Memory

Conserva experiencias completas.

Por ejemplo.

- investigaciones realizadas;
- incidentes resueltos;
- tareas ejecutadas;
- workflows anteriores.

En lugar de almacenar únicamente datos, almacena eventos.

Resulta especialmente útil para agentes que trabajan durante largos períodos.

---

# 🧠 Semantic Memory

Representa conocimiento estructurado.

Incluye.

- conceptos;
- definiciones;
- relaciones;
- hechos.

Muchas arquitecturas implementan esta memoria mediante bases vectoriales o mecanismos similares.

---

# 📖 Procedural Memory

Describe cómo realizar determinadas tareas.

Por ejemplo.

- procedimientos;
- workflows;
- playbooks;
- secuencias de acciones.

Este tipo de memoria resulta especialmente útil para automatizaciones repetitivas.

---

# 🌐 Long-Term Memory

Agrupa toda aquella información que debe persistir durante largos períodos.

Puede incluir.

- preferencias;
- historial;
- conocimientos;
- configuraciones;
- experiencias.

No depende de una única conversación.

Puede mantenerse durante semanas, meses o incluso años.

---

# 🔄 Cómo interactúan

Todas estas memorias colaboran durante la ejecución.

```text
Long-Term Memory
        │
        ▼
Semantic Memory
        │
        ▼
Conversation Memory
        │
        ▼
Working Memory
        │
        ▼
Reasoning
        │
        ▼
Decision
```

Cada una aporta información distinta.

No representan niveles de importancia.

Representan responsabilidades diferentes.

---

# ⚙️ Recuperación de información

La memoria no consiste únicamente en almacenar información.

También debe recuperarla cuando resulte necesaria.

Normalmente el flujo sigue un proceso similar.

```text
Goal

↓

Current Context

↓

Memory Retrieval

↓

Relevant Information

↓

Reasoning

↓

Decision
```

La calidad de esta recuperación influye directamente sobre las decisiones del agente.

---

# 🏗️ Architecture Perspective

Podemos representar una arquitectura simplificada de memoria.

```text
                     Agent
                       │
       ┌───────────────┼────────────────┐
       ▼               ▼                ▼
 Working        Conversation      Long-Term
 Memory            Memory          Memory
       │               │                │
       └───────────────┼────────────────┘
                       ▼
               Memory Manager
                       │
                       ▼
                  Reasoning
```

Obsérvese que el agente no interactúa necesariamente con cada memoria de forma directa.

En muchas arquitecturas existe un componente especializado encargado de administrarlas.

---

# ⚠️ La memoria no es una base de datos

Un error frecuente consiste en pensar que la memoria equivale simplemente a almacenar información.

La memoria implica mucho más.

Debe responder preguntas como.

- ¿Qué información conservar?
- ¿Qué información olvidar?
- ¿Cuándo actualizarla?
- ¿Cómo recuperarla?
- ¿Cómo evitar inconsistencias?
- ¿Cómo mantenerla sincronizada?

La arquitectura de memoria resulta tan importante como la propia capacidad de razonamiento del agente.

---

# 💡 Pensar como un Security Architect

Antes de analizar amenazas o implementar controles, un arquitecto intenta comprender cómo el agente recuerda información.

Preguntas habituales incluyen.

- ¿Qué memorias existen?
- ¿Qué información almacena cada una?
- ¿Cuánto tiempo permanece disponible?
- ¿Quién puede modificarla?
- ¿Cómo se recupera?
- ¿Qué ocurre si contiene información incorrecta?

Responder estas preguntas constituye el primer paso para diseñar mecanismos de protección de memoria que analizaremos en capítulos posteriores.

---

# 📌 Key Ideas

La memoria constituye uno de los componentes fundamentales de cualquier sistema agéntico moderno.

Permite mantener continuidad, reutilizar conocimiento y conservar información entre distintas ejecuciones.

Las arquitecturas actuales suelen combinar distintos tipos de memoria.

- Working Memory.
- Conversation Memory.
- Episodic Memory.
- Semantic Memory.
- Procedural Memory.
- Long-Term Memory.

Comprender estas diferencias resulta esencial para interpretar correctamente cómo los agentes almacenan y recuperan conocimiento antes de estudiar aspectos relacionados con seguridad, aislamiento y protección de memoria.

---

# 📚 References

- OpenAI Agents SDK Documentation
- Anthropic — Building Effective Agents
- Google Agent Development Kit (ADK)
- LangGraph Documentation
- CrewAI Documentation
- Microsoft Semantic Kernel
- NIST AI Risk Management Framework (AI RMF)
