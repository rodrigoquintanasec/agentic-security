# 🧩 Agentic System Components

> Un sistema agéntico no está compuesto únicamente por un modelo.
>
> Es una arquitectura distribuida donde objetivos, instrucciones, identidades, memoria, herramientas, datos, políticas y mecanismos de ejecución colaboran para transformar una intención en acciones sobre un entorno.

---

## Introducción

El término **AI Agent** suele asociarse visualmente con un modelo conectado a un conjunto de herramientas.

```text
Model
  │
  ▼
Tools
```

Aunque este esquema puede resultar útil como primera aproximación, es insuficiente para analizar la seguridad de un sistema real.

Una aplicación agéntica moderna puede incluir:

- Usuarios y sistemas originadores.
- Interfaces de aplicación.
- Agent runtimes.
- Model endpoints.
- System instructions.
- Context builders.
- Planners.
- Orchestrators.
- Estado de ejecución.
- Memoria de corto y largo plazo.
- Pipelines RAG.
- Vector stores.
- Tool registries.
- Tool brokers.
- MCP clients y servers.
- Identity Providers.
- Authorization Services.
- Policy Engines.
- Human Approval Layers.
- Sandboxes.
- Observability platforms.
- External APIs.
- Otros agentes.

Cada componente incorpora responsabilidades, activos, relaciones de confianza y posibles condiciones de fallo.

```text
User
  │
  ▼
Application
  │
  ▼
Agent Runtime
  │
  ├── Model
  ├── Instructions
  ├── Context
  ├── Planner
  ├── State
  ├── Memory
  ├── Policy Enforcement
  ├── Tool Registry
  └── Orchestration
          │
          ├── RAG Sources
          ├── MCP Servers
          ├── External APIs
          ├── Enterprise Systems
          └── Other Agents
```

El modelo es una parte relevante del sistema.

No es el sistema completo.

Esta distinción importa porque una arquitectura puede utilizar un modelo razonablemente robusto y continuar siendo insegura debido a:

- Permisos excesivos.
- Identidades compartidas.
- Herramientas mal diseñadas.
- Ausencia de autorización por acción.
- Contexto contaminado.
- Memoria sin procedencia.
- Trust Boundaries no identificadas.
- Delegaciones implícitas.
- Falta de trazabilidad.
- Contención insuficiente.

Por este motivo, el análisis de seguridad debe comenzar descomponiendo el sistema en componentes y entendiendo qué responsabilidad posee cada uno.

---

## Objetivo

El objetivo de este capítulo es establecer el vocabulario arquitectónico utilizado en el resto del repositorio.

Al finalizar, el lector debería ser capaz de:

- Identificar los componentes principales de un sistema agéntico.
- Diferenciar el modelo del Agent Runtime y de la aplicación que los contiene.
- Comprender cómo se construye el contexto operativo.
- Distinguir estado, memoria y conocimiento recuperado.
- Reconocer la función de planners, orchestrators y policy engines.
- Analizar herramientas como extensiones de capacidad y autoridad.
- Comprender el papel de MCP clients, hosts y servers.
- Identificar nuevas identidades y cadenas de delegación.
- Reconocer dónde aparecen Trust Boundaries.
- Asignar responsabilidades de seguridad a cada componente.
- Detectar arquitecturas donde el modelo recibe responsabilidades que deberían pertenecer a controles deterministas.
- Construir un inventario técnico previo al Threat Modeling.

---

## Una arquitectura de referencia

No existe una única arquitectura válida para todos los sistemas agénticos.

Sin embargo, el siguiente modelo permite visualizar los componentes que aparecen con mayor frecuencia.

```text
┌───────────────────────────────────────────────────────────────┐
│                        User / Principal                       │
└──────────────────────────────┬────────────────────────────────┘
                               │
                               ▼
┌───────────────────────────────────────────────────────────────┐
│                     Application / Agent Host                  │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │                    Agent Runtime                        │  │
│  │                                                         │  │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────────────┐   │  │
│  │  │Instructions│  │  Planner  │  │ Context Builder   │   │  │
│  │  └───────────┘  └───────────┘  └───────────────────┘   │  │
│  │                                                         │  │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────────────┐   │  │
│  │  │   State   │  │  Memory   │  │ Model Interface   │   │  │
│  │  └───────────┘  └───────────┘  └───────────────────┘   │  │
│  │                                                         │  │
│  │  ┌───────────┐  ┌───────────┐  ┌───────────────────┐   │  │
│  │  │ Tool      │  │ Policy    │  │ Human Approval    │   │  │
│  │  │ Registry  │  │ Client    │  │ Workflow          │   │  │
│  │  └───────────┘  └───────────┘  └───────────────────┘   │  │
│  └─────────────────────────────────────────────────────────┘  │
└─────────────┬──────────────────┬──────────────────┬───────────┘
              │                  │                  │
              ▼                  ▼                  ▼
      ┌─────────────┐    ┌─────────────┐    ┌──────────────┐
      │ Model       │    │ Identity &  │    │ Observability│
      │ Provider    │    │ Policy      │    │ Platform     │
      └─────────────┘    └─────────────┘    └──────────────┘
              │
              ▼
┌───────────────────────────────────────────────────────────────┐
│                    Capabilities and Context                   │
│                                                               │
│  ┌───────────┐ ┌───────────┐ ┌──────────┐ ┌──────────────┐  │
│  │ RAG       │ │ MCP       │ │ Native   │ │ Other Agents │  │
│  │ Pipeline  │ │ Servers   │ │ Tools    │ │              │  │
│  └───────────┘ └───────────┘ └──────────┘ └──────────────┘  │
└──────────────────────────────┬────────────────────────────────┘
                               │
                               ▼
┌───────────────────────────────────────────────────────────────┐
│                      External Systems                         │
│                                                               │
│  Databases · SaaS · APIs · Files · Infrastructure · Devices  │
└───────────────────────────────────────────────────────────────┘
```

No todos los sistemas implementarán todos estos bloques.

Una solución simple puede contener únicamente:

```text
User
  │
  ▼
Application
  │
  ▼
Model
  │
  ▼
Read-Only Tool
```

Una plataforma empresarial puede contener:

- Múltiples agentes.
- Modelos de diferentes proveedores.
- Memoria por tenant.
- Herramientas internas.
- Servidores MCP remotos.
- Delegaciones.
- Aprobaciones humanas.
- Policy Enforcement Points.
- Controles de red.
- Observabilidad centralizada.

La complejidad arquitectónica debería corresponder al valor y al riesgo del caso de uso.

---

## Cómo analizar cada componente

A lo largo de este capítulo, cada componente será evaluado mediante cinco dimensiones.

### Purpose

¿Qué función cumple dentro del sistema?

### Assets

¿Qué información, identidad, estado o capacidad protege?

### Trust Assumptions

¿Qué condiciones supone que son verdaderas?

### Security Responsibilities

¿Qué controles debería aplicar?

### Common Failure Modes

¿De qué manera puede fallar, ser manipulado o ampliar el impacto de un incidente?

Esta estructura permite evitar análisis puramente funcionales.

Un componente no debe documentarse solamente por lo que hace.

También debe documentarse por:

- La autoridad que recibe.
- La información que procesa.
- Los componentes en los que confía.
- Los controles que aplica.
- Las consecuencias de su compromiso.

---

# 1. User, Principal y Request Originator

## Purpose

El **User** es la persona que interactúa con el sistema.

El **Principal** es la entidad de seguridad cuya identidad y autoridad se utilizan para evaluar una operación.

No siempre son equivalentes.

Un request puede originarse desde:

- Una persona.
- Una aplicación.
- Un workflow.
- Un scheduler.
- Un servicio.
- Otro agente.
- Un evento.
- Una cola.
- Una integración externa.

```text
Human User
     │
     ▼
Frontend
     │
     ▼
Backend Service
     │
     ▼
Agent
```

