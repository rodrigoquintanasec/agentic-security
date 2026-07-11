# 🧠 Agentic Security Fundamentals

> Base conceptual para analizar, diseñar y proteger agentes de IA, aplicaciones basadas en LLM y sistemas agénticos capaces de razonar, utilizar herramientas, mantener memoria, delegar tareas y ejecutar acciones sobre sistemas externos.

---

## 📌 Propósito de esta sección

Los sistemas agénticos introducen un cambio importante en la seguridad de aplicaciones.

Una aplicación tradicional normalmente recibe una entrada, ejecuta una lógica previamente definida y produce una salida. Un sistema agéntico, en cambio, puede interpretar objetivos, construir planes, consultar memoria, recuperar información, seleccionar herramientas, interactuar con servicios externos, delegar tareas y ejecutar acciones con distintos grados de autonomía.

Esta capacidad amplía significativamente el impacto potencial de una decisión incorrecta, una instrucción manipulada, una identidad comprometida o un control de autorización insuficiente.

Por ese motivo, proteger un sistema agéntico no consiste únicamente en filtrar prompts o bloquear respuestas inseguras.

Requiere comprender:

- Qué activos intervienen en el sistema.
- Qué identidades y autoridades participan.
- Qué Trust Boundaries atraviesan los datos y las instrucciones.
- Qué capacidades posee cada agente.
- Qué acciones puede ejecutar.
- Qué grado de autonomía se le concede.
- Cómo se preservan el contexto, la memoria y la procedencia de la información.
- Cómo se detectan, limitan y revierten comportamientos no deseados.
- Qué riesgos permanecen después de implementar los controles.

Esta sección desarrolla el modelo mental necesario para responder esas preguntas antes de profundizar en amenazas, arquitecturas, controles y técnicas de validación específicas.

---

## 🎯 Objetivo

El objetivo de `01-Fundamentals` es establecer un vocabulario técnico consistente y una base de razonamiento para el resto del repositorio.

Al completar esta sección, el lector debería ser capaz de:

- Distinguir un modelo, una aplicación LLM, un asistente, un copiloto, un agente y un sistema multiagente.
- Identificar los componentes principales de una arquitectura agéntica.
- Analizar la relación entre autonomía, capacidad, autoridad y delegación.
- Reconocer activos específicos de sistemas basados en agentes.
- Identificar Trust Boundaries y flujos de información relevantes.
- Diferenciar amenazas, vulnerabilidades, escenarios de abuso y riesgos.
- Comprender cómo cambian los objetivos de seguridad cuando el software puede actuar.
- Diferenciar Security, Safety, Privacy, Reliability, Resilience y Trustworthiness.
- Seleccionar familias de controles alineadas con riesgos concretos.
- Aplicar principios de ingeniería segura durante el diseño de sistemas agénticos.

---

## 🏛️ Enfoque del repositorio

Este repositorio no está organizado como un catálogo de ataques contra prompts.

Su enfoque es **Agentic Security Engineering**.

Esto implica estudiar el sistema completo:

```text
Business Objectives
        │
        ▼
Agentic Use Case
        │
        ▼
Assets and Identities
        │
        ▼
Autonomy and Capabilities
        │
        ▼
Trust Boundaries
        │
        ▼
Threat Scenarios
        │
        ▼
Risk Assessment
        │
        ▼
Security Objectives
        │
        ▼
Security Controls
        │
        ▼
Validation and Monitoring
        │
        ▼
Secure Agentic System
```

El foco no se limita a descubrir cómo podría fallar un agente.

También incluye:

- Cómo debería diseñarse.
- Qué autoridad debería recibir.
- Cómo deben restringirse sus acciones.
- Cómo verificar que los controles funcionan.
- Cómo observar su comportamiento.
- Cómo contenerlo ante un incidente.
- Cómo aprender de los fallos y mejorar el sistema.

---

## ⚠️ ¿Por qué los sistemas agénticos requieren un análisis específico?

El uso de un LLM no convierte automáticamente a una aplicación en un sistema agéntico.

El cambio aparece cuando el sistema adquiere capacidad para:

- Interpretar objetivos de alto nivel.
- Descomponerlos en tareas.
- Seleccionar acciones.
- Invocar herramientas.
- Consultar fuentes externas.
- Mantener estado o memoria.
- Adaptar su comportamiento según resultados anteriores.
- Delegar tareas a otros agentes.
- Producir consecuencias fuera del entorno del modelo.

