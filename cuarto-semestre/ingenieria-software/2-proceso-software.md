# El Proceso del Software

> **Unidad:** Tema 2
> 
> 
> **Palabras clave:** `proceso`, `modelos prescriptivos`, `cascada`, `incremental`, `espiral`, `proceso unificado`, `agilidad`, `Scrum`, `sprint`, `manifiesto ágil`
> 

---

## 🎯 Objetivo de la clase

Conocer los distintos modelos de proceso software (prescriptivos, especializados y ágiles), entender sus diferencias, cuándo aplicar cada uno, y profundizar en Scrum como metodología ágil más extendida en la industria.

---

## 🧠 Conceptos principales

### 2.2 Un Modelo General de Proceso

El proceso de software se organiza jerárquicamente en tres niveles:

```
  Actividades estructurales  (5: comunicación, planeación, modelado, construcción, despliegue)
        │
        ▼
  Acciones  (grupos de trabajo dentro de cada actividad)
        │
        ▼
  Tareas  (unidades de trabajo concretas y medibles)
```

### Tipos de flujo de proceso

| Flujo | Descripción |
| --- | --- |
| **Lineal** | Las actividades se ejecutan una tras otra en secuencia |
| **Iterativo** | Se repite una o más actividades antes de pasar a la siguiente |
| **Evolutivo** | Las actividades se realizan en ciclos circulares, generando versiones cada vez más completas |
| **Paralelo** | Una o más actividades se ejecutan en paralelo con otras |

### Patrones de proceso

Son soluciones basadas en la experiencia a problemas recurrentes del proceso software. Facilitan al equipo la resolución de problemas conocidos cuando se presentan.

### Evaluación y mejora del proceso

El proceso software debe evaluarse para garantizar calidad, plazos y requisitos. Metodologías de evaluación:

- **SCAMPI** → basado en CMMI
- **CBA IPI** → basado en CMM
- **SPICE** → basado en ISO/IEC 15504
- **ISO 9001:2000**

---

### 2.3 Modelos de Proceso Prescriptivos

Son los modelos "clásicos" o tradicionales que definen con precisión las actividades, artefactos y roles del proceso.

### Modelo en Cascada (Ciclo de Vida Clásico)

Modelo secuencial estricto: cada actividad debe completarse **al 100%** antes de comenzar la siguiente.

```
  ┌─────────────┐
  │ Comunicación│
  └──────┬──────┘
         │
  ┌──────▼──────┐
  │  Planeación │
  └──────┬──────┘
         │
  ┌──────▼──────┐
  │  Modelado   │
  └──────┬──────┘
         │
  ┌──────▼──────┐
  │ Construcción│
  └──────┬──────┘
         │
  ┌──────▼──────┐
  │  Despliegue │
  └─────────────┘
```

> 📝 **Lectura:** Flujo estrictamente descendente. Una vez terminada una fase, no se vuelve atrás (o es muy costoso hacerlo).
> 

> ⚠️ **Problema principal:** Los requisitos raramente se conocen completamente al inicio. Los cambios tardíos son muy costosos.
> 

---

### Modelo Incremental

Aplica repetidas secuencias lineales de modo escalonado. Cada secuencia produce un **incremento** (versión parcial pero funcional) del producto.

```
  Incremento 1:  [Comunicación]─[Planeación]─[Modelado]─[Construcción]─[Despliegue] → v1.0
  Incremento 2:                 [Comunicación]─[Planeación]─[Modelado]─[Construcción]─[Despliegue] → v2.0
  Incremento 3:                               [Comunicación]─[Planeación]─[Modelado]─[Construcción]─[Despliegue] → v3.0
```

> 💡 **Intuición:** Como construir una casa por plantas — cada planta es funcional y habitable antes de añadir la siguiente.
> 

---

### Modelo Evolutivo

Modelo **iterativo** que produce versiones del software cada vez más completas en cada ciclo. Dos variantes principales:

**Modelo de Prototipos:**

- Se construye un prototipo rápido para obtener retroalimentación del cliente
- El prototipo puede desecharse o evolucionar hacia el producto final

**Modelo en Espiral:**

- Combina la naturaleza iterativa del prototipado con los aspectos controlados del modelo en cascada
- En cada vuelta de la espiral se realiza un **análisis de riesgo**

```
                    ↗ Planeación
  Análisis de riesgo         ↘
        ↑                   Ingeniería
        └──── Evaluación ◀──┘
              del cliente
```