En este flujo pueden existir varias identidades:

- Identidad humana.
- Identidad de la sesión.
- Identidad de la aplicación.
- Identidad del agente.
- Identidad utilizada por una herramienta.

## Assets

- Identidad.
- Sesión.
- Credenciales.
- Consentimiento.
- Preferencias.
- Datos proporcionados.
- Autoridad delegada.
- Intención original.
- Límites indicados por el usuario.

## Trust Assumptions

El sistema puede asumir incorrectamente que:

- El usuario fue autenticado correctamente.
- La sesión continúa siendo legítima.
- La instrucción refleja una intención informada.
- El usuario posee autoridad sobre el recurso.
- El request no fue manipulado.
- La interfaz representa con precisión la acción.
- El usuario comprende sus consecuencias.

## Security Responsibilities

- Autenticar al principal.
- Preservar su identidad durante todo el workflow.
- Verificar autorización sobre cada recurso.
- Registrar consentimiento cuando sea necesario.
- Limitar el alcance de la delegación.
- No inferir autoridad exclusivamente desde lenguaje natural.
- Separar identidad humana, identidad del agente e identidad de workload.
- Evitar que un agente amplíe los permisos originales.

## Common Failure Modes

- Session hijacking.
- Confusión entre usuarios.
- Cross-tenant access.
- Delegación excesiva.
- Impersonation.
- Aprobaciones ambiguas.
- Pérdida del identity chain.
- Uso de una identidad de servicio compartida.
- Autoridad inferida desde una conversación.
- Ejecución de acciones fuera del scope original.

> **La intención expresada por un usuario no reemplaza una decisión de autorización.**

---

# 2. Application y Agent Host

## Purpose

La **Application** o **Agent Host** es el entorno que contiene y coordina la experiencia agéntica.

Puede encargarse de:

- Recibir requests.
- Administrar sesiones.
- Inicializar agentes.
- Crear MCP clients.
- Seleccionar modelos.
- Inyectar contexto.
- Exponer herramientas.
- Aplicar políticas.
- Renderizar aprobaciones.
- Persistir estado.
- Registrar telemetría.

El host representa la frontera entre el usuario y la infraestructura agéntica.

## Assets

- Configuración.
- Sesiones.
- Routing rules.
- Tokens.
- Catálogo de agentes.
- Herramientas habilitadas.
- Políticas.
- Contexto del usuario.
- Tenant information.
- Secrets.
- Feature flags.

## Trust Assumptions

- Los componentes cargados son legítimos.
- Las configuraciones no fueron alteradas.
- Las herramientas corresponden al tenant correcto.
- Los plugins o MCP servers son confiables.
- Las sesiones están aisladas.
- El routing no mezcla contextos.
- Los secretos no se exponen al modelo.

## Security Responsibilities

- Aislar usuarios y tenants.
- Controlar qué agentes pueden iniciarse.
- Gestionar configuración de forma segura.
- Restringir integraciones.
- Preservar identity context.
- Crear clients separados cuando sea necesario.
- Proteger credenciales.
- Aplicar límites de ejecución.
- Coordinar aprobación, cancelación y recuperación.
- Mantener inventario de dependencias y componentes.

## Common Failure Modes

- Session confusion.
- Cross-tenant contamination.
- Tool exposure incorrecta.
- Secret leakage.
- Configuración insegura.
- Integraciones no autorizadas.
- Carga de agentes o plugins maliciosos.
- Uso de tokens globales.
- Falta de separación entre desarrollo y producción.
- Routing hacia modelos o herramientas incorrectas.

---

# 3. Agent Runtime

## Purpose

El **Agent Runtime** ejecuta el ciclo operativo del agente.

Coordina:

- Objetivos.
- Instrucciones.
- Contexto.
- Invocaciones al modelo.
- Tool calls.
- Estado.
- Memoria.
- Handoffs.
- Límites.
- Condiciones de finalización.

```text
Goal
  │
  ▼
Agent Runtime
  │
  ├── Build Context
  ├── Invoke Model
  ├── Parse Decision
  ├── Evaluate Policy
  ├── Execute Tool
  ├── Update State
  └── Continue or Stop
```

## Assets

- Estado de ejecución.
- Current plan.
- Tool availability.
- Runtime configuration.
- Error handling logic.
- Run identifiers.
- Approval state.
- Execution budgets.
- Cancellation signals.

## Trust Assumptions

- El modelo devolverá resultados parseables.
- Las herramientas respetarán sus contratos.
- El estado permanece íntegro.
- Los handlers son confiables.
- La política se aplica antes de ejecutar.
- Los límites no pueden ser evadidos.
- Los handoffs preservan restricciones.

## Security Responsibilities

- Aplicar límites de pasos, costo y tiempo.
- Validar outputs estructurados.
- Invocar policy checks.
- Mantener aislamiento de ejecución.
- Preservar identidad y scope.
- Evitar recursión ilimitada.
- Implementar cancellation.
- Manejar errores de forma segura.
- Registrar decisiones relevantes.
- No ejecutar directamente texto generado como código o comandos.

## Common Failure Modes

- Infinite loops.
- Tool call storms.
- State corruption.
- Bypass de approvals.
- Handoffs con autoridad excesiva.
- Parsing ambiguo.
- Error retries destructivos.
- Race conditions.
- Ejecución duplicada.
- Pérdida de restricciones después de una reanudación.
- Inconsistencia entre lo aprobado y lo ejecutado.

> **El Agent Runtime es una superficie de control. No debería limitarse a ser un simple loop de tool calling.**

---

# 4. Model y Model Endpoint

## Purpose

El modelo interpreta inputs, genera contenido y propone decisiones.

Puede participar en:

- Razonamiento.
- Clasificación.
- Planificación.
- Tool selection.
- Parameter generation.
- Resumen.
- Evaluación.
- Routing.
- Generación de respuestas.

El **Model Endpoint** representa el servicio mediante el cual se accede al modelo.

Puede ser:

- Interno.
- Hosted.
- Cloud.
- Third-party.
- Multi-model.
- Fallback-based.

## Assets

- Prompts.
- Context.
- Responses.
- Model configuration.
- Model identifier.
- Sampling parameters.
- Credentials del proveedor.
- Usage metadata.
- Fine-tuned artifacts.
- Routing logic.

## Trust Assumptions

- El endpoint corresponde al modelo esperado.
- El proveedor procesa los datos de acuerdo con la política.
- El modelo conserva capacidades conocidas.
- Una actualización no modifica comportamiento crítico.
- El output puede estructurarse correctamente.
- El proveedor aplica aislamiento adecuado.
- No existe cross-customer leakage.

## Security Responsibilities

- Minimizar datos enviados.
- Clasificar información antes de compartirla.
- Controlar modelos permitidos.
- Administrar credenciales.
- Validar outputs.
- Probar cambios de versión.
- Aplicar fallback de forma segura.
- Registrar model version.
- Definir límites de uso.
- Evitar que el modelo sea el único enforcement point.

## Common Failure Modes

- Hallucination.
- Prompt Injection susceptibility.
- Sensitive data disclosure.
- Model drift.
- Inconsistent tool selection.
- Malformed structured output.
- Model substitution.
- Provider outage.
- Unexpected behavior after upgrade.
- Data residency violations.
- Excessive trust in confidence-like language.

```text
Model Recommendation
        ≠
Security Decision
```

---

# 5. System Instructions y Prompt Templates

## Purpose

Las **System Instructions** definen el rol, el comportamiento esperado y las restricciones operativas del agente.

Pueden incluir:

- Objetivo.
- Scope.
- Estilo.
- Tool usage guidance.
- Escalamiento.
- Criterios de finalización.
- Reglas de seguridad.
- Políticas de interacción.

## Assets

