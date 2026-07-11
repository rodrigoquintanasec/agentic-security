# 🛡️ Agentic Security Controls

> Los Security Objectives describen qué propiedades debe preservar una arquitectura. Los **Security Controls** representan los mecanismos técnicos, organizacionales y operativos que permiten alcanzar esos objetivos y gobernar el comportamiento de agentes inteligentes durante todo su ciclo de vida.

---

# 🎯 Objetivo

Comprender qué son los **Agentic Security Controls**, cómo se clasifican y cómo trabajan conjuntamente para proteger sistemas basados en inteligencia artificial, agentes autónomos y arquitecturas multiagente.

---

# 📖 Introducción

Hasta este punto hemos identificado:

- los activos;
- los límites de confianza;
- las amenazas;
- los riesgos;
- los objetivos de seguridad.

Sin embargo.

Conocer el riesgo no reduce el riesgo.

Para ello resulta necesario implementar controles capaces de limitar, detectar, prevenir y responder frente a comportamientos inseguros.

En Agentic Security, los controles ya no protegen únicamente aplicaciones.

Ahora también gobiernan:

- decisiones;
- autoridad;
- identidad;
- herramientas;
- memoria;
- autonomía;
- colaboración entre agentes.

---

# 🏛️ ¿Qué es un Security Control?

Un **Security Control** representa cualquier mecanismo diseñado para reducir el riesgo asociado a un sistema.

Puede tratarse de:

- una política;
- un proceso;
- una tecnología;
- una restricción;
- una validación;
- una práctica de ingeniería.

Los controles no eliminan completamente el riesgo.

Su propósito consiste en reducir la probabilidad o el impacto de un incidente hasta niveles aceptables para la organización.

---

# 🧩 Una arquitectura basada en capas

Los controles no funcionan de forma aislada.

Una arquitectura madura distribuye controles a lo largo de todo el ciclo de vida del agente.

```text
Business Goal

↓

Identity

↓

Delegation

↓

Planning

↓

Reasoning

↓

Tool Selection

↓

Execution

↓

Monitoring

↓

Audit

↓

Response
```

Cada etapa incorpora controles diferentes.

---

# 🔐 Identity Controls

El primer conjunto de controles protege la identidad.

Su objetivo consiste en garantizar que cada agente actúe utilizando una identidad claramente definida y verificable.

Ejemplos.

- Workload Identity
- Managed Identity
- Service Accounts
- OAuth
- Mutual TLS
- Certificate-based Authentication

Estos controles responden una pregunta fundamental.

> ¿Quién está actuando?

---

# 🔑 Authorization Controls

Una vez conocida la identidad, resulta necesario limitar la autoridad.

Entre los controles más importantes encontramos.

- Least Privilege
- Scoped Delegation
- Just-in-Time Privileges
- Policy-based Authorization
- Runtime Authorization
- Approval Gates

Su objetivo consiste en responder.

> ¿Qué está autorizado a hacer?

---

# 🧠 Reasoning Controls

Los agentes toman decisiones.

Por ese motivo también resulta necesario gobernar el propio proceso de razonamiento.

Ejemplos.

- Goal Validation
- Context Validation
- Prompt Guardrails
- Output Validation
- Response Filtering
- Policy-based Reasoning

Estos controles reducen el riesgo de decisiones incorrectas antes de llegar a la fase de ejecución.

---

# 🔧 Tool Controls

Las herramientas representan el punto donde el agente deja de razonar y comienza a actuar.

Por ello constituyen uno de los dominios más críticos.

Entre los controles encontramos.

- Tool Allowlists
- Tool Isolation
- Parameter Validation
- Tool Broker
- Rate Limiting
- Runtime Policies

Nunca todas las herramientas deberían estar disponibles para todas las tareas.

---

# 🧠 Memory Controls

Los agentes modernos almacenan y reutilizan contexto.

La memoria debe protegerse mediante controles como.

- Memory Isolation
- Data Classification
- Retention Policies
- Encryption
- Poisoning Detection
- Context Validation

La memoria representa un activo crítico.

No simplemente una funcionalidad adicional.

---

# 📚 Knowledge Controls

Las fuentes de conocimiento también deben gobernarse.

Por ejemplo.

- Document Validation
- Trusted Sources
- RAG Filtering
- Content Classification
- Integrity Verification

Un agente únicamente puede tomar buenas decisiones si la información utilizada resulta confiable.

---

# 👥 Human Oversight Controls

