# 🧠 Agent Planning and Reasoning

> La principal diferencia entre un agente inteligente y una aplicación tradicional no radica únicamente en el uso de un modelo de IA. La diferencia fundamental consiste en que el agente es capaz de analizar un objetivo, construir un plan, tomar decisiones durante la ejecución y adaptar ese plan cuando el contexto cambia. Comprender estos mecanismos constituye uno de los pilares de la arquitectura moderna de sistemas agénticos.

---

# 🎯 Objetivo

Comprender cómo los agentes modernos planifican tareas, toman decisiones y adaptan su comportamiento durante la ejecución, identificando los principales patrones arquitectónicos utilizados por la industria para construir sistemas capaces de resolver problemas complejos.

---

# 📖 Introducción

Cuando observamos a un agente resolver un problema complejo, puede parecer que simplemente responde una pregunta.

En realidad ocurre algo mucho más interesante.

Antes de ejecutar una acción, el sistema normalmente necesita responder cuestiones como:

- ¿Cuál es el objetivo?
- ¿Qué información ya poseo?
- ¿Qué información necesito?
- ¿Qué herramientas debo utilizar?
- ¿Cuál es el mejor orden para ejecutar las tareas?
- ¿Debo modificar el plan si aparece nueva información?

Estas preguntas forman parte del proceso de planificación y razonamiento.

No describen cómo "piensa" un modelo de IA.

Describen cómo una arquitectura organiza el proceso de resolución de problemas.

---

# 🏛️ Planning vs Reasoning

Aunque ambos conceptos suelen utilizarse como sinónimos, representan responsabilidades diferentes.

## Planning

Responde la pregunta:

> **¿Qué acciones deberían ejecutarse para alcanzar el objetivo?**

El resultado es un plan.

---

## Reasoning

Responde preguntas como:

- ¿Cuál debería ser el siguiente paso?
- ¿Qué información necesito?
- ¿Qué herramienta resulta más adecuada?
- ¿Cómo interpreto el contexto actual?

El resultado es una decisión.

En términos simples.

El plan organiza el trabajo.

El razonamiento decide cómo avanzar durante la ejecución.

---

# 🌐 Una visión general

Podemos representar ambos procesos de la siguiente manera.

```text
Goal
 │
 ▼
Planning
 │
 ▼
Execution Plan
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
Observation
 │
 ▼
Reasoning
```

Obsérvese que el plan normalmente se genera una sola vez.

El razonamiento ocurre continuamente.

---

# 📋 Goal Decomposition

Muchos problemas no pueden resolverse mediante una única acción.

Por ese motivo los agentes suelen dividir un objetivo complejo en múltiples subtareas.

Por ejemplo.

```text
Analizar Incidente

↓

Recolectar Logs

↓

Correlacionar Eventos

↓

Identificar IOC

↓

Evaluar Riesgo

↓

Generar Reporte
```

Este proceso recibe el nombre de **Goal Decomposition**.

Dividir un problema permite reducir la complejidad y mejorar la calidad de las decisiones posteriores.

---

# 🧩 Static Planning

En algunos escenarios el agente puede construir un plan completo antes de comenzar la ejecución.

```text
Plan

↓

Step 1

↓

Step 2

↓

Step 3

↓

Finish
```

Este enfoque resulta adecuado cuando:

- el problema está bien definido;
- el entorno cambia poco;
- las tareas son predecibles.

Su principal ventaja consiste en la simplicidad.

Su principal limitación es la escasa capacidad de adaptación.

---

# 🔄 Dynamic Planning

Muchos entornos cambian constantemente.

Por ese motivo numerosas arquitecturas utilizan planificación dinámica.

En lugar de construir todo el plan desde el comienzo, el agente decide continuamente cuál será el siguiente paso.

```text
Observe

↓

Reason

↓

Plan Next Action

↓

Execute

↓

Observe Again
```

Este enfoque permite adaptar la ejecución frente a información nueva.

---

# 🔁 Replanning

En ocasiones el plan original deja de ser válido.

Por ejemplo.

- una herramienta falla;
- aparece nueva información;
- cambia el objetivo;
- una política bloquea una acción.