- System prompt.
- Developer instructions.
- Prompt templates.
- Policy text.
- Tool descriptions.
- Few-shot examples.
- Hidden metadata.
- Routing instructions.

## Trust Assumptions

- Las instrucciones no fueron alteradas.
- El modelo mantendrá su prioridad.
- El contenido externo no las reemplazará.
- Las templates usan variables seguras.
- Los ejemplos no introducen comportamientos inseguros.
- Las descripciones de tools son legítimas.

## Security Responsibilities

- Gestionar versiones.
- Restringir modificaciones.
- Separar datos e instrucciones.
- Evitar secretos.
- Validar variables.
- Revisar cambios.
- Firmar o verificar integridad cuando corresponda.
- No depender exclusivamente del prompt para controles críticos.
- Documentar assumptions.

## Common Failure Modes

- Prompt leakage.
- Instruction override.
- Template injection.
- Tool description poisoning.
- Inclusión de secretos.
- Políticas contradictorias.
- Instrucciones obsoletas.
- Diferencias entre entornos.
- Dependencia excesiva del comportamiento del modelo.

> **Una instrucción expresa una expectativa. Un control técnico aplica una restricción.**

---

# 6. Context Builder

## Purpose

El **Context Builder** decide qué información será enviada al modelo en cada paso.

Puede incorporar:

- System instructions.
- User input.
- Historial.
- Estado.
- Memoria.
- RAG results.
- Tool outputs.
- Agent messages.
- Policies.
- Metadata.

```text
Instructions
     +
User Input
     +
Memory
     +
Retrieved Data
     +
Tool Results
     =
Model Context
```

El Context Builder es uno de los componentes más sensibles del sistema porque decide qué evidencia influirá sobre el razonamiento.

## Assets

- Context window.
- Source metadata.
- Trust labels.
- Tenant identifiers.
- Data classification.
- Relevance scores.
- Conversation history.
- Access-control attributes.
- Provenance.

## Trust Assumptions

- Las fuentes están correctamente etiquetadas.
- El contenido recuperado pertenece al usuario.
- Los resultados son relevantes.
- La procedencia no se pierde.
- No existen instrucciones ocultas en datos externos.
- El contexto no excede límites.
- La compresión no elimina restricciones importantes.

## Security Responsibilities

- Aplicar autorización antes de recuperar.
- Preservar procedencia.
- Separar fuentes por nivel de confianza.
- Minimizar información.
- Evitar cross-tenant leakage.
- Sanitizar metadata.
- Controlar context length.
- Priorizar instrucciones confiables.
- No mezclar políticas con contenido no confiable.
- Registrar qué fuentes se utilizaron.

## Common Failure Modes

- Context poisoning.
- Indirect Prompt Injection.
- Cross-user contamination.
- Loss of provenance.
- Retrieval of unauthorized data.
- Context overflow.
- Truncation of security instructions.
- Stale data.
- Compresión insegura.
- Mezcla de datos e instrucciones.
- Inclusion de secrets innecesarios.

---

# 7. Planner

## Purpose

El **Planner** transforma un objetivo en una secuencia de pasos o subobjetivos.

Puede ser:

- Una función del propio modelo.
- Un componente separado.
- Un agente especializado.
- Un workflow híbrido.
- Un plan predefinido con decisiones dinámicas.

```text
High-Level Goal
       │
       ▼
Planner
       │
       ├── Step 1
       ├── Step 2
       ├── Step 3
       └── Stop Condition
```

## Assets

- Objetivo.
- Plan.
- Subtasks.
- Dependencias.
- Recursos.
- Criterios de éxito.
- Criterios de finalización.
- Prioridades.
- Constraints.

## Trust Assumptions

- El objetivo fue interpretado correctamente.
- El plan permanece dentro del scope.
- Los pasos son necesarios.
- Las dependencias son válidas.
- Las herramientas seleccionadas son apropiadas.
- La condición de finalización es segura.
- El plan no amplía autoridad.

## Security Responsibilities

- Validar objetivos.
- Limitar cantidad y tipo de pasos.
- Verificar que cada subtask preserve scope.
- Requerir aprobación para cambios relevantes.
- Evitar self-expansion ilimitada.
- Impedir creación arbitraria de agentes o capabilities.
- Aplicar límites de costo.
- Registrar cambios de plan.

## Common Failure Modes

- Goal drift.
- Agent Goal Hijacking.
- Plan manipulation.
- Scope expansion.
- Subtask injection.
- Plan loops.
- Selección de acciones irreversibles.
- Omisión de controles.
- Delegación insegura.
- Finalización incorrecta.

---

# 8. Orchestrator

## Purpose

El **Orchestrator** coordina agentes, tools y workflows.

Puede decidir:

- Qué agente interviene.
- Cuándo se produce un handoff.
- Qué tareas se ejecutan en paralelo.
- Cómo se combinan resultados.
- Cómo se manejan dependencias.
- Qué ocurre ante un error.

```text
                    Orchestrator
                         │
          ┌──────────────┼──────────────┐
          │              │              │
          ▼              ▼              ▼
     Research Agent  Analysis Agent  Action Agent
```

## Assets

- Agent registry.
- Routing policies.
- Handoff state.
- Delegation grants.
- Shared context.
- Aggregated outputs.
- Execution graph.
- Priority rules.

## Trust Assumptions

- Los agentes registrados son legítimos.
- Cada agente cumple su función.
- Los handoffs preservan restricciones.
- Los resultados pueden combinarse.
- La identidad del originador se mantiene.
- La autoridad no aumenta.
- Los agentes no pueden impersonar a otros.

## Security Responsibilities

- Autenticar agentes.
- Autorizar handoffs.
- Limitar delegation depth.
- Preservar identity chain.
- Aplicar separación de funciones.
- Validar resultados.
- Evitar circular delegation.
- Controlar parallel execution.
- Registrar decisiones de routing.
- Aislar agentes comprometidos.

## Common Failure Modes

- Agent impersonation.
- Insecure inter-agent communication.
- Delegation amplification.
- Circular workflows.
- Cascading failures.
- Shared-context poisoning.
- Privilege aggregation.
- Result substitution.
- Confused Deputy.
- Rogue Agent behavior.

---

# 9. State Store

## Purpose

El **State Store** conserva información necesaria para continuar una ejecución.

No debe confundirse automáticamente con memoria de largo plazo.

Puede almacenar:

- Current step.
- Plan status.
- Tool results.
- Approval status.
- Retry counters.
- Budget.
- Created resources.
- Pending actions.
- Error state.

## Assets

- Run state.
- Workflow identifiers.
- Checkpoints.
- Approval tokens.
- Transaction identifiers.
- Intermediate outputs.
- Execution limits.

## Trust Assumptions

- El estado corresponde al run correcto.
- No fue alterado.
- Las aprobaciones continúan vigentes.
- No se mezclan tenants.
- Los retries son idempotentes.
- Los checkpoints son consistentes.

## Security Responsibilities

- Aislar runs.
- Aplicar integrity protection.
- Controlar acceso.
- Usar concurrency controls.
- Validar reanudaciones.
- Expirar approvals.
- Asegurar idempotencia.
- Registrar cambios.
- Cifrar información sensible.
- Definir retención.

## Common Failure Modes

- State tampering.
- Replay.
- Cross-run contamination.
- Reuse of expired approval.
- Duplicate execution.
- Race conditions.
- Stale state.
- Incomplete rollback.
- Unauthorized resume.
- State desynchronization.

---

# 10. Memory

## Purpose

La **Memory** permite que el sistema conserve información más allá del paso actual o de una ejecución individual.

Puede adoptar diferentes formas:

- Short-term memory.
- Session memory.
- Long-term memory.
- Episodic memory.
- Semantic memory.
- User profile.
- Shared agent memory.

## Assets

- Preferencias.
- Historial.
- Hechos.
- Resúmenes.
- Decisiones.
- Experiencias previas.
- Tool results.
- User-specific data.
- Learned patterns.
- Provenance metadata.

