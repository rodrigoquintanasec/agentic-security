# 🎯 Agentic Security Objectives

> Antes de seleccionar controles, herramientas o tecnologías, una organización debe definir qué propiedades de seguridad necesita preservar. Los **Agentic Security Objectives** representan precisamente esos objetivos fundamentales sobre los cuales se diseñan las arquitecturas seguras de sistemas basados en agentes inteligentes.

---

# 🎯 Objetivo

Comprender cuáles son los principales **Security Objectives** de un sistema agéntico y cómo estos objetivos sirven como punto de partida para el diseño de controles, políticas y arquitecturas seguras.

---

# 📖 Introducción

Toda estrategia de seguridad comienza con una pregunta sencilla.

> **¿Qué estamos intentando proteger?**

Sin embargo.

Existe una segunda pregunta igual de importante.

> **¿Qué propiedades queremos preservar sobre aquello que protegemos?**

En Application Security estas propiedades suelen expresarse mediante conceptos como:

- Confidentiality
- Integrity
- Availability

En sistemas agénticos estos principios continúan siendo fundamentales.

Pero ya no resultan suficientes.

Los agentes toman decisiones, interpretan objetivos, utilizan herramientas, delegan autoridad y colaboran con otros agentes.

Como consecuencia, aparecen nuevas propiedades que también deben preservarse.

Los **Agentic Security Objectives** representan precisamente ese conjunto de objetivos.

---

# 🏛️ ¿Qué es un Security Objective?

Un **Security Objective** representa una propiedad que la arquitectura debe preservar para que el sistema continúe siendo seguro y confiable.

Es importante distinguir estos objetivos de los controles.

Por ejemplo.

**Least Privilege** no es un objetivo.

Es un mecanismo.

**Multi-Factor Authentication** tampoco es un objetivo.

Es un control.

El objetivo consiste en preservar determinadas propiedades.

Los controles son simplemente una forma de alcanzarlas.

---

# 🧩 Principales Agentic Security Objectives

## 🔒 Confidentiality

El agente únicamente debería acceder a la información estrictamente necesaria para completar su objetivo.

Esto incluye.

- prompts;
- memoria;
- documentos;
- secretos;
- credenciales;
- datos personales;
- conversaciones.

La exposición innecesaria de información incrementa significativamente el riesgo.

---

## ✔️ Integrity

Toda decisión debería basarse en información íntegra.

La arquitectura debe proteger.

- prompts;
- memoria;
- bases de conocimiento;
- herramientas;
- políticas;
- respuestas del modelo.

Una decisión basada en información manipulada puede producir consecuencias incorrectas incluso cuando el modelo funciona perfectamente.

---

## ⚙️ Availability

El sistema debe permanecer disponible para ejecutar las tareas autorizadas.

Esto implica proteger.

- modelos;
- APIs;
- herramientas;
- memorias;
- infraestructura.

Un agente incapaz de acceder a sus componentes críticos deja de cumplir su propósito.

---

## 🎯 Goal Integrity

Uno de los objetivos más importantes de Agentic Security.

El agente debe conservar el objetivo originalmente autorizado durante toda la ejecución.

No debería modificarlo.

Ni reinterpretarlo hasta convertirlo en una tarea completamente diferente.

Este principio reduce significativamente riesgos como:

- Goal Drift;
- Prompt Injection;
- Context Manipulation.

---

## 👤 Identity Integrity

La identidad utilizada durante la ejecución debe permanecer consistente.

La arquitectura debería garantizar que:

- el usuario original permanezca identificado;
- el agente utilice la identidad correcta;
- la autoridad nunca se desvincule de la identidad que la originó.

---

## 🔑 Authority Integrity

No basta con proteger la identidad.

También resulta necesario proteger la autoridad.

Durante toda la ejecución debería preservarse:

- Scope;
- Delegation;
- Least Privilege;
- Approval Context.

La autoridad nunca debería ampliarse sin una nueva decisión explícita.

---

## 🛡️ Policy Enforcement

Todas las decisiones importantes deberían permanecer gobernadas por políticas.

No únicamente al inicio de la ejecución.

Sino durante todo el ciclo de vida del agente.

Las políticas representan uno de los principales mecanismos para mantener la arquitectura bajo control.

---

## 👁️ Observability

Una arquitectura segura debe permitir comprender.

- qué hizo el agente;
- por qué;
- utilizando qué herramientas;
- bajo qué identidad;
- con qué autoridad.

Sin observabilidad resulta imposible investigar incidentes o demostrar cumplimiento.

---

## 📜 Accountability

Cada acción debería poder atribuirse a una identidad claramente identificable.

La organización debería poder responder preguntas como.

- ¿Quién inició esta acción?
- ¿Qué agente la ejecutó?
- ¿Qué políticas la autorizaron?
- ¿Qué herramientas participaron?

La trazabilidad constituye una propiedad esencial de cualquier sistema agéntico.

---

## 🔄 Recoverability

Toda arquitectura debería asumir que eventualmente ocurrirá un error.

Por ese motivo debería resultar posible.

- detener agentes;
- revocar autoridad;
- revertir cambios;
- restaurar estados consistentes.

La capacidad de recuperación representa un objetivo de seguridad tan importante como la prevención.

---

# 📊 Relación entre los objetivos

Los Agentic Security Objectives no funcionan de manera aislada.

Todos se relacionan entre sí.

```text
Security Objectives

│

├── Confidentiality
├── Integrity
├── Availability
├── Goal Integrity
├── Identity Integrity
├── Authority Integrity
├── Policy Enforcement
├── Observability
├── Accountability
└── Recoverability
```

Una arquitectura madura intenta preservar todas estas propiedades de manera simultánea.

---

# 🏗️ Security Objectives vs Security Controls

Es importante comprender la diferencia.

```text
Security Objective

↓

Authority Integrity

↓

Security Controls

Least Privilege

Scoped Delegation

Policy Engine

Runtime Authorization

Audit Logs
```

Los objetivos describen **qué queremos preservar**.

Los controles describen **cómo lo logramos**.

Confundir ambos conceptos suele conducir a arquitecturas donde abundan las tecnologías pero no existe una estrategia clara de seguridad.

---

# 💡 Pensar como un Agentic Security Engineer

Antes de seleccionar controles, un arquitecto normalmente intenta responder preguntas como:

- ¿Qué propiedades debe preservar este sistema?
- ¿Qué ocurriría si el agente pierde el objetivo original?
- ¿Qué pasa si la autoridad cambia durante la ejecución?
- ¿Qué decisiones deben permanecer auditables?
- ¿Qué componentes deben poder recuperarse?
- ¿Qué propiedades resultan críticas para el negocio?

Estas preguntas permiten construir una estrategia de seguridad orientada a objetivos y no únicamente a tecnologías.

---

# 📌 Key Ideas

Los **Agentic Security Objectives** representan las propiedades fundamentales que una arquitectura basada en agentes debe preservar.

No describen herramientas.

No describen productos.

No describen controles.

Describen el estado que la organización espera mantener durante toda la operación del sistema.

Una vez definidos estos objetivos resulta posible seleccionar controles apropiados para alcanzarlos.

Precisamente ese será el foco del próximo capítulo.

**Agentic Security Controls**.

---

# 📚 References

- NIST SP 800-53 Rev. 5
- NIST SP 800-160
- NIST AI Risk Management Framework (AI RMF)
- NIST SP 800-207 — Zero Trust Architecture
- OWASP Agentic Security Initiative
- OWASP ASVS
- Google Secure AI Framework (SAIF)
- Microsoft AI Security
- MITRE ATLAS
