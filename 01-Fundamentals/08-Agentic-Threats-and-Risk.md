# ⚠️ Agentic Threats and Risk

> Los sistemas agénticos introducen nuevas superficies de ataque, nuevos modelos de confianza y nuevas formas de delegar autoridad. Comprender sus amenazas constituye el primer paso para diseñar arquitecturas resilientes y gobernarlas de forma segura.

---

# 🎯 Objetivo

Comprender cómo evolucionan las amenazas y el riesgo en sistemas basados en agentes inteligentes, identificar las principales categorías de amenazas y desarrollar un modelo mental que permita realizar análisis de riesgo y Threat Modeling desde una perspectiva de Agentic Security.

---

# 📖 Introducción

Toda arquitectura posee riesgos.

Durante décadas, Application Security centró gran parte de sus esfuerzos en proteger aplicaciones frente a vulnerabilidades como:

- SQL Injection.
- Cross-Site Scripting (XSS).
- Broken Access Control.
- Server-Side Request Forgery (SSRF).
- Deserialización insegura.

Estas amenazas continúan existiendo.

Sin embargo, los sistemas agénticos incorporan nuevas capacidades que modifican profundamente el panorama de riesgo.

Ahora un agente puede:

- interpretar objetivos;
- tomar decisiones;
- planificar múltiples pasos;
- utilizar herramientas;
- delegar trabajo;
- colaborar con otros agentes;
- acceder a conocimiento dinámico.

Como consecuencia, el riesgo deja de depender únicamente de vulnerabilidades técnicas.

También depende del comportamiento autónomo del sistema.

---

# 🏛️ ¿Qué es una Threat?

Desde la perspectiva de seguridad, una **Threat** representa cualquier evento, actor o condición capaz de comprometer la confidencialidad, integridad o disponibilidad de un activo.

Una amenaza no implica necesariamente que exista una vulnerabilidad.

Representa la posibilidad de que ocurra un evento con impacto sobre el negocio.

Por ejemplo.

- un atacante;
- un error humano;
- una herramienta comprometida;
- una configuración incorrecta;
- un agente mal diseñado;
- un proveedor externo.

Las amenazas existen incluso cuando la arquitectura no presenta vulnerabilidades conocidas.

---

# 🎯 ¿Qué es el Risk?

El riesgo representa la combinación entre:

- la probabilidad de que una amenaza ocurra;
- el impacto que tendría sobre la organización.

Podemos representarlo de forma simplificada.

```text
Risk

=

Threat

×

Likelihood

×

Impact
```

Un mismo incidente puede producir riesgos completamente distintos según el contexto del negocio.

Por ejemplo.

Una respuesta incorrecta generada por un chatbot público probablemente tenga un impacto reducido.

La misma decisión tomada por un agente responsable de administrar identidades privilegiadas podría afectar toda la organización.

---

# 🤖 ¿Por qué cambia el riesgo en Agentic Security?

En aplicaciones tradicionales, la mayoría de las acciones eran completamente determinísticas.

En un sistema agéntico aparecen nuevas variables.

Por ejemplo.

- razonamiento probabilístico;
- planificación dinámica;
- uso de herramientas externas;
- memoria persistente;
- colaboración entre agentes;
- delegación de autoridad.

Cada una de estas capacidades incorpora nuevos escenarios de riesgo.

En consecuencia.

Ya no basta con preguntarse:

> **¿Existe una vulnerabilidad?**

Ahora también resulta necesario analizar:

- ¿Qué puede decidir el agente?
- ¿Qué autoridad posee?
- ¿Qué herramientas puede utilizar?
- ¿Qué ocurre si interpreta incorrectamente un objetivo?
- ¿Qué impacto tendría una mala decisión?

---

# 🧩 Principales categorías de amenazas

Aunque el ecosistema evoluciona rápidamente, la mayoría de las amenazas actuales pueden agruparse en varias categorías.

## 🎭 Prompt Manipulation

Intentos de modificar el comportamiento del agente mediante instrucciones maliciosas o ambiguas.

Ejemplos.

- Prompt Injection.
- Indirect Prompt Injection.
- Prompt Leakage.

---

## 🔑 Identity & Delegation

Problemas relacionados con autoridad e identidades.

Por ejemplo.

- Confused Deputy.
- Privilege Amplification.
- Delegación excesiva.
- Credenciales comprometidas.

---

## 🔧 Tool Abuse

El agente utiliza herramientas de forma insegura.

Ejemplos.

- Tool Injection.
- Ejecución no autorizada.
- Acceso a APIs críticas.
- Uso excesivo de herramientas.

---

## 🧠 Memory Manipulation