## Trust Assumptions

- La información es correcta.
- Continúa vigente.
- Pertenece al principal correcto.
- Su origen es conocido.
- Fue autorizada para persistir.
- Puede utilizarse en el propósito actual.
- No fue contaminada.

## Security Responsibilities

- Autorizar lecturas y escrituras.
- Preservar provenance.
- Limitar persistencia.
- Aplicar tenant isolation.
- Soportar deletion.
- Controlar actualización.
- Separar hechos de instrucciones.
- Validar información antes de persistir.
- Proteger integridad.
- Auditar usos.

## Common Failure Modes

- Memory Poisoning.
- Cross-user leakage.
- Stale memory.
- Excessive retention.
- Context contamination.
- Unauthorized write.
- Loss of provenance.
- Sensitive data persistence.
- Incorrect summarization.
- Propagación de información manipulada.

> **La memoria transforma una influencia temporal en una condición persistente.**

---

# 11. Tool Registry

## Purpose

El **Tool Registry** mantiene el catálogo de capacidades disponibles.

Puede almacenar:

- Tool name.
- Description.
- Input schema.
- Output schema.
- Risk classification.
- Required permissions.
- Owner.
- Version.
- Endpoint.
- Approval requirements.

## Assets

- Tool definitions.
- Schemas.
- Descriptions.
- Risk metadata.
- Routing information.
- Capability mappings.
- Ownership information.

## Trust Assumptions

- Las tools son legítimas.
- Las descripciones son correctas.
- Los schemas reflejan la implementación.
- Las versiones fueron aprobadas.
- El catálogo no fue modificado.
- El agente no puede registrar herramientas arbitrariamente.

## Security Responsibilities

- Permitir únicamente tools aprobadas.
- Verificar integridad.
- Mantener ownership.
- Versionar.
- Clasificar riesgo.
- Definir autorización.
- Deshabilitar tools.
- Separar read y write capabilities.
- Revisar cambios.
- Evitar tool shadowing.

## Common Failure Modes

- Tool Poisoning.
- Malicious descriptions.
- Schema mismatch.
- Unauthorized registration.
- Typosquatting.
- Hidden side effects.
- Version substitution.
- Excessive capabilities.
- Tool shadowing.
- Dynamic discovery sin validación.

---

# 12. Tool Broker y Execution Layer

## Purpose

El **Tool Broker** se ubica entre el agente y las herramientas.

Su objetivo es evitar que el agente invoque directamente sistemas sensibles.

```text
Agent
  │
  ▼
Tool Broker
  │
  ├── Authenticate
  ├── Authorize
  ├── Validate
  ├── Limit
  ├── Execute
  └── Audit
          │
          ▼
       Tool
```

## Assets

- Tool credentials.
- Authorization policies.
- Action requests.
- Parameters.
- Results.
- Transaction limits.
- Approval evidence.
- Execution logs.

## Trust Assumptions

- La identidad fue preservada.
- La policy está actualizada.
- Los schemas son correctos.
- El tool endpoint es legítimo.
- La acción aprobada coincide con la solicitada.
- El resultado puede procesarse de manera segura.

## Security Responsibilities

- Autenticar al caller.
- Autorizar por tool, action y resource.
- Validar parámetros.
- Aplicar rate y cost limits.
- Requerir aprobación.
- Redactar secrets.
- Ejecutar con credenciales acotadas.
- Registrar acciones.
- Aplicar idempotency.
- Soportar revocación y disablement.

## Common Failure Modes

- Authorization bypass.
- Confused Deputy.
- Parameter injection.
- Tool misuse.
- Excessive privilege.
- Token passthrough.
- Approval mismatch.
- Replay.
- Destructive retries.
- Response poisoning.

> **El Tool Broker permite convertir una recomendación probabilística en una ejecución determinísticamente controlada.**

---

# 13. Native Tools y Functions

## Purpose

Las **Native Tools** o **Functions** representan operaciones expuestas directamente por la aplicación.

Ejemplos:

- Buscar información.
- Leer archivos.
- Crear tickets.
- Enviar mensajes.
- Modificar registros.
- Ejecutar código.
- Administrar infraestructura.

## Assets

- Business operations.
- Data.
- Credentials.
- Execution environment.
- Parameters.
- Results.
- Side effects.

## Trust Assumptions

- Los inputs cumplen el schema.
- El caller posee permisos.
- La operación se ejecuta una vez.
- El resultado corresponde a la solicitud.
- La tool respeta límites.
- Los errores no filtran datos.

## Security Responsibilities

- Validar inputs.
- Aplicar autorización.
- Restringir acciones.
- Evitar command injection.
- Implementar idempotencia.
- Minimizar output.
- Aplicar timeouts.
- Limitar recursos.
- Registrar invocaciones.
- Clasificar impacto.

## Common Failure Modes

- Tool misuse.
- Injection.
- Over-permissive APIs.
- Destructive action.
- Sensitive output.
- Unexpected side effects.
- Insecure defaults.
- Missing authorization.
- Insufficient tenant isolation.
- Error leakage.

---

# 14. MCP Host, Client y Server

## Purpose

El **Model Context Protocol (MCP)** proporciona una arquitectura estandarizada para conectar aplicaciones de IA con recursos, prompts y herramientas.

Su arquitectura diferencia:

- **Host:** aplicación que contiene y coordina la experiencia.
- **Client:** conexión protocolar mantenida por el host hacia un server.
- **Server:** componente que expone capabilities.
- **Resources:** datos o contexto.
- **Prompts:** templates o workflows.
- **Tools:** funciones ejecutables.

```text
MCP Host
   │
   ├── MCP Client A ─── MCP Server A
   │
   ├── MCP Client B ─── MCP Server B
   │
   └── MCP Client C ─── MCP Server C
```

## Assets

- Connection state.
- OAuth tokens.
- Server metadata.
- Tool definitions.
- Resources.
- Prompts.
- Capability negotiation.
- Session identifiers.
- User consent.
- Authorization context.

## Trust Assumptions

- El server es legítimo.
- Las capabilities anunciadas son honestas.
- Los tokens poseen audiencia correcta.
- El client aísla conexiones.
- El host controla consentimiento.
- El server aplica autorización.
- Los resultados no son instrucciones confiables.

## Security Responsibilities

### Host

- Controlar qué servers pueden conectarse.
- Mostrar consentimiento.
- Aislar clients.
- Preservar user control.
- Aplicar políticas locales.
- Gestionar credenciales.
- Registrar interacciones.

### Client

- Validar server identity.
- Gestionar sesión.
- Preservar authorization context.
- Validar protocol messages.
- No reutilizar tokens indebidamente.
- Aplicar límites.

### Server

- Autenticar y autorizar.
- Validar audiencia.
- Rechazar token passthrough.
- Validar inputs.
- Aplicar rate limiting.
- Proteger recursos.
- Sanitizar outputs.
- Registrar accesos.

## Common Failure Modes

- Malicious MCP server.
- Tool Poisoning.
- Token passthrough.
- Audience confusion.
- Confused Deputy.
- Excessive capabilities.
- Server impersonation.
- Resource leakage.
- Prompt poisoning.
- Insecure dynamic discovery.
- Cross-client state leakage.

MCP estandariza la conexión.

No convierte automáticamente a un server o una tool en confiables.

---

# 15. RAG Pipeline

## Purpose

Un **Retrieval-Augmented Generation Pipeline** recupera información externa y la incorpora al contexto.

```text
Query
  │
  ▼
Retriever
  │
  ▼
Candidate Documents
  │
  ▼
Filtering and Ranking
  │
  ▼
Context Builder
  │
  ▼
Model
```

## Assets

- Source documents.
- Embeddings.
- Index.
- Query.
- Retrieval results.
- Access-control metadata.
- Relevance scores.
- Provenance.
- Tenant labels.

