# 🔧 Agent Tool Execution Architecture

> Los agentes inteligentes no interactúan directamente con el mundo real. Para consultar información, modificar sistemas o ejecutar acciones utilizan herramientas. Comprender cómo funciona la arquitectura de ejecución de herramientas constituye uno de los fundamentos para diseñar sistemas agénticos escalables, gobernables y seguros.

---

# 🎯 Objetivo

Comprender cómo los agentes seleccionan, invocan y utilizan herramientas durante la ejecución de una tarea, identificando los principales componentes involucrados y el flujo arquitectónico que sigue una llamada desde la decisión inicial hasta la recepción del resultado.

---

# 📖 Introducción

Uno de los aspectos que más diferencia a un agente inteligente de un chatbot tradicional es su capacidad para actuar sobre el entorno.

Mientras un chatbot únicamente genera texto, un agente puede.

- consultar APIs;
- acceder a bases de datos;
- ejecutar código;
- leer documentos;
- enviar correos electrónicos;
- desplegar infraestructura;
- interactuar con otros sistemas.

Estas acciones no son realizadas directamente por el modelo de IA.

Son ejecutadas mediante herramientas.

Comprender esta diferencia resulta fundamental para interpretar correctamente la arquitectura de un sistema agéntico.

---

# 🏛️ ¿Qué es una Tool?

Una **Tool** representa cualquier capacidad externa que un agente puede invocar para interactuar con el mundo fuera del modelo.

Una herramienta puede ser.

- una API;
- una función;
- un servicio;
- una base de datos;
- un sistema empresarial;
- un servidor MCP;
- un motor de búsqueda;
- un entorno de ejecución.

El modelo no ejecuta directamente estas herramientas.

Únicamente decide cuándo resulta apropiado utilizarlas.

---

# 🌐 Una visión general

Podemos representar el flujo general de ejecución de herramientas de la siguiente manera.

```text
Goal
 │
 ▼
Reasoning
 │
 ▼
Tool Selection
 │
 ▼
Tool Invocation
 │
 ▼
External System
 │
 ▼
Tool Response
 │
 ▼
Reasoning
```

Obsérvese que la herramienta forma parte del ciclo de ejecución del agente.

No constituye un componente independiente del proceso de razonamiento.

---

# 🧩 Las etapas de ejecución

La mayoría de las arquitecturas modernas siguen un proceso similar.

---

## 🎯 1. Tool Selection

Durante el razonamiento, el agente determina si necesita utilizar una herramienta.

Por ejemplo.

- consultar una API;
- recuperar información;
- ejecutar una búsqueda;
- realizar un cálculo;
- enviar una solicitud.

No todas las tareas requieren herramientas.

Muchas pueden resolverse únicamente mediante razonamiento.

---

## 📝 2. Parameter Construction

Una vez seleccionada la herramienta, el agente debe construir los parámetros necesarios para invocarla.

Por ejemplo.

```text
Tool

SearchVulnerabilities()

Parameters

Host = SERVER01

Severity = Critical
```

En esta etapa todavía no ocurre ninguna interacción con el sistema externo.

---

## 🔧 3. Tool Invocation

El agente solicita la ejecución de la herramienta.

Normalmente no la ejecuta directamente.

La petición suele enviarse a un componente intermedio encargado de administrar las herramientas disponibles.

Este componente puede validar.

- formato;
- parámetros;
- disponibilidad;
- compatibilidad.

---

## 🌍 4. External Execution

La herramienta interactúa con el sistema correspondiente.

Por ejemplo.

- una API REST;
- una base de datos;
- Kubernetes;
- GitHub;
- Microsoft Graph;
- AWS;
- Azure;
- Google Cloud.

Aquí ocurre la acción real sobre el entorno.

---

## 📥 5. Response Collection

Una vez finalizada la ejecución, la herramienta devuelve un resultado.

Por ejemplo.

- datos;
- documentos;
- errores;
- identificadores;
- estados;
- métricas.

Este resultado todavía no constituye la respuesta final del agente.

Simplemente representa nueva información disponible.

---

## 🧠 6. Result Interpretation

El agente incorpora el resultado obtenido al contexto actual.

Posteriormente puede.

- continuar razonando;
- ejecutar otra herramienta;
- modificar el plan;
- finalizar la tarea.

La ejecución de herramientas constituye únicamente una etapa dentro del ciclo completo del agente.