---

### Modelo Concurrente

Permite representar elementos **concurrentes e iterativos**. Las actividades coexisten en diferentes estados simultáneamente; los eventos desencadenan transiciones entre estados.

> 💡 **Intuición:** En lugar de una cadena de montaje, es como un taller donde varias estaciones trabajan al mismo tiempo en distintas partes del producto.
> 

---

### 2.4 Modelos de Proceso Especializado

### Modelo Basado en Componentes

El software se construye ensamblando **componentes prefabricados** (fragmentos de software reutilizables):

1. Se identifican componentes candidatos
2. Se buscan en un repositorio
3. Se extraen y adaptan
4. Se integran y prueban

> 🔁 Fomenta la **reutilización del software**, reduciendo tiempo y coste de desarrollo.
> 

### Modelo de Métodos Formales

Usa **notación matemática estricta** para especificar, desarrollar y verificar el sistema. Garantiza corrección formal pero requiere conocimientos matemáticos avanzados. Se usa en sistemas críticos (aeronáutica, medicina, nuclear).

### Modelo Orientado a Aspectos

Define, especifica, diseña y construye **aspectos**: propiedades globales (funcionales y no funcionales) que afectan a múltiples componentes (ej: logging, seguridad, transacciones).

---

### 2.5 El Proceso Unificado (UP / RUP)

> **UML** es el lenguaje de modelado y desarrollo de software del paradigma orientado a objetos basado en una notación robusta. El Proceso Unificado lo usa como lenguaje estándar.
> 

El Proceso Unificado consta de **cinco fases**:

```
  ┌────────────┐   ┌─────────────┐   ┌──────────────┐   ┌────────────┐   ┌────────────┐
  │ CONCEPCIÓN │──▶│ ELABORACIÓN │──▶│ CONSTRUCCIÓN │──▶│ TRANSICIÓN │──▶│ PRODUCCIÓN │
  └────────────┘   └─────────────┘   └──────────────┘   └────────────┘   └────────────┘
```

| Fase | Objetivo |
| --- | --- |
| **Concepción** | Definir el alcance del proyecto, identificar los casos de uso principales |
| **Elaboración** | Analizar el dominio del problema, establecer la arquitectura base |
| **Construcción** | Desarrollar el producto completo de forma iterativa e incremental |
| **Transición** | Trasladar el sistema a los usuarios finales (beta, formación, ajustes) |
| **Producción** | Monitorizar y dar soporte al software en uso |

> 💡 **Intuición:** A diferencia del cascada, en el UP todas las disciplinas (requisitos, diseño, implementación, pruebas) se trabajan en **todas las fases**, variando solo la intensidad.
> 

---

### 2.6 Modelos del Proceso Personal y del Equipo

### PSP — Proceso Personal del Software

Se basa en la **medición personal** del producto generado y su calidad. Actividades estructurales:

1. Planeación
2. Diseño de alto nivel
3. Revisión del diseño
4. Desarrollo
5. Mediciones para la mejora del proceso

### TSP — Proceso del Equipo de Software

Objetivo: construir un equipo capaz de **autodirigirse** en el desarrollo de proyectos. Incorpora medición cuantitativa del proceso y el producto por parte del propio equipo.

---

### 2.7 ¿Qué es la Agilidad?

La agilidad surge del **Manifiesto por el Desarrollo Ágil** (2001), firmado por 17 expertos en desarrollo software.

### Los 4 valores del Manifiesto Ágil

```
  Individuos e interacciones   >   Procesos y herramientas
  Software funcionando         >   Documentación exhaustiva
  Colaboración con el cliente  >   Negociación de contratos
  Respuesta al cambio          >   Seguimiento de un plan
```

> ⚠️ Los elementos de la derecha tienen valor, pero los de la izquierda se valoran **más**.
> 

### Coste del cambio

El coste de incorporar un cambio crece exponencialmente cuanto más avanzado está el proyecto (rediseño + nueva codificación + pruebas de regresión...). Los **procesos ágiles bien diseñados reducen este coste**.

```
  Coste del cambio
       │
  alto │              ╱ Cascada
       │             ╱
       │           ╱
       │    ─────────── Ágil
  bajo │
       └─────────────────────────▶ Tiempo del proyecto
          Inicio              Fin
```

---

### 2.8 ¿Qué es un Proceso Ágil?

Un proceso ágil es de **adaptación incremental rápida** a cambios en las necesidades y condiciones técnicas, apoyándose en la **retroalimentación constante del cliente**.