## Trust Assumptions

- Los documentos son legítimos.
- El índice está íntegro.
- La búsqueda respeta autorización.
- Los resultados pertenecen al tenant.
- La procedencia se conserva.
- La información continúa vigente.
- Los documentos no contienen instrucciones maliciosas.

## Security Responsibilities

- Autorizar antes de recuperar.
- Validar ingestion.
- Preservar metadata.
- Aplicar document-level access.
- Filtrar resultados.
- Limitar contenido.
- Detectar contenido sospechoso.
- Mantener provenance.
- Separar instrucciones y documentos.
- Registrar fuentes utilizadas.

## Common Failure Modes

- Retrieval poisoning.
- Index poisoning.
- Chunk Injection.
- Unauthorized retrieval.
- Cross-tenant leakage.
- Stale knowledge.
- Loss of provenance.
- Ranking manipulation.
- Sensitive data exposure.
- Indirect Prompt Injection.

---

# 16. Vector Store y Embedding Service

## Purpose

El **Vector Store** almacena representaciones utilizadas para búsqueda semántica.

El **Embedding Service** transforma contenido en vectores.

## Assets

- Embeddings.
- Document references.
- Metadata.
- Tenant IDs.
- Access labels.
- Query vectors.
- Index configuration.
- Source mappings.

## Trust Assumptions

- Los embeddings corresponden al contenido correcto.
- La metadata es íntegra.
- El índice está aislado.
- El servicio no retiene datos indebidamente.
- Las queries no exponen información.
- Los resultados respetan autorización.

## Security Responsibilities

- Aislar tenants.
- Aplicar access control.
- Proteger metadata.
- Validar ingestion.
- Gestionar retención.
- Cifrar datos.
- Auditar queries.
- Controlar proveedores.
- Permitir deletion.
- Mantener mapping hacia fuentes.

## Common Failure Modes

- Cross-tenant retrieval.
- Metadata tampering.
- Embedding inversion risk.
- Unauthorized indexing.
- Poisoned embeddings.
- Orphaned records.
- Data retention violations.
- Provider leakage.
- Inconsistent deletion.
- Index manipulation.

---

# 17. Identity Provider

## Purpose

El **Identity Provider (IdP)** autentica entidades y emite assertions o tokens.

Puede gestionar:

- Usuarios.
- Workloads.
- Agents.
- Services.
- Tools.
- Administrators.

## Assets

- Identities.
- Credentials.
- Tokens.
- Claims.
- Roles.
- Groups.
- Sessions.
- Authentication factors.
- Trust configuration.

## Trust Assumptions

- El principal fue autenticado.
- Los tokens son válidos.
- Las claims son correctas.
- La audiencia coincide.
- Las credenciales no fueron comprometidas.
- La identidad del agente es única.
- El lifecycle está gestionado.

## Security Responsibilities

- Strong authentication.
- Workload identity.
- Token scoping.
- Short-lived credentials.
- Audience restriction.
- Key rotation.
- Revocation.
- Agent inventory.
- Separation of human and non-human identities.
- Lifecycle management.

## Common Failure Modes

- Shared credentials.
- Long-lived tokens.
- Identity confusion.
- Token replay.
- Audience misuse.
- Impersonation.
- Orphaned agent identities.
- Excessive claims.
- Weak workload authentication.
- Failure to revoke.

---

# 18. Authorization Service y Policy Engine

## Purpose

El **Authorization Service** determina si una operación está permitida.

El **Policy Engine** evalúa reglas considerando:

- Principal.
- Agent.
- Action.
- Resource.
- Context.
- Purpose.
- Risk.
- Time.
- Approval.
- Delegation.

```text
Principal
   +
Agent
   +
Action
   +
Resource
   +
Context
     │
     ▼
Policy Decision
     │
     ├── Permit
     ├── Deny
     ├── Require Approval
     └── Permit with Constraints
```

## Assets

- Policies.
- Roles.
- Attributes.
- Entitlements.
- Delegation grants.
- Risk scores.
- Decision logs.
- Policy versions.

## Trust Assumptions

- La identidad es correcta.
- Los atributos son íntegros.
- La policy corresponde al entorno.
- El resource está identificado.
- La decisión se aplica.
- El agente no puede evadir el enforcement.
- La delegación continúa vigente.

## Security Responsibilities

- Deny by default.
- Autorizar por acción y recurso.
- Soportar constraints.
- Aplicar policy versioning.
- Registrar decisions.
- Invalidar grants.
- Preservar purpose.
- Evitar privilege amplification.
- Separar decisión y enforcement.
- Probar policies.

## Common Failure Modes

- Missing authorization.
- Policy bypass.
- Stale policy.
- Conflicting rules.
- Attribute manipulation.
- Delegation abuse.
- Excessive roles.
- Enforcement gap.
- Policy drift.
- Overreliance on model classification.

---

# 19. Policy Enforcement Point

## Purpose

El **Policy Enforcement Point (PEP)** aplica la decisión antes de que la acción llegue al recurso.

Puede ubicarse en:

- API Gateway.
- Tool Broker.
- MCP Server.
- Service mesh.
- Application backend.
- Database proxy.
- Workflow engine.

## Assets

- Action request.
- Authorization decision.
- Resource identifiers.
- Constraints.
- Session context.
- Enforcement logs.

## Trust Assumptions

- Toda acción atraviesa el PEP.
- La decisión no fue alterada.
- Los parámetros coinciden.
- No existe un camino alternativo.
- El PEP identifica correctamente el recurso.

## Security Responsibilities

- Bloquear operaciones no autorizadas.
- Aplicar constraints.
- Verificar freshness.
- Evitar TOCTOU.
- Registrar enforcement.
- Fail closed.
- Soportar revocación.
- Garantizar complete mediation.

## Common Failure Modes

- Bypass path.
- Fail-open behavior.
- Resource mismatch.
- Approval/action mismatch.
- Time-of-check to time-of-use.
- Missing mediation.
- Policy cache desactualizada.
- Partial enforcement.
- Parameter substitution.
- Inconsistent controls entre tools.

---

# 20. Human Approval Layer

## Purpose

La **Human Approval Layer** permite que una persona revise y autorice acciones sensibles.

No debería limitarse a presentar un botón genérico.

Debe permitir comprender:

- Qué se ejecutará.
- Sobre qué recurso.
- Bajo qué identidad.
- Con qué parámetros.
- Con qué impacto.
- Si es reversible.
- Qué información originó la decisión.

## Assets

- Approval request.
- Action summary.
- Exact parameters.
- Approver identity.
- Decision.
- Expiration.
- Scope.
- Evidence.
- Policy context.

## Trust Assumptions

- El approver posee autoridad.
- La información presentada es correcta.
- La acción no cambiará después.
- La aprobación no se reutilizará.
- El usuario comprende el impacto.
- No existe approval fatigue.

## Security Responsibilities

- Mostrar datos exactos.
- Identificar recurso y acción.
- Explicar impacto.
- Usar approvals específicas.
- Expirar approvals.
- Vincular aprobación con payload.
- Verificar approver authority.
- Registrar decisión.
- Permitir rechazo y escalamiento.
- Evitar self-approval cuando corresponda.

## Common Failure Modes

- Rubber stamping.
- Approval fatigue.
- Misleading summary.
- Payload substitution.
- Overbroad approval.
- Permanent consent.
- Reuse.
- Unauthorized approver.
- Approval generated by compromised model.
- Missing context.

---

# 21. Sandbox y Execution Environment

## Purpose

El **Sandbox** limita el impacto de código, comandos, navegación o procesamiento no confiable.

Puede restringir:

- Filesystem.
- Network.
- CPU.
- Memory.
- Execution time.
- Processes.
- Credentials.
- Devices.
- System calls.

## Assets

