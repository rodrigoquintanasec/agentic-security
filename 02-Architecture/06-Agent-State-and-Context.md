# 📊 Agent State and Context

> Un agente inteligente no toma decisiones únicamente a partir del objetivo recibido. Cada decisión depende del estado actual de la ejecución y del contexto disponible en ese momento. Comprender la diferencia entre **State** y **Context** constituye uno de los fundamentos para diseñar arquitecturas agénticas modernas.

---

# 🎯 Objetivo

Comprender qué representan el **State** y el **Context** dentro de una arquitectura basada en agentes inteligentes, cómo evolucionan durante la ejecución y por qué ambos conceptos resultan fundamentales para la planificación, el razonamiento y la toma de decisiones.

---

# 📖 Introducción

Cuando observamos un agente resolver una tarea compleja, normalmente vemos únicamente el resultado final.

Sin embargo.

Durante toda la ejecución el agente mantiene información que cambia constantemente.

Por ejemplo.

- qué objetivo intenta alcanzar;
- qué tareas ya completó;
- qué herramientas utilizó;
- qué información obtuvo;
- qué decisiones tomó;
- qué acciones permanecen pendientes.

Toda esa información determina cuál será la siguiente acción.

Sin ella, el agente debería comenzar desde cero en cada iteración.

Por este motivo aparecen dos conceptos fundamentales.

- **State**
- **Context**

Aunque suelen utilizarse como sinónimos, representan ideas diferentes.

---

# 🏛️ ¿Qué es el State?

El **State** representa el estado interno de una ejecución en un momento determinado.

Describe **qué está ocurriendo actualmente** dentro del agente.

Puede incluir información como.

- objetivo actual;
- tarea en ejecución;
- herramientas utilizadas;
- resultados obtenidos;
- variables temporales;
- progreso del workflow.

En otras palabras.

El State representa la fotografía actual del proceso de ejecución.

---

# 📦 Ejemplo de State

Supongamos el siguiente objetivo.

> Analizar vulnerabilidades críticas.

Durante la ejecución el State podría contener.

```text
Goal

Analyze Critical Vulnerabilities

Current Task

Correlate Assets

Completed

✓ Query Scanner

✓ Retrieve CMDB

Pending

Generate Report
```

Obsérvese que el State describe el progreso de la tarea.

No el conocimiento permanente del agente.

---

# 🌐 ¿Qué es el Context?

El **Context** representa toda la información disponible que puede influir sobre la siguiente decisión del agente.

Incluye mucho más que el State.

Por ejemplo.

- instrucciones del usuario;
- resultados de herramientas;
- políticas;
- contexto conversacional;
- documentos recuperados;
- variables de ejecución;
- restricciones del sistema.

El contexto responde una pregunta diferente.

> **¿Qué información conoce actualmente el agente para tomar la siguiente decisión?**

---

# 📊 State vs Context

Aunque ambos evolucionan durante la ejecución, cumplen funciones distintas.

| State | Context |
|--------|---------|
| Describe el progreso de la ejecución | Describe la información disponible para decidir |
| Cambia continuamente | También cambia continuamente |
| Representa el estado del workflow | Representa el conocimiento utilizado durante el razonamiento |
| Se utiliza para coordinar la ejecución | Se utiliza para interpretar el problema |

En términos simples.

El **State** responde:

> ¿Dónde estoy?

Mientras que el **Context** responde:

> ¿Qué sé en este momento?

---

# 🔄 Cómo evolucionan durante la ejecución

Tanto el State como el Context cambian continuamente.

```text
Goal

↓

State Updated

↓

Tool Execution

↓

New Context

↓

Reasoning

↓

State Updated Again

↓

Next Action
```

Cada nueva acción modifica ambos elementos.

Por este motivo resulta incorrecto considerarlos estructuras estáticas.

---

# 🧠 El Context no es solamente conversación

Uno de los errores más frecuentes consiste en pensar que el contexto corresponde únicamente al historial de mensajes.

En realidad.

El contexto puede incluir.

- conversación;
- resultados de herramientas;
- documentos recuperados mediante RAG;
- políticas;
- configuraciones;
- restricciones;
- estado del workflow;
- información del usuario;
- objetivos.

Todo aquello que influye sobre la siguiente decisión forma parte del contexto.

---

# 📍 El State tampoco es Memory

Otra confusión habitual consiste en utilizar State y Memory como si fueran equivalentes.

No lo son.

El State existe únicamente durante la ejecución actual.

Cuando la tarea finaliza, normalmente desaparece.

La Memory, en cambio, puede sobrevivir entre distintas ejecuciones y conservar información durante largos períodos.

Este aspecto será desarrollado en profundidad en el próximo capítulo.

---

# 🏗️ Una visión arquitectónica

Podemos representar ambos conceptos de la siguiente manera.

```text
User Goal
      │
      ▼
Execution State
      │
      ▼
Current Context
      │
      ▼
Reasoning
      │
      ▼
Decision
      │
      ▼
Action
      │
      ▼
Updated State
      │
      ▼
Updated Context
```

Obsérvese que ambos evolucionan continuamente a medida que avanza la ejecución.

---

# ⚙️ ¿Quién mantiene el State?

Dependiendo de la arquitectura.

El State puede mantenerse en distintos componentes.

Por ejemplo.

- Agent Runtime;
- Workflow Engine;
- Orchestrator;
- State Machine;
- LangGraph State;
- Session Manager.

El principio continúa siendo el mismo.

Siempre existe algún componente responsable de conservar el estado actual del sistema.

---

# 💡 Pensar como un Security Architect

Antes de analizar amenazas o implementar controles, un arquitecto intenta comprender cómo evoluciona la información durante la ejecución.

Preguntas habituales incluyen.

- ¿Qué información forma parte del contexto?
- ¿Qué componentes modifican el State?
- ¿Quién mantiene el estado actual?
- ¿Qué datos permanecen únicamente durante esta ejecución?
- ¿Qué ocurre si el contexto es manipulado?
- ¿Qué decisiones dependen directamente del State?

Comprender estas relaciones resulta esencial para diseñar posteriormente mecanismos de aislamiento, validación y protección del contexto.

---

# 📌 Key Ideas

El **State** y el **Context** representan dos conceptos diferentes pero complementarios.

El **State** describe el estado actual de la ejecución.

El **Context** representa toda la información disponible utilizada para tomar decisiones.

Ambos evolucionan continuamente durante el ciclo de vida del agente y condicionan cada nueva acción que este realiza.

Comprender esta diferencia resulta esencial para interpretar correctamente el funcionamiento interno de una arquitectura agéntica.

En el próximo capítulo profundizaremos en un concepto estrechamente relacionado pero diferente: **Agent Memory Architecture**, donde analizaremos cómo los agentes almacenan y reutilizan conocimiento más allá de una única ejecución.

---

# 📚 References

- OpenAI Agents SDK Documentation
- LangGraph Documentation
- Microsoft Semantic Kernel
- Google Agent Development Kit (ADK)
- CrewAI Documentation
- Anthropic — Building Effective Agents
- NIST AI Risk Management Framework (AI RMF)