En una aplicación generativa tradicional, una salida incorrecta puede producir contenido defectuoso.

En un sistema agéntico, esa misma salida puede convertirse en una acción:

```text
Incorrect Interpretation
          │
          ▼
Incorrect Plan
          │
          ▼
Unauthorized Tool Call
          │
          ▼
External Side Effect
          │
          ▼
Business Impact
```

El riesgo diferencial no proviene únicamente de lo que el modelo genera.

Proviene de aquello que el sistema puede **decidir, recordar, autorizar, delegar y ejecutar**.

---

## 🔐 Principio rector

> **Capability is not authorization.**

Que un agente posea la capacidad técnica para ejecutar una acción no significa que esté autorizado a realizarla.

Un sistema seguro debe diferenciar claramente:

```text
User Intent
     ≠
Agent Interpretation
     ≠
Technical Capability
     ≠
Granted Authority
     ≠
Authorized Action
```

Esta distinción será utilizada a lo largo de todo el repositorio para analizar identidades, permisos, delegaciones, herramientas, MCP, memoria y sistemas multiagente.

---

## 🗺️ Ruta de aprendizaje

Los documentos fueron organizados de manera incremental. Cada uno incorpora conceptos necesarios para comprender el siguiente.

| Orden | Documento | Pregunta principal |
|------:|-----------|--------------------|
| 01 | [Building an Agentic Security Mindset](01-Building-an-Agentic-Security-Mindset.md) | ¿Cómo debe analizarse la seguridad de un sistema agéntico? |
| 02 | [What Is an AI Agent?](02-What-Is-an-AI-Agent.md) | ¿Qué convierte a un sistema en un agente? |
| 03 | [From LLM Applications to Agentic Systems](03-From-LLM-Applications-to-Agentic-Systems.md) | ¿Qué cambia al pasar de generar contenido a ejecutar acciones? |
| 04 | [Agentic System Components](04-Agentic-System-Components.md) | ¿Qué componentes forman una arquitectura agéntica? |
| 05 | [Autonomy, Agency and Delegation](05-Autonomy-Agency-and-Delegation.md) | ¿Cómo se relacionan capacidad, autoridad, autonomía y delegación? |
| 06 | [Agentic Assets](06-Agentic-Assets.md) | ¿Qué posee valor y necesita protección? |
| 07 | [Agentic Trust Boundaries](07-Agentic-Trust-Boundaries.md) | ¿Dónde cambian la confianza, el control y la autoridad? |
| 08 | [Agentic Threats and Risk](08-Agentic-Threats-and-Risk.md) | ¿Qué escenarios pueden materializarse y qué impacto tendrían? |
| 09 | [Agentic Security Objectives](09-Agentic-Security-Objectives.md) | ¿Qué propiedades debe preservar un sistema capaz de actuar? |
| 10 | [Safety, Security and Trustworthiness](10-Safety-Security-and-Trustworthiness.md) | ¿Cómo se diferencian seguridad, safety y confianza? |
| 11 | [Agentic Security Controls](11-Agentic-Security-Controls.md) | ¿Qué familias de controles permiten reducir el riesgo? |
| 12 | [Agentic Security Engineering Principles](12-Agentic-Security-Engineering-Principles.md) | ¿Qué principios deberían guiar el diseño de estos sistemas? |

---

## 📚 Contenido de la sección

### 01 — Building an Agentic Security Mindset

Introduce el modelo de razonamiento utilizado en todo el repositorio.

El foco deja de estar únicamente en:

```text
Prompt Injection
      │
      ▼
Mitigation
```

y pasa a considerar:

```text
Business Objective
        │
        ▼
Agentic Assets
        │
        ▼
Autonomy and Authority
        │
        ▼
Trust Boundaries
        │
        ▼
Threat Scenarios
        │
        ▼
Risk
        │
        ▼
Controls and Architecture
```

---

### 02 — What Is an AI Agent?

Establece diferencias entre:

- Model.
- LLM Application.
- Chatbot.
- Assistant.
- Copilot.
- Agent.
- Agentic System.
- Multi-Agent System.

También introduce conceptos como percepción, razonamiento, planificación, acción, estado, memoria, herramientas y supervisión humana.

---

### 03 — From LLM Applications to Agentic Systems

Analiza la transición desde aplicaciones que generan respuestas hacia sistemas que pueden producir consecuencias externas.

Se estudian:

- Agent loops.
- Multi-step workflows.
- Tool invocation.
- State and memory.
- Delegation.
- External side effects.
- Compositional risk.
- Emergent behavior.

---

### 04 — Agentic System Components

Define el vocabulario arquitectónico utilizado en las secciones posteriores.

Incluye, entre otros:

- User or Principal.
- Agent Runtime.
- Model Endpoint.
- Orchestrator.
- Planner.
- System Prompt.
- Context Window.
- Short-Term and Long-Term Memory.
- Tool Registry.
- MCP Client and Server.
- RAG Pipeline.
- Vector Store.
- Policy Engine.
- Identity Provider.
- Human Approval Layer.
- Observability Layer.
- External APIs.
- Other Agents.

---

### 05 — Autonomy, Agency and Delegation

Profundiza en conceptos que suelen utilizarse indistintamente, pero representan propiedades diferentes:

- Capability.
- Authority.
- Permission.
- Autonomy.
- Agency.
- Delegation.
- Impersonation.
- Accountability.
- Scope.
- Duration.
- Revocation.
- Reversibility.

Este documento analiza quién concede autoridad, cómo se limita, si puede delegarse y quién conserva la responsabilidad por las acciones ejecutadas.

---

### 06 — Agentic Assets

Identifica activos que no siempre aparecen en aplicaciones tradicionales:

- Goals.
- Plans.
- System prompts.
- Agent state.
- Memory.
- Context.
- Tool definitions.
- Capability mappings.
- Delegation grants.
- Access tokens.
- Retrieved documents.
- Embeddings.
- Vector stores.
- Models and endpoints.
- Policies.
- Audit evidence.
- Business actions.

La identificación de activos proporciona el contexto necesario para evaluar amenazas y justificar controles.

---

### 07 — Agentic Trust Boundaries

Analiza los puntos donde cambian:

- La propiedad de los datos.
- El nivel de confianza.
- La identidad.
- La autoridad.
- La responsabilidad.
- El dominio de administración.

Entre ellos:

```text
User → Agent
Agent → Model
Agent → Memory
Agent → Tool
Agent → MCP Server
Agent → RAG Source
Agent → External API
Agent → Another Agent
Tenant → Tenant
```

Una Trust Boundary no es solamente una separación de red. También puede representar un cambio de identidad, autorización, procedencia o control organizacional.

---

### 08 — Agentic Threats and Risk

Introduce el modelo de riesgo utilizado por el repositorio:

```text
Asset
   │
   ▼
Threat Source
   │
   ▼
Threat Scenario
   │
   ▼
Vulnerability or Unsafe Condition
   │
   ▼
Likelihood and Impact
   │
   ▼
Risk
   │
   ▼
Controls
   │
   ▼
Residual Risk
```

También incorpora factores específicos de sistemas agénticos:

- Autonomy level.
- Permission scope.
- Tool sensitivity.
- Memory persistence.
- Action reversibility.
- Delegation depth.
- Human oversight.
- Propagation potential.
- Blast radius.
- Cascading impact.

---

### 09 — Agentic Security Objectives

Parte de objetivos tradicionales como Confidentiality, Integrity y Availability, pero incorpora propiedades necesarias para sistemas capaces de actuar:

- Goal Integrity.
- Action Integrity.
- Authorization.
- Accountability.
- Controllability.
- Traceability.
- Isolation.
- Reversibility.
- Human Oversight.
- Data Provenance.
- Delegation Integrity.

Estos objetivos no reemplazan la tríada CIA. La complementan para representar riesgos derivados de autonomía, memoria, herramientas y delegación.

---

### 10 — Safety, Security and Trustworthiness

Diferencia conceptos relacionados, pero no equivalentes:

| Propiedad | Pregunta principal |
|-----------|--------------------|
| Security | ¿El sistema resiste comportamiento adversarial? |
| Safety | ¿Evita generar daños incluso sin un atacante? |
| Privacy | ¿Utiliza y protege adecuadamente los datos personales? |
| Reliability | ¿Se comporta consistentemente bajo condiciones esperadas? |
| Resilience | ¿Puede resistir, recuperarse y adaptarse? |
| Trustworthiness | ¿El sistema reúne las propiedades necesarias para ser utilizado responsablemente? |

Un sistema puede resistir ataques y, aun así, producir una acción insegura por ambigüedad, error o comportamiento emergente.

---

### 11 — Agentic Security Controls

Introduce las principales familias de controles:

- Identity Controls.
- Authorization Controls.
- Input and Context Controls.
- Memory Controls.
- Tool and Execution Controls.
- Human Oversight Controls.
- Data Protection Controls.
- Observability Controls.
- Recovery Controls.
- Governance Controls.

También analiza cómo los controles pueden actuar de manera:

- Preventive.
- Detective.
- Corrective.
- Recovery.
- Compensating.

---

### 12 — Agentic Security Engineering Principles

Cierra la sección con principios reutilizables durante el diseño:

- Least Agency.
- Least Privilege.
- Explicit Authorization.
- Deny by Default.
- Verify Before Acting.
- Separate Instructions from Data.
- Preserve Provenance.
- Constrain Tools and Capabilities.
- Minimize Persistent Memory.
- Design for Revocation.
- Prefer Reversible Actions.
- Require Approval for High-Impact Actions.
- Make Decisions and Actions Observable.
- Limit Blast Radius.
- Assume Compromise.
- Fail Safely.
- Design for Recovery.
- Avoid Implicit Delegation.
- Treat External Content as Untrusted.
- Security by Construction, not by Prompt.

---

## 🔗 Relación con el resto del repositorio

`01-Fundamentals` no pretende profundizar en cada amenaza o implementación.

Su función es proporcionar la base conceptual utilizada por las secciones especializadas.

```text
01-Fundamentals
       │
       ├── 02-Architecture
       ├── 03-Assets-and-Trust-Boundaries
       ├── 04-Threat-Modeling
       ├── 05-Agentic-Threats
       ├── 06-Identity-and-Authorization
       ├── 07-Tool-and-Action-Security
       ├── 08-MCP-Security
       ├── 09-Memory-and-Context-Security
       ├── 10-RAG-and-Data-Security
       ├── 11-Multi-Agent-Security
       ├── 12-AI-Supply-Chain
       ├── 13-Security-Controls
       ├── 14-Secure-Design-Patterns
       ├── 15-Validation-and-Red-Teaming
       ├── 16-Observability-and-Detection
       ├── 17-Incident-Response
       └── 18-Governance-and-Risk
```

Ejemplos:

- Los conceptos de **capability** y **authority** se aplicarán en `06-Identity-and-Authorization`.
- Las Trust Boundaries se utilizarán en `04-Threat-Modeling`.
- La ejecución de acciones se profundizará en `07-Tool-and-Action-Security`.
- Los principios de consentimiento y autorización se aplicarán en `08-MCP-Security`.
- La procedencia y persistencia se desarrollarán en `09-Memory-and-Context-Security`.
- Los objetivos de controllability y traceability se validarán en `15-Validation-and-Red-Teaming` y `16-Observability-and-Detection`.

---

## 🧭 Rutas recomendadas

### Para Application Security Engineers

```text
Fundamentals
    → Architecture
    → Threat Modeling
    → Security Controls
    → Secure Design Patterns
    → Validation
```

### Para AI, ML y Software Engineers

```text
Fundamentals
    → Architecture
    → Tool and Action Security
    → Memory and Context Security
    → RAG and Data Security
    → Checklists
```

### Para Security Architects

```text
Fundamentals
    → Assets and Trust Boundaries
    → Identity and Authorization
    → MCP Security
    → Multi-Agent Security
    → Secure Design Patterns
```

### Para Red Teamers y Pentesters

```text
Fundamentals
    → Threat Modeling
    → Agentic Threats
    → MCP Security
    → Validation and Red Teaming
    → Labs
```

### Para Technical Managers and Security Leaders

```text
Fundamentals
    → Governance and Risk
    → Security Controls
    → Observability and Detection
    → Incident Response
    → Checklists
```

---

## 🛠️ Criterios editoriales

El contenido de esta sección sigue los siguientes principios:

### Risk-driven

Los controles se justifican por el riesgo que reducen, no por la popularidad de una tecnología.

### Architecture-aware

Los hallazgos se analizan dentro del sistema completo y no como problemas aislados.

### Threat-informed

Las decisiones consideran escenarios de abuso, capacidades adversarias y evidencia disponible.

### Vendor-neutral

Los conceptos se presentan de manera independiente de proveedores y frameworks específicos.

### Lifecycle-oriented

La seguridad se analiza durante el diseño, desarrollo, despliegue, operación y retiro del sistema.

### Evidence-based

Las afirmaciones técnicas se respaldan mediante documentación oficial, estándares, investigaciones o casos públicos verificables.

### Practical