- Host environment.
- Files.
- Secrets.
- Network access.
- Workload identity.
- Execution artifacts.
- Temporary data.
- Outputs.

## Trust Assumptions

- El aislamiento es efectivo.
- El sandbox no posee secrets innecesarios.
- El escape no es trivial.
- Los recursos están limitados.
- Las imágenes son confiables.
- Los outputs se validan.

## Security Responsibilities

- Aplicar isolation.
- Usar ephemeral environments.
- Restringir network egress.
- Minimizar mounts.
- Aplicar resource quotas.
- Eliminar secrets.
- Validar artifacts.
- Destruir entornos.
- Mantener imágenes.
- Registrar actividad.

## Common Failure Modes

- Sandbox escape.
- Secret exposure.
- Unrestricted egress.
- Persistent compromise.
- Resource exhaustion.
- Malicious artifact.
- Shared filesystem contamination.
- Weak isolation.
- Vulnerable base image.
- Insecure cleanup.

---

# 22. External APIs y Enterprise Systems

## Purpose

Representan los sistemas sobre los que el agente consulta información o ejecuta acciones.

Ejemplos:

- CRM.
- ERP.
- Email.
- Calendar.
- Ticketing.
- Databases.
- Cloud platforms.
- Source control.
- Payment systems.
- Identity platforms.

## Assets

- Business data.
- Transactions.
- Configuration.
- Users.
- Permissions.
- Infrastructure.
- Communications.
- Audit records.

## Trust Assumptions

- La API está disponible.
- La identidad es válida.
- El contrato no cambió.
- El sistema aplica autorización.
- Los resultados son confiables.
- Los errores son manejables.
- Las operaciones son idempotentes.

## Security Responsibilities

- Aplicar autorización propia.
- No confiar únicamente en el agente.
- Validar inputs.
- Minimizar outputs.
- Implementar rate limits.
- Proteger secrets.
- Soportar rollback.
- Registrar acciones.
- Aplicar tenant isolation.
- Alertar sobre uso anómalo.

## Common Failure Modes

- API abuse.
- Broken Access Control.
- Excessive data exposure.
- Mass assignment.
- Business logic abuse.
- Credential compromise.
- Unexpected side effects.
- Inconsistent retries.
- Third-party compromise.
- Contract drift.

---

# 23. Other Agents y Multi-Agent Communication

## Purpose

Otros agentes pueden:

- Investigar.
- Analizar.
- Validar.
- Ejecutar.
- Supervisar.
- Coordinar.
- Delegar.

```text
Agent A
   │
   ▼
Message / Handoff
   │
   ▼
Agent B
```

## Assets

- Agent identity.
- Messages.
- Handoff context.
- Delegated authority.
- Shared memory.
- Results.
- Coordination state.
- Trust relationships.

## Trust Assumptions

- El agente receptor es legítimo.
- El mensaje conserva integridad.
- El scope es correcto.
- El agente no está comprometido.
- La autoridad puede delegarse.
- Los resultados son confiables.
- No existe impersonation.

## Security Responsibilities

- Authenticate agents.
- Authorize handoffs.
- Sign or protect messages.
- Preserve provenance.
- Limit delegation.
- Isolate memory.
- Validate results.
- Prevent circular delegation.
- Attribute actions.
- Support containment.

## Common Failure Modes

- Agent impersonation.
- Insecure inter-agent communication.
- Delegation abuse.
- Shared memory poisoning.
- Rogue Agent.
- Cascading failures.
- Trust exploitation.
- Message tampering.
- Privilege aggregation.
- Accountability gaps.

---

# 24. Observability, Tracing y Audit

## Purpose

La capa de observabilidad permite comprender qué ocurrió durante una ejecución.

Debe registrar más que errores y latencia.

```text
Goal
  │
  ▼
Context
  │
  ▼
Decision
  │
  ▼
Policy
  │
  ▼
Tool
  │
  ▼
Action
  │
  ▼
Result
```

## Assets

- Traces.
- Logs.
- Metrics.
- Tool calls.
- Policy decisions.
- Approval records.
- Model versions.
- Context references.
- Memory writes.
- Agent handoffs.

## Trust Assumptions

- Los logs son completos.
- Las timestamps son confiables.
- Los IDs correlacionan correctamente.
- La evidencia no fue alterada.
- Los datos sensibles están protegidos.
- Las decisiones pueden reconstruirse.

## Security Responsibilities

- Correlacionar runs.
- Registrar identity chain.
- Registrar model y tool versions.
- Mantener policy decisions.
- Proteger integridad.
- Redactar secrets.
- Aplicar retention.
- Detectar anomalías.
- Permitir investigación.
- Generar alertas.

## Common Failure Modes

- Missing traces.
- Sensitive data logging.
- Uncorrelated events.
- Mutable evidence.
- Excessive retention.
- Lack of identity.
- Missing policy context.
- Unobservable delegation.
- Inability to reconstruct actions.
- Logging controlled by compromised component.

> **Si una acción no puede reconstruirse, tampoco puede gobernarse ni investigarse adecuadamente.**

---

# 25. Safety, Guardrails y Content Controls

## Purpose

Los **Guardrails** y controles de contenido pueden evaluar:

- Entradas.
- Outputs.
- Topics.
- Policy violations.
- Data leakage.
- Harmful content.
- Known attack patterns.

## Assets

- Input.
- Output.
- Classification policies.
- Detection models.
- Thresholds.
- Blocklists.
- Review decisions.

## Trust Assumptions

- El detector identifica el patrón.
- El threshold es apropiado.
- El atacante no evade el control.
- La clasificación corresponde al contexto.
- El guardrail no es el único control.

## Security Responsibilities

- Aplicar controles en múltiples puntos.
- Validar inputs y outputs.
- Versionar policies.
- Medir false positives y negatives.
- Evitar dependencia exclusiva.
- Escalar acciones de riesgo.
- Mantener telemetría.
- Probar bypasses.

## Common Failure Modes

- Guardrail bypass.
- Encoding evasion.
- False negatives.
- False positives.
- Inconsistent enforcement.
- Overreliance.
- Context-unaware blocking.
- Policy drift.
- Missing coverage on tool outputs.
- Controls applied after execution.

Los guardrails pueden reducir ciertos riesgos.

No reemplazan la autorización, el aislamiento ni la seguridad de las herramientas.

---

# 26. Recovery y Control Plane

## Purpose

El **Recovery and Control Plane** permite administrar el sistema durante condiciones anómalas.

Puede incluir:

- Kill switches.
- Tool disablement.
- Credential revocation.
- Agent suspension.
- Policy rollback.
- Memory quarantine.
- Workflow cancellation.
- Safe mode.
- Incident flags.
- Recovery orchestration.

## Assets

- Control commands.
- Administrative identities.
- Revocation state.
- Agent inventory.
- Tool inventory.
- Policy versions.
- Incident data.
- Recovery procedures.

## Trust Assumptions

- El control plane permanece disponible.
- Los administradores están autenticados.
- La orden de contención se aplica.
- Los agentes no pueden evadirla.
- Las credenciales pueden revocarse.
- La evidencia se preserva.

## Security Responsibilities

- Separar control plane y data plane.
- Proteger administración.
- Probar kill switches.
- Mantener inventario.
- Permitir revocación granular.
- Soportar rollback.
- Preservar evidencia.
- Definir ownership.
- Ensayar respuesta.
- Evitar single points of failure.

## Common Failure Modes

- Kill switch inexistente.
- Tool que no puede deshabilitarse.
- Tokens no revocables.
- Dependencia del mismo sistema comprometido.
- Memory poisoning persistente.
- Falta de inventario.
- Recovery no probado.
- Privilegios administrativos excesivos.
- Evidence destruction.
- Inconsistent containment.

---

## Componentes y Trust Boundaries