---

# 🎼 El Tool Broker

Muchas arquitecturas modernas incorporan un componente especializado para administrar herramientas.

Generalmente denominado **Tool Broker**, **Tool Manager** o **Tool Registry**.

Su función consiste en.

- registrar herramientas;
- descubrir capacidades;
- recibir solicitudes;
- ejecutar herramientas;
- devolver resultados.

Podemos representarlo de la siguiente manera.

```text
Agent
 │
 ▼
Tool Broker
 │
 ├─────────────┬─────────────┬─────────────┐
 ▼             ▼             ▼
GitHub      Database      Kubernetes
```

El agente ya no necesita conocer cómo funciona cada herramienta internamente.

---

# 📦 Herramientas locales y remotas

No todas las herramientas se ejecutan en el mismo lugar.

Algunas forman parte del propio proceso del agente.

Otras se encuentran completamente separadas.

Por ejemplo.

### Herramientas locales

- funciones;
- librerías;
- cálculos.

### Herramientas remotas

- APIs;
- servicios cloud;
- servidores MCP;
- sistemas empresariales;
- plataformas SaaS.

Desde el punto de vista arquitectónico, ambas representan capacidades externas al modelo.

---

# 🔄 Tool Chaining

En muchas ocasiones una única herramienta no resulta suficiente.

El agente puede construir cadenas de ejecución.

```text
Search Assets

↓

Query Vulnerabilities

↓

Retrieve Owner

↓

Generate Report
```

Cada herramienta alimenta a la siguiente.

Este patrón aparece frecuentemente en automatizaciones complejas.

---

# ⚙️ Tool Discovery

Las arquitecturas modernas no siempre conocen todas las herramientas durante el desarrollo.

Algunas permiten descubrir capacidades dinámicamente.

Por ejemplo.

```text
Available Tools

↓

Tool Discovery

↓

Capability Selection

↓

Execution
```

Este enfoque facilita la incorporación de nuevas capacidades sin modificar el agente principal.

---

# 🏗️ Architecture Perspective

Podemos representar una arquitectura simplificada de ejecución de herramientas.

```text
                    Agent
                      │
                 Reasoning
                      │
                      ▼
               Tool Selection
                      │
                      ▼
                Tool Broker
                      │
      ┌───────────────┼────────────────┐
      ▼               ▼                ▼
   GitHub        Kubernetes      CRM System
      │               │                │
      └───────────────┼────────────────┘
                      ▼
                Tool Response
                      │
                      ▼
                  Reasoning
```

Obsérvese que el agente nunca interactúa directamente con cada sistema.

La arquitectura incorpora una capa responsable de administrar la ejecución de herramientas.

---

# 💡 Pensar como un Security Architect

Antes de analizar amenazas o implementar controles, un arquitecto intenta comprender cómo un agente interactúa con sistemas externos.

Preguntas habituales incluyen.

- ¿Qué herramientas existen?
- ¿Quién decide cuál utilizar?
- ¿Cómo se construyen los parámetros?
- ¿Qué componente ejecuta realmente la herramienta?
- ¿Cómo se incorporan los resultados al contexto?
- ¿Qué ocurre cuando una herramienta falla?

Comprender estas relaciones constituye la base para estudiar posteriormente los mecanismos de autorización, validación y protección de herramientas.

---

# 📌 Key Ideas

Las herramientas representan el mecanismo mediante el cual un agente interactúa con el mundo exterior.

El modelo de IA no ejecuta directamente acciones sobre sistemas externos.

Su responsabilidad consiste en decidir cuándo resulta necesario utilizar una herramienta y cómo interpretar posteriormente el resultado obtenido.

La arquitectura de ejecución de herramientas suele incluir componentes especializados como.

- Tool Selection.
- Tool Broker.
- Tool Discovery.
- Tool Invocation.
- External Execution.
- Result Interpretation.

Comprender este flujo resulta esencial para interpretar posteriormente los desafíos relacionados con Tool Security, MCP Security y Runtime Authorization.

---

# 📚 References

- OpenAI Agents SDK Documentation
- Anthropic — Building Effective Agents
- LangGraph Documentation
- Google Agent Development Kit (ADK)
- CrewAI Documentation
- Microsoft Semantic Kernel
- Model Context Protocol (MCP) Specification
- NIST AI Risk Management Framework (AI RMF)
