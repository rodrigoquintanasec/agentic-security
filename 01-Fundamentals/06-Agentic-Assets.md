# 🏛️ Agentic Assets

> Antes de proteger un sistema agéntico es necesario comprender exactamente qué constituye un activo y por qué los activos de una arquitectura basada en agentes son diferentes a los de una aplicación tradicional.

---

# 🎯 Objetivo

Comprender qué son los **Agentic Assets**, cómo identificarlos dentro de una arquitectura basada en IA y por qué representan el punto de partida para cualquier actividad de análisis de riesgos, Threat Modeling o diseño de controles de seguridad.

---

# 📖 Introducción

En seguridad de aplicaciones solemos comenzar identificando los activos que poseen valor para la organización.

Por ejemplo:

- Bases de datos.
- Credenciales.
- APIs.
- Código fuente.
- Información personal.
- Propiedad intelectual.

En un sistema agéntico estos activos continúan existiendo.

Sin embargo, aparecen otros completamente nuevos.

Un agente no solamente procesa información.

También interpreta objetivos, utiliza herramientas, mantiene contexto, consulta memoria, interactúa con modelos de IA y toma decisiones.

Todo ello introduce nuevos activos que también requieren protección.

---

# 🏛️ ¿Qué es un Agentic Asset?

Un **Agentic Asset** es cualquier componente cuya confidencialidad, integridad o disponibilidad resulta necesaria para que un sistema agéntico opere de forma segura y confiable.

No todos los activos almacenan información.

Algunos representan capacidades.

Otros representan autoridad.

Otros contienen conocimiento.

Otros determinan el comportamiento del agente.

Por ese motivo, limitar el inventario únicamente a bases de datos o documentos resulta insuficiente.

---

# 🧩 Categorías de Agentic Assets

Podemos agrupar los activos de una arquitectura agéntica en distintas categorías.

## 🤖 AI Models

Los modelos representan el motor de razonamiento del sistema.

Por ejemplo.

- Foundation Models
- Fine-tuned Models
- Embedding Models
- Local Models

---

## 💬 Prompts

Los prompts forman parte de la lógica de negocio.

Por ejemplo.

- System Prompts
- Developer Prompts
- Templates
- Hidden Instructions

En muchos casos poseen tanto valor como el propio código fuente.

---

## 🧠 Memory

Los mecanismos de memoria almacenan información utilizada por el agente para mantener contexto.

Por ejemplo.

- Short-term Memory
- Long-term Memory
- Vector Databases
- Conversation History

---

## 🔧 Tools

Las herramientas representan la capacidad operativa del agente.

Por ejemplo.

- APIs
- MCP Servers
- Databases
- File Systems
- Email
- Cloud Services
- Kubernetes
- GitHub

Una herramienta comprometida puede afectar directamente el comportamiento del agente.

---

## 👤 Identity

Los agentes operan utilizando identidades.

Estas pueden incluir.

- Service Accounts
- API Keys
- OAuth Tokens
- Managed Identities
- Workload Identities

La identidad constituye uno de los activos más críticos de toda la arquitectura.

---

## 🔐 Delegated Authority

No solamente importa quién es el agente.

También importa qué autoridad recibió.

Entre los activos encontramos.

- Delegation Tokens
- Scopes
- Temporary Credentials
- Approval Context
- Runtime Permissions

---

## 📄 Knowledge

Los agentes toman decisiones utilizando conocimiento.

Por ejemplo.

- RAG Sources
- Internal Documentation
- Policies
- Wikis
- PDFs
- Threat Intelligence

La calidad y la integridad de estas fuentes impactan directamente sobre las decisiones del agente.

---

# ⚠️ Todos los activos poseen distinto valor

No todos los activos requieren el mismo nivel de protección.

Por ejemplo.

```text
Public Documentation

↓

Internal Wiki

↓

Customer Data

↓

Identity Provider

↓

Delegated Authority

↓

Production Secrets
```

La criticidad depende del impacto que tendría su compromiso sobre el negocio.

---

# 💡 Pensar como un Agentic Security Engineer

Antes de implementar controles, un ingeniero de Agentic Security normalmente intenta responder preguntas como:

- ¿Qué activos existen realmente?
- ¿Qué activos poseen mayor valor?
- ¿Cuáles contienen autoridad?
- ¿Cuáles almacenan conocimiento?
- ¿Cuáles permiten ejecutar acciones?
- ¿Qué ocurriría si uno de ellos fuera comprometido?

Estas preguntas permiten construir un inventario mucho más completo que el utilizado tradicionalmente en Application Security.

---

# 📌 Key Ideas

Los sistemas agénticos introducen nuevos activos que no existían en aplicaciones tradicionales.

Además del código, las bases de datos y las APIs, ahora también deben protegerse:

- modelos de IA;
- prompts;
- memorias;
- herramientas;
- identidades;
- autoridad delegada;
- fuentes de conocimiento.

Identificar correctamente estos activos constituye el primer paso para realizar Threat Modeling, evaluar riesgos y diseñar arquitecturas seguras.

En el próximo capítulo analizaremos cómo estos activos se relacionan entre sí mediante **Agentic Trust Boundaries**, definiendo los límites de confianza que gobiernan toda la arquitectura.
