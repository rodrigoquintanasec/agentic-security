# 🛡️ Agentic Trust Boundaries

> Todo sistema seguro comienza identificando dónde termina un dominio de confianza y comienza otro. En los sistemas agénticos, estos límites son considerablemente más complejos que en las aplicaciones tradicionales debido a la interacción continua entre usuarios, modelos, herramientas, memorias y agentes.

---

# 🎯 Objetivo

Comprender qué son los **Trust Boundaries**, cómo identificarlos dentro de una arquitectura basada en agentes inteligentes y por qué representan uno de los conceptos más importantes para realizar Threat Modeling y diseñar arquitecturas seguras.

---

# 📖 Introducción

Ningún sistema opera completamente aislado.

Toda aplicación intercambia información con otros componentes.

Por ejemplo.

- Usuarios.
- APIs.
- Bases de datos.
- Servicios Cloud.
- Sistemas externos.

Cada vez que la información atraviesa uno de esos límites, la organización pierde parte del control directo sobre ella.

Ese cambio de contexto recibe el nombre de **Trust Boundary**.

En Application Security, estos límites suelen encontrarse relativamente bien definidos.

En los sistemas agénticos, en cambio, aparecen nuevos dominios de confianza debido a la presencia de modelos de IA, herramientas externas, memorias, sistemas multiagente y fuentes de conocimiento dinámicas.

Comprender estos límites resulta esencial antes de analizar amenazas o implementar controles de seguridad.

---

# 🏛️ ¿Qué es un Trust Boundary?

Un **Trust Boundary** representa el punto donde la información, la autoridad o una decisión atraviesan un cambio de dominio de confianza.

Ese cambio puede producirse porque interviene:

- otra identidad;
- otro sistema;
- otra organización;
- otro agente;
- otro entorno;
- otra política de seguridad.

Cada vez que esto ocurre, la arquitectura debería asumir que las condiciones de confianza han cambiado.

En consecuencia, cualquier dato, acción o autoridad que atraviese ese límite debería volver a validarse.

---

# 🧩 ¿Por qué son tan importantes?

Los ataques más importantes rara vez ocurren completamente dentro de un mismo componente.

Generalmente aparecen cuando una organización asume que un dato continúa siendo confiable después de atravesar un nuevo contexto.

Por ejemplo.

Un usuario envía un prompt.

↓

El prompt llega al agente.

↓

El agente consulta una herramienta.

↓

La herramienta llama a una API.

↓

La API accede a una base de datos.

Cada transición representa un cambio de dominio de confianza.

Si alguno de esos cambios no se valida correctamente, la arquitectura comienza a acumular riesgo.

---

# 🌐 Trust Boundaries en sistemas agénticos

Una arquitectura basada en IA suele contener muchos más límites de confianza que una aplicación tradicional.

Por ejemplo.

```text
User
    │
────┼────────────────────────
    ▼
Agent
    │
────┼────────────────────────
LLM
    │
────┼────────────────────────
Memory
    │
────┼────────────────────────
Tool Broker
    │
────┼────────────────────────
External Tools
    │
────┼────────────────────────
Cloud Services
```

Cada línea representa un lugar donde la confianza cambia.

Y donde la arquitectura debería aplicar controles específicos.

---

# 📦 Principales Trust Boundaries

## 👤 User → Agent

El agente recibe instrucciones provenientes del usuario.

En este límite deben validarse aspectos como:

- identidad;
- autorización;
- contexto;
- objetivos;
- contenido recibido.

---

## 🤖 Agent → LLM

El agente delega parte del razonamiento al modelo.

Aquí resulta importante controlar:

- prompts enviados;
- información sensible;
- contexto compartido;
- respuestas generadas.

---

## 🧠 Agent → Memory

El agente consulta o almacena información.

Este límite afecta directamente:

- confidencialidad;
- integridad;
- persistencia;
- aislamiento entre usuarios.

---

## 🔧 Agent → Tools

Uno de los límites más críticos.