### 12 Principios de Agilidad (Manifiesto Ágil)

Los más relevantes para el examen:

| # | Principio clave |
| --- | --- |
| 1 | Satisfacer al cliente con entrega temprana y continua de software con valor |
| 2 | **Bienvenidos los cambios** de requisitos, incluso en etapas tardías |
| 3 | Entregar software funcionando con **frecuencia** (semanas, no meses) |
| 4 | Desarrolladores y personas de negocio deben trabajar **juntos diariamente** |
| 5 | Construir proyectos en torno a **individuos motivados** con el entorno y apoyo que necesitan |
| 6 | La conversación **cara a cara** es el método más eficiente de comunicación |
| 7 | El **software funcionando** es la medida principal de progreso |
| 8 | Los procesos ágiles promueven el **desarrollo sostenible** |
| 9 | La atención continua a la excelencia técnica mejora la agilidad |
| 10 | La **simplicidad** es esencial |
| 11 | Los mejores diseños emergen de **equipos autoorganizados** |
| 12 | El equipo reflexiona periódicamente sobre cómo ser más efectivo (**retrospectiva**) |

### Características del equipo ágil

- Competentes técnicamente
- Enfoque común en el proyecto
- Colaboración interna y con todos los participantes
- Capacidad de autoorganización
- Habilidad para toma de decisiones y resolución de problemas
- Confianza y respeto mutuos

---

### 2.9 Scrum

**Scrum** es un método de desarrollo ágil ideado por **Jeff Sutherland** (años 90) y formalizado por **Schwaber y Beedle**.

> **Definición:** Establece pequeños ciclos de vida denominados **sprint**, para desarrollar un requisito y durante el cual **no se introducen cambios**.
> 

### Roles en Scrum

| Rol | Responsabilidad |
| --- | --- |
| **Product Owner** | Representa al cliente; prioriza el Product Backlog |
| **Scrum Master** | Facilita el proceso; elimina impedimentos del equipo |
| **Development Team** | Equipo autoorganizado que ejecuta el sprint |
| **Stakeholders** | Partes interesadas que evalúan el producto al final del sprint |

### Artefactos de Scrum

| Artefacto | Descripción |
| --- | --- |
| **Product Backlog** | Lista priorizada de todo lo que el producto necesita |
| **Sprint Backlog** | Subconjunto del Product Backlog seleccionado para el sprint actual |
| **Incremento** | Versión funcional y potencialmente entregable al final del sprint |

### Flujo de un Sprint en Scrum

```
  Product        Sprint          Sprint           Revisión        Retrospectiva
  Backlog ──▶   Planning  ──▶   (1-4 semanas) ──▶  del Sprint ──▶  del Sprint
     │                              │
     │            ┌─────────────────┘
     │            │  Daily Scrum (15 min/día)
     │            │  ¿Qué hice ayer?
     │            │  ¿Qué haré hoy?
     │            │  ¿Hay impedimentos?
     │            └─────────────────▶ Incremento funcional
     │
     └──── (siguiente sprint) ◀─────┘
```

> 📝 **Lectura:** El ciclo se repite sprint a sprint hasta completar el Product Backlog o decidir que el producto es suficiente. Cada sprint entrega valor real al cliente.
> 

### Eventos de Scrum

- **Sprint Planning:** El equipo selecciona del backlog lo que puede completar en el sprint
- **Daily Scrum:** Reunión diaria de **15 minutos** máximo: qué se hizo, qué se hará, qué bloquea
- **Sprint Review:** Al final del sprint, se muestra al cliente la nueva funcionalidad para evaluación
- **Sprint Retrospective:** El equipo reflexiona sobre el proceso para mejorar

---

### 2.10 Herramientas para el Proceso Ágil

Las herramientas ágiles incorporan soporte automatizado para:

- Planificación del proyecto
- Desarrollo de casos y obtención de requisitos
- Diseño rápido
- Generación de código
- Realización de pruebas

Ejemplos de herramientas: **OnTime** (Axosoft), **Ideogramic UML**, **Together Tool Set** (Borland). Actualmente también: Jira, Trello, Azure DevOps, Linear.

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Modelo de proceso** | Representación abstracta del proceso software que define actividades, artefactos y roles |
| **Sprint** | Ciclo corto de desarrollo (1-4 semanas) en Scrum durante el cual no se introducen cambios |
| **Product Backlog** | Lista priorizada de funcionalidades y requisitos pendientes de un producto |
| **Incremento** | Versión parcial pero funcional del software entregada al final de un ciclo |
| **Patrón de proceso** | Solución reutilizable basada en experiencia para problemas recurrentes del proceso |
| **Daily Scrum** | Reunión diaria de 15 minutos del equipo para sincronizarse y detectar impedimentos |

