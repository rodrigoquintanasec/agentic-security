# 🛡️ Safety, Security and Trustworthiness

> Un sistema puede ser seguro y aun así producir resultados peligrosos. Del mismo modo, un sistema puede ser confiable desde el punto de vista funcional y, sin embargo, presentar graves problemas de seguridad. Comprender la diferencia entre **Safety**, **Security** y **Trustworthiness** constituye uno de los fundamentos de la ingeniería moderna de sistemas basados en inteligencia artificial.

---

# 🎯 Objetivo

Comprender las diferencias entre **Safety**, **Security** y **Trustworthiness**, cómo se complementan entre sí y por qué los tres conceptos resultan esenciales para diseñar agentes inteligentes confiables.

---

# 📖 Introducción

A medida que los sistemas basados en inteligencia artificial adquieren mayor autonomía, aparecen nuevos desafíos relacionados con la confianza.

Tradicionalmente, la seguridad informática se centró en responder preguntas como:

- ¿Puede un atacante comprometer el sistema?
- ¿Están protegidos los datos?
- ¿Existen vulnerabilidades?

Sin embargo, cuando un agente comienza a razonar, planificar y ejecutar acciones, aparecen preguntas diferentes.

Por ejemplo.

- ¿Tomará decisiones seguras?
- ¿Interpretará correctamente los objetivos?
- ¿Generará resultados confiables?
- ¿Podemos confiar en su comportamiento?

Responder estas preguntas requiere distinguir tres conceptos fundamentales.

- **Safety**
- **Security**
- **Trustworthiness**

Aunque se encuentran estrechamente relacionados, representan propiedades completamente diferentes.

---

# 🛡️ ¿Qué es Safety?

**Safety** representa la capacidad de un sistema para evitar causar daño accidental a personas, organizaciones o procesos, incluso cuando no existe un atacante.

El foco de Safety no son los ataques.

El foco son las consecuencias no intencionales.

Por ejemplo.

Un agente podría:

- eliminar información importante por error;
- interpretar incorrectamente una instrucción;
- recomendar una acción riesgosa;
- ejecutar una automatización equivocada;
- tomar una decisión basada en contexto incompleto.

En todos estos casos.

No existe necesariamente un atacante.

El problema proviene del propio comportamiento del sistema.

---

## Objetivo de Safety

El propósito consiste en minimizar el daño derivado de errores, decisiones incorrectas o comportamientos inesperados.

---

## Ejemplos

- Goal Drift.
- Hallucinations.
- Planeamiento incorrecto.
- Acciones irreversibles.
- Automatizaciones peligrosas.
- Errores de razonamiento.

---

# 🔒 ¿Qué es Security?

**Security** representa la capacidad de proteger el sistema frente a acciones maliciosas.

Aquí sí existe un adversario.

El objetivo consiste en impedir que un atacante comprometa:

- activos;
- identidades;
- autoridad;
- información;
- herramientas;
- infraestructura.

Security responde preguntas como:

- ¿Puede un atacante manipular el agente?
- ¿Puede obtener privilegios?
- ¿Puede comprometer la memoria?
- ¿Puede ejecutar herramientas no autorizadas?

---

## Objetivo de Security

Preservar la confidencialidad, integridad, disponibilidad y autoridad del sistema frente a amenazas deliberadas.

---

## Ejemplos

- Prompt Injection.
- Credential Theft.
- Tool Abuse.
- Confused Deputy.
- Privilege Amplification.
- Data Exfiltration.
- Supply Chain Attacks.

---

# 🤝 ¿Qué es Trustworthiness?

**Trustworthiness** representa el grado de confianza que una organización puede depositar en el comportamiento completo del sistema.

No constituye un mecanismo técnico específico.

Es una propiedad emergente.

Un sistema resulta confiable cuando demuestra consistentemente que:

- produce resultados correctos;
- protege la información;
- respeta las políticas;
- mantiene la autoridad delegada;
- explica sus decisiones;
- responde de manera predecible.

En otras palabras.