El agente deja de razonar y comienza a ejecutar acciones.

Aquí deberían validarse:

- Scope;
- políticas;
- herramientas permitidas;
- identidad utilizada;
- autoridad delegada.

---

## 🌍 Agent → External Services

Cuando un agente interactúa con servicios externos, la organización pierde parte del control sobre el entorno.

Esto requiere controles adicionales relacionados con:

- autenticación;
- cifrado;
- monitoreo;
- auditoría;
- clasificación de datos.

---

## 🤝 Agent → Agent

En arquitecturas multiagente aparecen nuevos límites.

Cada agente puede poseer:

- capacidades distintas;
- identidades distintas;
- herramientas distintas;
- autoridad distinta.

La confianza nunca debería propagarse automáticamente entre ellos.

---

# ⚠️ Trust no significa confianza absoluta

Uno de los errores más frecuentes consiste en interpretar el término "Trust Boundary" como una zona donde todo resulta seguro.

Ocurre exactamente lo contrario.

Un Trust Boundary señala precisamente el lugar donde **la confianza debe volver a construirse**.

Cada vez que una solicitud atraviesa un límite, la arquitectura debería preguntarse nuevamente:

- ¿Quién originó esta acción?
- ¿Continúa siendo válida la identidad?
- ¿La autoridad sigue siendo correcta?
- ¿El Scope permanece vigente?
- ¿Las políticas continúan aplicándose?

La confianza nunca debería heredarse automáticamente.

---

# 🛡️ Zero Trust y Agentic Security

El concepto de **Zero Trust** adquiere una nueva dimensión dentro de los sistemas agénticos.

El principio continúa siendo el mismo.

> **Never Trust. Always Verify.**

Sin embargo.

Ahora ya no solamente verificamos usuarios.

También verificamos:

- agentes;
- herramientas;
- modelos;
- memorias;
- APIs;
- cadenas de delegación.

Cada componente representa un posible cambio de dominio de confianza.

---

# 🏗️ Architecture Perspective

Una arquitectura madura identifica explícitamente todos los Trust Boundaries.

```text
User
    │
Authentication
    │
──────────────
Agent
    │
Policy Engine
    │
──────────────
LLM
    │
──────────────
Memory
    │
──────────────
Tool Broker
    │
──────────────
External Systems
```

Obsérvese que cada transición incorpora nuevos mecanismos de validación.

La confianza nunca fluye libremente entre componentes.

Debe reconstruirse continuamente.

---

# 💡 Pensar como un Agentic Security Engineer

Antes de analizar amenazas, un arquitecto suele formular preguntas como:

- ¿Dónde cambia el dominio de confianza?
- ¿Qué datos cruzan ese límite?
- ¿Qué autoridad viaja junto con ellos?
- ¿Qué identidad utiliza cada componente?
- ¿Qué políticas deberían volver a evaluarse?
- ¿Qué ocurriría si este límite fuera comprometido?

Responder estas preguntas permite descubrir gran parte de la superficie de ataque de una arquitectura basada en agentes.

---

# 📌 Key Ideas

Los **Trust Boundaries** representan uno de los conceptos fundamentales de Agentic Security.

Cada vez que información, autoridad o decisiones atraviesan un cambio de dominio de confianza, la arquitectura debe asumir que las condiciones de seguridad también han cambiado.

En los sistemas agénticos, estos límites ya no existen únicamente entre usuarios y aplicaciones.

Ahora también aparecen entre:

- usuarios y agentes;
- agentes y modelos;
- agentes y memorias;
- agentes y herramientas;
- agentes y otros agentes;
- agentes y servicios externos.

Identificar correctamente estos límites constituye el primer paso para construir un modelo de amenazas realista y diseñar controles capaces de gobernar arquitecturas basadas en inteligencia artificial.

En el próximo capítulo profundizaremos precisamente en ese proceso analizando cómo identificar, clasificar y priorizar las amenazas propias de los sistemas agénticos mediante **Agentic Threats and Risk**.