No todas las decisiones deberían automatizarse completamente.

Dependiendo del riesgo, la arquitectura puede incorporar.

- Human-in-the-Loop
- Human-on-the-Loop
- Approval Workflows
- Escalation Rules
- Kill Switch

Estos controles permiten mantener el gobierno humano sobre decisiones críticas.

---

# 👁️ Observability Controls

Una arquitectura segura debe permitir comprender qué hizo el agente y por qué.

Algunos controles incluyen.

- Centralized Logging
- Decision Logging
- Audit Trails
- Trace Correlation
- Metrics
- Distributed Tracing

La observabilidad constituye un requisito esencial para investigación forense y mejora continua.

---

# 🚨 Runtime Security Controls

Muchos riesgos aparecen únicamente durante la ejecución.

Por ello resulta fundamental incorporar controles dinámicos.

Por ejemplo.

- Runtime Policy Enforcement
- Runtime Authorization
- Continuous Authorization
- Behavioral Monitoring
- Anomaly Detection
- Runtime Guardrails

Estos mecanismos permiten detectar comportamientos inseguros antes de que produzcan consecuencias relevantes.

---

# 🔄 Response Controls

Toda arquitectura debería asumir que eventualmente ocurrirá un incidente.

Entre los principales controles de respuesta encontramos.

- Revocation
- Kill Switch
- Session Termination
- Rollback
- Incident Response
- Credential Rotation

Su objetivo consiste en recuperar rápidamente el control sobre la arquitectura.

---

# 🏗️ Defense in Depth

Uno de los principios más importantes consiste en evitar depender de un único control.

La seguridad emerge de múltiples capas trabajando conjuntamente.

```text
User
     │
Authentication
     │
Authorization
     │
Delegation
     │
Policy Engine
     │
Reasoning Controls
     │
Tool Broker
     │
Runtime Validation
     │
Monitoring
     │
Audit
     │
Incident Response
```

Si una capa falla.

Las demás continúan reduciendo el riesgo.

---

# 📊 Relación entre controles y objetivos

Los controles existen para proteger los Security Objectives.

```text
Security Objective

↓

Authority Integrity

↓

Controls

• Least Privilege

• Scoped Delegation

• Runtime Authorization

• Policy Engine
```

Otro ejemplo.

```text
Security Objective

↓

Goal Integrity

↓

Controls

• Goal Validation

• Prompt Guardrails

• Human Oversight

• Runtime Monitoring
```

Pensar primero en los objetivos evita implementar controles innecesarios.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto diseña un sistema basado en agentes rara vez comienza preguntando.

> ¿Qué herramienta de seguridad vamos a utilizar?

Primero intenta responder.

- ¿Qué decisiones toma el agente?
- ¿Qué autoridad necesita?
- ¿Qué herramientas utilizará?
- ¿Qué controles gobiernan cada etapa?
- ¿Qué ocurre cuando alguno de esos controles falla?
- ¿Qué mecanismos permanecen disponibles durante un incidente?

Los controles representan una consecuencia del diseño.

No el punto de partida.

---

# 📌 Key Ideas

Los **Agentic Security Controls** constituyen los mecanismos que permiten transformar una arquitectura basada en agentes en un sistema gobernado y resiliente.

Una estrategia madura distribuye controles sobre todo el ciclo de vida del agente.

- Identity Controls
- Authorization Controls
- Reasoning Controls
- Tool Controls
- Memory Controls
- Knowledge Controls
- Human Oversight Controls
- Runtime Controls
- Observability Controls
- Response Controls

Ninguno de estos mecanismos resulta suficiente por sí solo.

La verdadera seguridad surge cuando todos trabajan de forma coordinada bajo principios como **Least Privilege**, **Defense in Depth**, **Zero Trust** y **Continuous Authorization**.

En el próximo capítulo analizaremos cómo estos controles se integran dentro de una disciplina más amplia: los **Agentic Security Engineering Principles**, es decir, los principios de diseño que guían la construcción de arquitecturas seguras desde sus primeras etapas.

---

# 📚 References

- NIST SP 800-53 Rev. 5
- NIST SP 800-207 — Zero Trust Architecture
- NIST AI Risk Management Framework (AI RMF)
- NIST SP 800-160 — Systems Security Engineering
- OWASP Agentic Security Initiative
- OWASP ASVS
- Google Secure AI Framework (SAIF)
- Microsoft AI Security
- MITRE ATLAS
- CIS Controls v8