Una arquitectura agéntica no debería documentar componentes sin representar también sus Trust Boundaries.

```text
[User Device]
      │
      │ Boundary: identity and session
      ▼
[Application Host]
      │
      │ Boundary: application to model provider
      ▼
[Model Endpoint]
      │
      │ Boundary: agent to external capability
      ▼
[Tool Broker]
      │
      │ Boundary: tool to business system
      ▼
[Enterprise API]
```

Una Trust Boundary puede aparecer cuando cambia:

- La identidad.
- La autoridad.
- El tenant.
- El propietario.
- El proveedor.
- La región.
- La clasificación.
- La persistencia.
- El propósito.
- El nivel de confianza.
- El dominio administrativo.

No debe limitarse al perímetro de red.

---

## Responsibility Matrix

Una arquitectura madura asigna responsabilidades explícitas.

| Componente | Responsabilidad principal |
|---|---|
| User Interface | Capturar intención y mostrar consecuencias |
| Agent Host | Gestionar sesiones, clients e integraciones |
| Agent Runtime | Controlar el loop y los límites |
| Model | Proponer contenido y decisiones |
| Context Builder | Seleccionar evidencia y preservar procedencia |
| Planner | Descomponer objetivos dentro del scope |
| Orchestrator | Coordinar agentes y handoffs |
| State Store | Preservar integridad de la ejecución |
| Memory | Mantener conocimiento autorizado y trazable |
| Tool Registry | Inventariar capabilities aprobadas |
| Tool Broker | Autorizar y controlar ejecuciones |
| Identity Provider | Autenticar humanos y workloads |
| Policy Engine | Tomar decisiones de autorización |
| PEP | Aplicar decisiones de política |
| Human Approval | Autorizar acciones sensibles de forma informada |
| Sandbox | Limitar el impacto de ejecución |
| Observability | Proporcionar trazabilidad y detección |
| Control Plane | Revocar, contener y recuperar |

Una responsabilidad no asignada termina dependiendo implícitamente del modelo o de supuestos no documentados.

---

## Component Inventory Template

Antes de realizar Threat Modeling, documentar cada componente.

```text
Component Name:

Component Type:

Business Purpose:

Owner:

Environment:

Tenant Scope:

Data Processed:

Identity Used:

Credentials:

Capabilities:

Permissions:

External Dependencies:

Trust Boundaries:

Inputs:

Outputs:

Persistent State:

Security Controls:

Logging:

Failure Modes:

Containment Mechanism:

Recovery Procedure:
```

Esta información permite identificar:

- Activos.
- Dependencias.
- Autoridad.
- Attack paths.
- Gaps de ownership.
- Single points of failure.
- Componentes no inventariados.
- Controles inexistentes.

---

## Component Interaction Matrix

También resulta útil documentar relaciones.

| Source | Destination | Data | Identity | Authority | Protocol | Trust Level |
|---|---|---|---|---|---|---|
| User | Agent Host | Goal | User | Request scope | HTTPS | Authenticated |
| Host | Model | Context | Workload | Inference only | API | Third party |
| Agent | Tool Broker | Proposed action | Agent | Scoped | Internal API | Controlled |
| Broker | CRM | Update | Workload | Record-level | REST | Enterprise |
| Agent | Memory | Fact | Agent | Write-limited | API | Persistent |
| MCP Client | MCP Server | Tool call | Delegated | Tool-specific | MCP | External |

La matriz muestra dónde deben aplicarse controles.

---

## Architecture Perspective

> **La calidad de una arquitectura agéntica depende menos de cuántos componentes utiliza y más de qué responsabilidades mantiene fuera del modelo.**

Una arquitectura riesgosa:

```text
Model
  │
  ├── Selects any tool
  ├── Builds arbitrary parameters
  ├── Decides authorization
  ├── Executes with broad token
  ├── Writes permanent memory
  └── Determines whether action was safe
```

Una arquitectura más segura:

```text
Model
  │
  ▼
Proposed Action
  │
  ▼
Policy Engine
  │
  ▼
Tool Broker
  │
  ├── Validate schema
  ├── Check identity
  ├── Check resource
  ├── Apply limits
  ├── Require approval
  └── Execute with scoped credential
          │
          ▼
       Audited Result
```

La arquitectura más segura no intenta volver determinista al modelo.

Mantiene deterministas las fronteras que deciden:

- Qué puede ejecutarse.
- Quién puede ejecutarlo.
- Sobre qué recurso.
- Bajo qué límites.
- Con qué evidencia.
- Con qué posibilidad de revocación.

---

## Pensar como un Agentic Security Engineer

Durante una revisión arquitectónica, no alcanza con preguntar:

> ¿Qué modelo utilizan?

También resulta necesario preguntar:

### Sobre el host

- ¿Qué componentes inicializa?
- ¿Qué servers, plugins o tools puede cargar?
- ¿Cómo se aíslan usuarios y tenants?
- ¿Quién administra la configuración?

### Sobre el runtime

- ¿Cómo se controlan los loops?
- ¿Qué ocurre ante errores?
- ¿Cómo se cancelan ejecuciones?
- ¿Cómo se reanudan de forma segura?

### Sobre el modelo

- ¿Qué datos recibe?
- ¿Qué versión se utiliza?
- ¿Qué decisiones puede influir?
- ¿Qué cambios requieren reevaluación?

### Sobre el contexto

- ¿Qué fuentes ingresan?
- ¿Qué nivel de confianza posee cada una?
- ¿Cómo se preserva la procedencia?
- ¿Qué ocurre cuando el contexto se trunca?

### Sobre herramientas

- ¿Quién las registra?
- ¿Qué permisos requieren?
- ¿Quién autoriza cada invocación?
- ¿Qué impacto pueden producir?
- ¿Cómo se deshabilitan?

### Sobre memoria

- ¿Qué puede persistir?
- ¿Quién puede escribir?
- ¿Cómo se elimina?
- ¿Cómo se evita contaminación entre usuarios?

### Sobre identidad

- ¿El agente posee identidad propia?
- ¿Actúa como el usuario?
- ¿Cómo se delega autoridad?
- ¿Los tokens son acotados y revocables?

### Sobre políticas

- ¿Dónde se toma la decisión?
- ¿Dónde se aplica?
- ¿Existen caminos alternativos?
- ¿Qué ocurre si el servicio de políticas falla?

### Sobre aprobación humana

- ¿Qué ve el approver?
- ¿La aprobación corresponde al payload exacto?
- ¿Cuándo expira?
- ¿Puede reutilizarse?

### Sobre observabilidad

- ¿Puede reconstruirse la ejecución?
- ¿Se conoce qué modelo, contexto y política intervinieron?
- ¿Los logs contienen datos sensibles?
- ¿La evidencia es íntegra?

### Sobre recuperación

- ¿Puede detenerse un agente?
- ¿Puede revocarse su identidad?
- ¿Puede deshabilitarse una tool?
- ¿Puede ponerse la memoria en cuarentena?
- ¿Puede revertirse la acción?

---

## Anti-patterns arquitectónicos

### The Omnipotent Agent

Un único agente posee acceso a todas las fuentes, herramientas y permisos.

Consecuencia:

- Blast Radius elevado.
- Dificultad de atribución.
- Escalamiento sencillo.
- Contención compleja.

---

### The Model as Policy Engine

El propio modelo decide si una acción está autorizada.

Consecuencia:

- Decisión probabilística.
- Bypass mediante contexto.
- Falta de complete mediation.
- Políticas difíciles de auditar.

---

### The Shared Super Token

Todos los agentes y herramientas utilizan la misma credencial privilegiada.

Consecuencia:

- Pérdida de identidad.
- Imposibilidad de atribución.
- Privilegios excesivos.
- Revocación disruptiva.

---

### The Context Dump

Toda la información disponible se envía al modelo.

Consecuencia:

- Data leakage.
- Cross-tenant exposure.
- Prompt Injection.
- Pérdida de minimización.
- Costos innecesarios.

---

### The Permanent Memory Sink

Todo output considerado útil se persiste automáticamente.

Consecuencia:

- Memory Poisoning.
- Retención excesiva.
- Información obsoleta.
- Influencia futura no controlada.

---

### Direct-to-Tool Execution

El modelo invoca directamente APIs sensibles.

Consecuencia:

- Ausencia de policy enforcement independiente.
- Parámetros no validados.
- Uso de credenciales amplias.
- Menor trazabilidad.

---

### Approval Theater

Existe un botón de aprobación, pero no muestra la acción real.

Consecuencia:

- Consentimiento no informado.
- Approval fatigue.
- Reutilización indebida.
- Falsa sensación de seguridad.

---

### Invisible Agent Handoffs

Los agentes se delegan tareas sin registrar identidad, scope o restricciones.

Consecuencia:

- Authority amplification.
- Accountability gaps.
- Agent impersonation.
- Cascading failures.

---

### Logging Without Traceability

Se registran prompts y errores, pero no decisiones, policies ni acciones.

Consecuencia:

- Investigación incompleta.
- Imposibilidad de reconstrucción.
- Métricas insuficientes.
- Falta de accountability.

---

### Recovery as an Afterthought

El sistema se despliega sin kill switch, revocación ni rollback.

Consecuencia:

- Contención lenta.
- Persistencia del incidente.
- Mayor impacto.
- Dependencia de acciones manuales improvisadas.

---

## Principios de diseño derivados

De la descomposición anterior surgen varios principios.

1. **El modelo no debe concentrar responsabilidades de seguridad.**
2. **Cada tool debe poseer autorización independiente.**
3. **Toda identidad debe ser explícita y atribuible.**
4. **La autoridad debe disminuir al atravesar delegaciones.**
5. **El contexto debe conservar procedencia y clasificación.**
6. **La memoria debe tratarse como un activo persistente.**
7. **Los loops deben tener límites verificables.**
8. **Las acciones sensibles deben pasar por un PEP.**
9. **Las approvals deben vincularse al payload exacto.**
10. **Las integraciones externas deben considerarse Trust Boundaries.**
11. **Los componentes deben poder deshabilitarse de manera granular.**
12. **La trazabilidad debe cubrir objetivo, decisión, autorización y acción.**
13. **La arquitectura debe limitar Blast Radius.**
14. **Las operaciones deberían ser reversibles siempre que sea posible.**
15. **La seguridad debe evaluarse sobre las interacciones, no solo sobre componentes individuales.**

---

## Relación con el resto de Fundamentals

Este capítulo proporciona el mapa arquitectónico utilizado por los próximos documentos.

```text
Agentic System Components
        │
        ├── Autonomy, Agency and Delegation
        ├── Agentic Assets
        ├── Agentic Trust Boundaries
        ├── Agentic Threats and Risk
        ├── Agentic Security Objectives
        ├── Safety, Security and Trustworthiness
        ├── Agentic Security Controls
        └── Agentic Security Engineering Principles
```

Los componentes aquí introducidos serán profundizados en secciones especializadas:

- MCP Host, Client y Server en `08-MCP-Security`.
- Memory en `09-Memory-and-Context-Security`.
- RAG y Vector Stores en `10-RAG-and-Data-Security`.
- Agent identity en `06-Identity-and-Authorization`.
- Tools y Tool Brokers en `07-Tool-and-Action-Security`.
- Multi-agent orchestration en `11-Multi-Agent-Security`.
- Policy Enforcement en `13-Security-Controls`.
- Sandboxing y approvals en `14-Secure-Design-Patterns`.
- Tracing en `16-Observability-and-Detection`.
- Revocation y containment en `17-Incident-Response`.

---

## Key Takeaways

- Un sistema agéntico es una arquitectura compuesta, no solamente un modelo.
- El Agent Host administra la experiencia, las conexiones y el contexto de ejecución.
- El Agent Runtime controla el loop, el estado, las tools y la finalización.
- El modelo propone decisiones, pero no debería aplicar políticas críticas.
- Las instrucciones expresan comportamiento esperado; no sustituyen controles deterministas.
- El Context Builder decide qué evidencia influye sobre el agente.
- El Planner puede ampliar el espacio de decisión y debe permanecer dentro del scope.
- El Orchestrator introduce relaciones de identidad, confianza y delegación.
- State y Memory son activos diferentes y requieren controles específicos.
- El Tool Registry inventaría capabilities; el Tool Broker controla su ejecución.
- Una tool disponible no está automáticamente autorizada.
- MCP define una arquitectura de conexión, no una garantía de confianza.
- RAG incorpora datos no confiables al contexto y requiere autorización y procedencia.
- El IdP autentica; el Policy Engine decide; el PEP aplica.
- Human-in-the-Loop solo aporta seguridad cuando la aprobación es informada y específica.
- La observabilidad debe permitir reconstruir objetivo, identidad, decisión, política y acción.
- Recovery, revocation y containment deben formar parte del diseño inicial.
- Las Trust Boundaries aparecen cuando cambia la identidad, autoridad, propiedad o control.
- El mayor riesgo suele emerger de la interacción entre componentes.
- La responsabilidad de seguridad debe asignarse explícitamente y no quedar implícitamente delegada al modelo.

---

## Referencias

Este capítulo sintetiza conceptos arquitectónicos y de seguridad publicados por fuentes oficiales y organizaciones reconocidas. Las responsabilidades y clasificaciones presentadas constituyen un modelo operativo para este repositorio.

### NIST

- [AI Agent Standards Initiative](https://www.nist.gov/artificial-intelligence/ai-agent-standards-initiative)
- [Software and AI Agent Identity and Authorization](https://www.nccoe.nist.gov/projects/software-and-ai-agent-identity-and-authorization)
- [NIST Artificial Intelligence Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST SP 800-160 Vol. 1 Rev. 1 — Systems Security Engineering](https://csrc.nist.gov/pubs/sp/800/160/v1/r1/final)
- [NIST SP 800-218 — Secure Software Development Framework](https://csrc.nist.gov/pubs/sp/800/218/final)

### OWASP GenAI Security Project

- [OWASP Agentic Security Initiative](https://genai.owasp.org/initiatives/agentic-security-initiative/)
- [Agentic AI — Threats and Mitigations](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)
- [Securing Agentic Applications Guide 1.0](https://genai.owasp.org/resource/securing-agentic-applications-guide-1-0/)
- [Multi-Agentic System Threat Modeling Guide](https://genai.owasp.org/resource/multi-agentic-system-threat-modeling-guide-v1-0/)
- [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)

### Model Context Protocol

- [MCP Architecture Overview](https://modelcontextprotocol.io/docs/learn/architecture)
- [MCP Specification](https://modelcontextprotocol.io/specification/)
- [MCP Tools](https://modelcontextprotocol.io/specification/2025-11-25/server/tools)
- [Understanding Authorization in MCP](https://modelcontextprotocol.io/docs/tutorials/security/authorization)
- [MCP Security Best Practices](https://modelcontextprotocol.io/docs/tutorials/security/security_best_practices)

### OpenAI

- [Building Agents](https://developers.openai.com/tracks/building-agents)
- [Agents SDK](https://developers.openai.com/api/docs/guides/agents)
- [A Practical Guide to Building AI Agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/)

Las referencias específicas sobre identidad, MCP, memoria, RAG, herramientas y sistemas multiagente serán ampliadas en sus respectivos documentos.

---

> **La seguridad de un sistema agéntico no depende únicamente de la calidad del modelo.**
>
> **Depende de qué componente construye el contexto, quién controla la autoridad, dónde se aplican las políticas, cómo se ejecutan las herramientas y qué mecanismos permiten observar, contener y recuperar el sistema cuando algo falla.**