Cada concepto debe poder traducirse en decisiones de arquitectura, controles, pruebas o actividades operativas.

### Continuously reviewed

Agentic Security es un dominio en evolución. Los documentos se revisarán a medida que cambien los estándares, protocolos, arquitecturas y técnicas de ataque.

---

## 📊 Estado de la sección

| Documento | Estado |
|-----------|--------|
| Building an Agentic Security Mindset | ⏳ Planned |
| What Is an AI Agent? | ⏳ Planned |
| From LLM Applications to Agentic Systems | ⏳ Planned |
| Agentic System Components | ⏳ Planned |
| Autonomy, Agency and Delegation | ⏳ Planned |
| Agentic Assets | ⏳ Planned |
| Agentic Trust Boundaries | ⏳ Planned |
| Agentic Threats and Risk | ⏳ Planned |
| Agentic Security Objectives | ⏳ Planned |
| Safety, Security and Trustworthiness | ⏳ Planned |
| Agentic Security Controls | ⏳ Planned |
| Agentic Security Engineering Principles | ⏳ Planned |

Estados utilizados:

- ✅ Complete
- 🚧 In progress
- ⏳ Planned
- 🔄 Under review

---

## 📖 Referencias principales

Esta sección utiliza fuentes oficiales y comunitarias de reconocido prestigio como base técnica, sin reproducirlas literalmente.

### OWASP GenAI Security Project

- [OWASP Agentic Security Initiative](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- [OWASP Agentic AI — Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [OWASP Securing Agentic Applications Guide](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/)
- [OWASP Multi-Agentic System Threat Modeling Guide](https://genai.owasp.org/resource/multi-agentic-system-threat-modeling-guide-v1-0/)
- [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)

### NIST

- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST AI RMF Resources](https://airc.nist.gov/airmf-resources/airmf/)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/pubs/sp/800/218/final)
- [NIST Systems Security Engineering](https://csrc.nist.gov/pubs/sp/800/160/v1/r1/final)

### MITRE

- [MITRE ATLAS](https://atlas.mitre.org/)
- [MITRE SAFE-AI Framework](https://atlas.mitre.org/pdf-files/SAFEAI_Full_Report.pdf)

### Model Context Protocol

- [MCP Specification](https://modelcontextprotocol.io/specification/)
- [MCP Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)
- [Understanding Authorization in MCP](https://modelcontextprotocol.io/docs/tutorials/security/authorization)

Las referencias específicas utilizadas por cada tema se incluirán también al final de su documento correspondiente.

---

## ⚠️ Alcance

Esta sección no intenta reemplazar:

- La documentación oficial de OWASP, NIST, MITRE o MCP.
- Un proceso formal de Threat Modeling.
- Una revisión de arquitectura.
- Evaluaciones legales, regulatorias o de privacidad.
- Pruebas de seguridad realizadas sobre un sistema concreto.

Su propósito es organizar los fundamentos y proporcionar un modelo técnico que facilite el análisis posterior.

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas cuando:

- Corrigen errores técnicos.
- Incorporan fuentes oficiales relevantes.
- Mejoran la claridad de una explicación.
- Añaden escenarios reproducibles.
- Proponen controles o patrones con justificación técnica.
- Actualizan contenido afectado por cambios en estándares o protocolos.

Toda contribución debería:

1. Explicar claramente el problema.
2. Indicar qué riesgo o concepto aborda.
3. Incluir referencias verificables.
4. Evitar afirmaciones comerciales sin evidencia.
5. Distinguir hechos, recomendaciones e interpretaciones.
6. Mantener el enfoque vendor-neutral siempre que sea posible.

Consultar el archivo [`CONTRIBUTING.md`](../CONTRIBUTING.md) antes de enviar cambios.

---

## 📌 Idea central

> **Un sistema agéntico seguro no es aquel que nunca comete errores.**
>
> Es aquel cuya arquitectura limita qué puede hacer, verifica bajo qué identidad y autoridad actúa, preserva la procedencia de sus instrucciones y datos, registra cómo tomó sus decisiones y permite contener, revocar o revertir sus acciones cuando algo sale mal.

---

## ➡️ Comenzar

El recorrido recomendado comienza en:

### [01 — Building an Agentic Security Mindset](01-Building-an-Agentic-Security-Mindset.md)

En este primer documento se desarrolla la transición desde una seguridad centrada en prompts y vulnerabilidades individuales hacia un enfoque basado en arquitectura, activos, autoridad, riesgo y control.