---

## 📊 Comparativa de Modelos de Proceso

```
  MODELO          FLUJO        CAMBIOS    ENTREGA      CUÁNDO USARLO
  ─────────────────────────────────────────────────────────────────────
  Cascada         Lineal       Costosos   Al final     Requisitos muy estables
  Incremental     Lineal       Moderados  Parciales    Entrega por etapas
  Prototipado     Evolutivo    Fáciles    Prototipo    Requisitos poco claros
  Espiral         Evolutivo    Moderados  Por ciclos   Proyectos con alto riesgo
  Componentes     Variable     Moderados  Por ciclos   Reutilización disponible
  Proceso Unif.   Iterativo    Moderados  Por fases    Proyectos OO medianos/grandes
  Scrum/Ágil      Iterativo    Bienvenidos  Cada sprint  Requisitos cambiantes
```

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ El modelo en **cascada** también se llama "ciclo de vida clásico" — son lo mismo en el examen.
- ❗ Los modelos evolutivos comunes son **prototipos y espiral**, no confundir con otros.
- ✅ El flujo **iterativo** repite actividades; el **paralelo** las ejecuta simultáneamente — son conceptos distintos.
- ✅ El Proceso Unificado tiene **5 fases**: Concepción, Elaboración, Construcción, Transición, Producción.
- ❗ En Scrum, durante un sprint **no se introducen cambios** de requisitos.
- ✅ Las **actividades sombrilla** del Tema 1 (gestión del riesgo, calidad, etc.) se aplican en **todos** los modelos.
- 🔁 El modelo concurrente combina flujo **evolutivo y paralelo** — pregunta frecuente de test.
- ❗ El desarrollo basado en componentes **fomenta la reutilización** pero sí requiere pruebas de integración.

---

## 🔗 Conexiones con otros temas

- **Tema 1 (Proceso del software):** Este tema profundiza en los modelos concretos que implementan ese proceso genérico.
- **UML:** El Proceso Unificado usa UML como lenguaje de modelado estándar en todas sus fases.
- **Requisitos (temas posteriores):** El Product Backlog de Scrum está formado por historias de usuario, que son una forma de captura de requisitos.

---

## 📝 Preguntas de repaso

1. ¿Cuáles son los cuatro tipos de flujo de proceso y en qué se diferencian?
2. ¿Qué modelo de proceso se conoce como "ciclo de vida clásico" y cuál es su principal limitación?
3. ¿Cuáles son las dos variantes del modelo evolutivo?
4. ¿Cuáles son las cinco fases del Proceso Unificado?
5. ¿Qué diferencia hay entre PSP y TSP?
6. ¿Cuáles son los 4 valores del Manifiesto Ágil?
7. ¿Qué es un sprint en Scrum y qué restricción fundamental tiene?
8. ¿Cuáles son los tres roles principales de Scrum y qué hace cada uno?
9. ¿Qué son los patrones de proceso y para qué sirven?
10. ¿Por qué el coste del cambio es menor en los modelos ágiles que en los tradicionales?

---

## 📌 Resumen en una página

El proceso software puede organizarse según distintos modelos. Los **modelos prescriptivos** (cascada, incremental, evolutivo, concurrente) son los clásicos, cada uno con un tipo de flujo diferente; el cascada es secuencial y rígido, el incremental entrega partes funcionales, el evolutivo genera versiones crecientes y el concurrente permite trabajo paralelo. Los **modelos especializados** (componentes, métodos formales, orientado a aspectos) abordan necesidades específicas. El **Proceso Unificado** organiza el desarrollo en cinco fases iterativas usando UML como lenguaje. Frente a estos, la **agilidad** (formalizada en el Manifiesto Ágil de 2001) propone priorizar individuos, software funcional, colaboración con el cliente y respuesta al cambio. **Scrum** es el método ágil más popular: organiza el trabajo en sprints cortos (1-4 semanas) sin cambios internos, con un Product Backlog priorizado, Daily Scrums de 15 minutos, y revisiones con el cliente al final de cada sprint para obtener retroalimentación continua.