La confianza no se configura.

Se construye.

---

# 🧩 Trustworthiness como propiedad emergente

Podemos imaginar la confianza como el resultado de múltiples propiedades trabajando conjuntamente.

```text
Safety
      │
      ├──────────┐
      │          │
Security      Reliability
      │          │
      ├──────────┤
      │          │
Transparency Accountability
      │          │
      └──────────┘
             │
             ▼
      Trustworthiness
```

Ninguno de estos elementos resulta suficiente por sí solo.

La confianza aparece cuando todos funcionan de manera coordinada.

---

# ⚠️ Un sistema puede ser seguro pero no seguro para usar

Una diferencia importante consiste en comprender que un sistema puede ser muy resistente frente a ataques y, sin embargo, producir decisiones peligrosas.

Por ejemplo.

Un agente podría:

- autenticarse correctamente;
- utilizar políticas adecuadas;
- proteger credenciales;
- registrar auditoría.

Y aun así recomendar una decisión completamente equivocada debido a una alucinación.

En ese caso.

El sistema posee un buen nivel de **Security**.

Pero un bajo nivel de **Safety**.

---

# ⚠️ También puede ocurrir lo contrario

Imaginemos un agente extremadamente preciso.

Siempre responde correctamente.

Nunca alucina.

Nunca interpreta mal un objetivo.

Sin embargo.

Todas sus herramientas utilizan credenciales administrativas permanentes.

Un atacante logra comprometer una de ellas.

Ahora el agente continúa razonando perfectamente.

Pero la arquitectura ya no es segura.

Posee un buen nivel de **Safety**.

Pero un bajo nivel de **Security**.

---

# 🏛️ La relación entre los tres conceptos

Podemos visualizar estos conceptos como capas complementarias.

```text
Trustworthiness
        ▲
        │
 ┌──────┴──────┐
 │             │
Safety     Security
```

Safety protege frente a errores.

Security protege frente a ataques.

Trustworthiness representa la confianza obtenida cuando ambas dimensiones funcionan conjuntamente.

---

# 🏗️ Architecture Perspective

Una arquitectura madura incorpora controles destinados a fortalecer simultáneamente las tres propiedades.

```text
Business Goal
       │
       ▼
Agent
       │
────────────────────────────
Safety
• Goal Validation
• Guardrails
• HITL
────────────────────────────
Security
• Identity
• Policies
• Least Privilege
• Delegation
────────────────────────────
Trustworthiness
• Monitoring
• Audit
• Explainability
• Accountability
```

Ninguna de estas capas reemplaza a las demás.

Todas trabajan de forma complementaria.

---

# 💡 Pensar como un Agentic Security Engineer

Cuando un arquitecto analiza un sistema basado en IA normalmente intenta responder tres preguntas diferentes.

## Safety

- ¿Puede causar daño sin intención?

## Security

- ¿Puede un atacante abusar del sistema?

## Trustworthiness

- ¿Podemos confiar consistentemente en su comportamiento?

Responder únicamente una de estas preguntas deja incompleta la evaluación.

---

# 📌 Key Ideas

Los sistemas agénticos modernos deben diseñarse considerando tres dimensiones complementarias.

- **Safety** protege frente a errores y comportamientos no intencionales.
- **Security** protege frente a ataques deliberados.
- **Trustworthiness** representa la confianza obtenida cuando el sistema demuestra de forma consistente que actúa de manera segura, gobernada y predecible.

Comprender estas diferencias permite diseñar arquitecturas donde la inteligencia artificial no solamente sea poderosa, sino también responsable y confiable para operar en entornos reales.

---

# 📚 References

- NIST AI Risk Management Framework (AI RMF 1.0)
- ISO/IEC 42001 — Artificial Intelligence Management Systems
- Google Secure AI Framework (SAIF)
- Microsoft Responsible AI Standard
- OECD AI Principles
- Anthropic — Responsible Scaling Policy
- OpenAI Model Spec
- OWASP Agentic Security Initiative
- MITRE ATLAS