En estos casos el agente necesita generar un nuevo plan.

```text
Original Plan

↓

Unexpected Event

↓

Replanning

↓

New Plan
```

Esta capacidad constituye una de las características más importantes de los agentes modernos.

---

# 🎯 Decision Making

El razonamiento produce decisiones.

Por ejemplo.

- utilizar una herramienta;
- solicitar aclaraciones;
- consultar memoria;
- finalizar la ejecución;
- modificar el plan.

Cada decisión depende del estado actual del sistema.

No únicamente del objetivo inicial.

---

# 🧠 Planning Patterns

La industria ha desarrollado distintos patrones para organizar la planificación.

Entre los más conocidos encontramos.

## ReAct

Alterna continuamente entre razonamiento y acciones.

```text
Reason

↓

Act

↓

Observe

↓

Reason
```

---

## Plan and Execute

Primero construye un plan.

Luego ejecuta cada paso.

```text
Plan

↓

Execute

↓

Finish
```

---

## Reflection

Después de ejecutar una acción, el sistema evalúa el resultado antes de continuar.

```text
Act

↓

Observe

↓

Reflect

↓

Next Action
```

---

## Iterative Planning

El plan evoluciona durante toda la ejecución.

Cada nueva observación puede modificar las acciones futuras.

---

# ⚙️ ¿Quién realiza estas tareas?

Dependiendo de la arquitectura, la planificación y el razonamiento pueden estar distribuidos entre distintos componentes.

Por ejemplo.

- Planner
- Agent Runtime
- LLM
- Workflow Engine
- Orchestrator

No existe una única implementación.

Lo importante es comprender las responsabilidades.

---

# 🏗️ Architecture Perspective

Una arquitectura típica puede representarse de la siguiente manera.

```text
Goal
 │
 ▼
Planner
 │
 ▼
Execution Plan
 │
 ▼
Reasoning Engine
 │
 ▼
Decision
 │
 ▼
Tool Selection
 │
 ▼
Execution
 │
 ▼
Observation
 │
 ▼
Reflection
 │
 ├───────────────┐
 │               │
 ▼               │
Finish           │
                 │
                 ▼
           Replanning
```

Obsérvese que la arquitectura permite adaptar continuamente el comportamiento del agente sin necesidad de reconstruir completamente el sistema.

---

# 💡 Pensar como un Security Architect

Antes de analizar amenazas, un arquitecto intenta comprender cómo toma decisiones el agente.

Preguntas habituales incluyen.

- ¿Quién construye el plan?
- ¿Quién decide utilizar una herramienta?
- ¿Qué información participa del razonamiento?
- ¿Cuándo puede modificarse el plan?
- ¿Qué ocurre si una herramienta devuelve información inesperada?
- ¿Qué componentes participan del replanning?

Comprender estas relaciones permite identificar posteriormente los puntos donde deberán incorporarse mecanismos de validación, autorización y supervisión.

---

# 📌 Key Ideas

La planificación y el razonamiento representan dos capacidades complementarias.

La planificación organiza el trabajo.

El razonamiento permite tomar decisiones durante la ejecución.

Las arquitecturas modernas combinan ambos mecanismos para construir sistemas capaces de adaptarse continuamente a cambios en el entorno.

Patrones como **Goal Decomposition**, **Dynamic Planning**, **Replanning**, **ReAct**, **Plan and Execute** y **Reflection** permiten desarrollar agentes más flexibles, robustos y preparados para resolver tareas complejas.

Comprender estos patrones resulta esencial para analizar posteriormente cómo estas capacidades introducen nuevos desafíos relacionados con seguridad, gobernanza y control.

---

# 📚 References

- OpenAI Agents SDK Documentation
- Anthropic — Building Effective Agents
- Google Agent Development Kit (ADK)
- LangGraph Documentation
- CrewAI Documentation
- Microsoft Semantic Kernel
- ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2022)
- Plan-and-Solve Prompting (Wang et al., 2023)
- Tree of Thoughts: Deliberate Problem Solving with Large Language Models (Yao et al., 2023)
- NIST AI Risk Management Framework (AI RMF)