Ataques dirigidos a alterar el conocimiento utilizado por el agente.

Por ejemplo.

- Poisoning.
- Context Manipulation.
- Memory Corruption.

---

## 📚 Knowledge Manipulation

Compromiso de fuentes utilizadas mediante RAG o documentación.

Ejemplos.

- Document Poisoning.
- Knowledge Base Manipulation.
- Retrieval Manipulation.

---

## 🤝 Multi-Agent Risks

Amenazas que aparecen únicamente cuando participan varios agentes.

Por ejemplo.

- Delegation Chains.
- Authority Propagation.
- Cross-Agent Trust.
- Cascading Failures.

---

# ⚠️ El riesgo no depende únicamente del modelo

Uno de los errores más frecuentes consiste en pensar que el riesgo depende principalmente del LLM.

En realidad.

El modelo representa únicamente uno de los componentes de la arquitectura.

El riesgo también depende de:

- herramientas;
- identidades;
- políticas;
- memorias;
- usuarios;
- APIs;
- infraestructura;
- procesos de negocio.

Por este motivo.

Dos organizaciones pueden utilizar exactamente el mismo modelo y presentar niveles de riesgo completamente diferentes.

---

# 📊 Evaluando el riesgo

Una forma práctica consiste en analizar cada activo considerando preguntas como las siguientes.

## ¿Qué activo estamos protegiendo?

Por ejemplo.

- Modelo.
- Memoria.
- Herramienta.
- Base de conocimiento.
- Identidad.

---

## ¿Qué amenaza podría afectarlo?

Por ejemplo.

- Prompt Injection.
- Compromiso de identidad.
- Tool Abuse.

---

## ¿Cuál sería el impacto?

- Financiero.
- Operativo.
- Reputacional.
- Regulatorio.

---

## ¿Qué controles existen actualmente?

- Policies.
- Human Oversight.
- Logging.
- Runtime Monitoring.
- Approval Gates.

---

## ¿Qué riesgo residual permanece?

Todo riesgo que continúa existiendo después de aplicar controles.

---

# 🏛️ Threat Modeling en sistemas agénticos

El Threat Modeling continúa siendo una de las herramientas más importantes de la seguridad.

Sin embargo.

Los modelos tradicionales deben ampliarse para considerar aspectos propios de la IA.

Por ejemplo.

Además de analizar:

- usuarios;
- servidores;
- APIs;
- bases de datos.

Ahora también deben incorporarse.

- agentes;
- modelos;
- memorias;
- herramientas;
- cadenas de delegación;
- Trust Boundaries;
- autoridad.

Este cambio representa una evolución natural del Threat Modeling clásico.

No un reemplazo.

---

# 💡 Pensar como un Agentic Security Engineer

Antes de hablar sobre controles, un arquitecto normalmente intenta responder preguntas como:

- ¿Qué decisiones puede tomar este agente?
- ¿Qué autoridad posee?
- ¿Qué activos protege?
- ¿Qué herramientas puede utilizar?
- ¿Qué límites de confianza atraviesa?
- ¿Qué amenazas aparecen en cada transición?
- ¿Cuál es el peor escenario posible?
- ¿Cuál es el Blast Radius máximo?

Responder estas preguntas permite comprender el riesgo antes de intentar mitigarlo.

---

# 📌 Key Ideas

Las amenazas de los sistemas agénticos no reemplazan a las amenazas tradicionales de Application Security.

Las amplían.

Los agentes incorporan nuevas capacidades relacionadas con razonamiento, autonomía, herramientas, memoria y delegación de autoridad.

Como consecuencia.

El análisis de riesgo también debe evolucionar.

Una arquitectura segura comienza identificando:

- los activos;
- los límites de confianza;
- las amenazas;
- el impacto potencial;
- los controles existentes;
- el riesgo residual.

Este enfoque permite diseñar controles alineados con el verdadero comportamiento del sistema y no únicamente con sus componentes tecnológicos.

En el próximo capítulo profundizaremos precisamente en esos mecanismos de protección analizando los **Agentic Security Objectives**, es decir, las propiedades que una arquitectura debe preservar para que los agentes operen de forma segura, confiable y gobernada.

---

# 📚 References

- NIST AI Risk Management Framework (AI RMF)
- NIST SP 800-30 — Guide for Conducting Risk Assessments
- NIST SP 800-37 — Risk Management Framework
- NIST SP 800-53 Rev. 5
- NIST SP 800-207 — Zero Trust Architecture
- OWASP Agentic Security Initiative
- OWASP Top 10 for LLM Applications
- MITRE ATLAS
- Google Secure AI Framework (SAIF)
- Microsoft AI Security
