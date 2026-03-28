# Diseño de la Arquitectura

> **Unidad:** Tema 8
> 
> 
> **Palabras clave:** `arquitectura del software`, `estilos arquitectónicos`, `géneros arquitectónicos`, `arquetipos`, `mapeo DFD`, `evaluación de arquitectura`, `diseño arquitectónico`
> 

---

## 🎯 Objetivo de la clase

Entender qué es la arquitectura del software, cómo se clasifica (géneros y estilos), cómo se diseña en su contexto y cómo se evalúan las alternativas arquitectónicas. La arquitectura es el puente entre los requisitos y los componentes concretos del sistema.

---

## 🧠 Conceptos principales

### Arquitectura del Software

Proporciona una **visión estructural** del sistema a construir: cómo se organizan los componentes, cómo interactúan entre sí y qué estructuras de datos utilizan. Es la decisión de diseño más importante, ya que condiciona todo lo demás.

**Para qué sirve:**

- Analizar la efectividad del diseño para cumplir los requisitos.
- Considerar alternativas antes de construir.
- Reducir los riesgos de construcción.
- Facilitar la comunicación entre todos los participantes del proyecto.

> 💡 **Intuición:** La arquitectura de software es como los planos estructurales de un edificio: antes de poner un solo ladrillo, se define dónde van las columnas, cómo se distribuyen las cargas, qué salas se comunican con cuáles. Cambiar esto después es caro; mejor decidirlo bien al principio.
> 

---

### Género Arquitectónico

Dicta el **enfoque específico** para el tipo de software a construir. Cada dominio de aplicación tiene sus propios géneros con características y restricciones propias.

Ejemplos de géneros: inteligencia artificial, comunicaciones, contenido de autor, entretenimiento, financiero, medicina, militar, sistemas operativos, utilidades…

> 💡 **Intuición:** El género es como la "categoría" del proyecto. Un software médico tiene requisitos de fiabilidad y seguridad muy distintos a un videojuego de entretenimiento. El género orienta las decisiones de diseño desde el principio.
> 

---

### Estilos Arquitectónicos

Son patrones estructurales recurrentes y probados. La mayoría de los sistemas encajan en uno o varios de estos estilos:

| Estilo | Descripción | Ejemplo |
| --- | --- | --- |
| **Centrado en datos** | Los datos son el centro; los componentes acceden a un repositorio compartido | Bases de datos, data warehouses |
| **Flujo de datos** | Los datos de entrada se transforman en salida mediante componentes en secuencia | Compiladores, pipelines ETL |
| **Llamada y retorno** | Programa principal llama a subprogramas (o a procedimientos remotos) | Aplicaciones clásicas, RPC |
| **Orientado a objetos** | Los componentes encapsulan datos y operaciones; se comunican mediante mensajes | Aplicaciones OOP modernas |
| **En capas** | El sistema se organiza en capas; cada capa solo interactúa con la adyacente | Aplicaciones web (UI/Lógica/BD) |

---

### Arquetipo

Clase o patrón que representa una **abstracción de importancia crítica** dentro de la arquitectura. Los arquetipos capturan las entidades esenciales del dominio del sistema antes de descender al detalle de los componentes.

---

### Evaluación de Diseños Alternativos

El diseño produce varias alternativas arquitectónicas. Para seleccionar la mejor se usan dos técnicas:

**1. Método de la Negociación (ATAM — Architecture Tradeoff Analysis Method):**

Proceso iterativo de 6 pasos para evaluar alternativas según atributos de calidad.

**2. Complejidad Arquitectónica:**

Evalúa la complejidad midiendo las **dependencias** entre componentes (de flujo de información o de control). Menos dependencias → arquitectura más simple y mantenible.

---

### Mapeo DFD → Arquitectura

El diseño estructurado propone transformar el DFD del análisis directamente en la arquitectura del software mediante 6 pasos.

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Arquitectura del software** | Visión de la estructura y organización de los componentes del sistema, sus propiedades y sus conexiones |
| **Género arquitectónico** | Enfoque específico condicionado por el dominio de aplicación del software (medicina, finanzas, IA…) |
| **Estilo arquitectónico** | Patrón estructural recurrente que describe cómo se organizan e interactúan los componentes |
| **Arquetipo** | Clase o patrón que representa una abstracción crítica en la arquitectura del sistema |
| **Complejidad arquitectónica** | Medida de la complejidad de una arquitectura basada en las dependencias entre sus componentes |
| **Mapeo arquitectónico** | Proceso de transformar un DFD en una estructura de software con jerarquía de componentes |

---

## 📊 Diagramas

### Mapa del Tema 8

```
                  DISEÑO DE LA ARQUITECTURA
  ┌─────────────────────┬──────────────────────────────────┐
  │  DEFINICIÓN DE LA   │      DISEÑO DE LA ARQUITECTURA   │  EVALUACIÓN
  │   ARQUITECTURA      │                                  │
  ├──────────┬──────────┼────────────┬──────────┬──────────┤
  │  Género  │  Estilo  │ Arquetipos │  Comps.  │ Mapeo    │
  │          │          │  Contexto  │          │ con DFD  │
  └──────────┴──────────┴────────────┴──────────┴──────────┘
```

> 📝 **Lectura:** El tema se divide en tres bloques: primero *qué es* la arquitectura (género y estilo), luego *cómo se diseña* (contexto, arquetipos, componentes, mapeo DFD), y finalmente *cómo se evalúa*.
> 

---

### Los 5 Estilos Arquitectónicos

```
  ┌──────────────────────────────────────────────────────────────┐
  │                  ESTILOS ARQUITECTÓNICOS                     │
  ├───────────────────────────────────────────────────────────── ┤
  │                                                              │
  │  1. CENTRADO EN DATOS                                        │
  │     ┌───────┐        ┌──────────────────┐     ┌───────┐     │
  │     │ Comp A│ ──────▶│   Repositorio    │◀─── │ Comp B│     │
  │     └───────┘        │  (BD / fichero)  │     └───────┘     │
  │                      └──────────────────┘                   │
  │                                                              │
  ├──────────────────────────────────────────────────────────────┤
  │  2. FLUJO DE DATOS (pipeline)                                │
  │     Entrada ──▶ [Filtro A] ──▶ [Filtro B] ──▶ [Filtro C] ──▶ Salida│
  │     (cada filtro transforma y pasa al siguiente)             │
  ├──────────────────────────────────────────────────────────────┤
  │  3. LLAMADA Y RETORNO                                        │
  │         ┌───────────────────┐                               │
  │         │ Programa Principal│                               │
  │         └──┬────────┬───────┘                               │
  │            │        │   llama y espera respuesta            │
  │       ┌────▼──┐  ┌──▼────┐                                  │
  │       │ Sub A │  │ Sub B │  (o llamada remota RPC)          │
  │       └───────┘  └───────┘                                  │
  ├──────────────────────────────────────────────────────────────┤
  │  4. ORIENTADA A OBJETOS                                      │
  │    ┌──────────┐  mensaje   ┌──────────┐  mensaje ┌────────┐ │
  │    │ Objeto A │ ──────────▶│ Objeto B │─────────▶│Objeto C│ │
  │    └──────────┘            └──────────┘          └────────┘ │
  ├──────────────────────────────────────────────────────────────┤
  │  5. EN CAPAS                                                 │
  │    ┌────────────────────────┐  ← Capa de Presentación (UI)  │
  │    ├────────────────────────┤  ← Capa de Lógica de Negocio  │
  │    ├────────────────────────┤  ← Capa de Acceso a Datos     │
  │    └────────────────────────┘  ← Base de Datos              │
  │    Cada capa solo habla con la capa adyacente                │
  └──────────────────────────────────────────────────────────────┘
```

> 📝 **Lectura:** Cada estilo impone restricciones sobre cómo se comunican los componentes. Elegir el estilo correcto depende del tipo de problema: si el problema es transformar datos en secuencia → flujo de datos; si hay un repositorio central → centrado en datos; si se modela el dominio con objetos → OO; etc.
> 

---

### Contexto del Sistema en su Entorno

```
               ┌──────────────────────────────────┐
               │       SISTEMAS SUPERIORES         │
               │  (los que usan a nuestro sistema) │
               └────────────┬─────────────────────┘
                            │ «usados por»
               ┌────────────▼─────────────────────┐
  ┌─────────┐  │                                   │  ┌─────────┐
  │ ACTORES │──│       SISTEMA OBJETIVO            │──│  PARES  │
  │(usuarios│  │   (el software que diseñamos)     │  │(sistemas│
  │externos)│  │                                   │  │ iguales)│
  └─────────┘  └────────────┬─────────────────────┘
                            │ «depende de»
               ┌────────────▼─────────────────────┐
               │       SISTEMAS SUBORDINADOS        │
               │  (los que nuestro sistema usa)    │
               └──────────────────────────────────┘

  Cada flecha de comunicación = una INTERFAZ que hay que diseñar
```

> 📝 **Lectura:** Antes de diseñar los componentes internos, el arquitecto debe mapear el contexto: con qué actores interactúa el sistema, qué sistemas superiores lo usan, de qué sistemas subordinados depende y con qué pares se coordina. Cada conexión es una interfaz.
> 

---

### Arquetipos — Ejemplo CasaSegura

```
  ARQUETIPOS DEL SISTEMA CASASEGURA
  (abstracciones críticas antes de descender a componentes)

  ┌─────────────────┐
  │   Controlador   │  ← Gestiona y coordina los nodos del sistema
  └────────┬────────┘
           │ «se comunica con»
  ┌────────▼────────┐
  │      Nodo       │  ← Unidad de hardware/software que puede detectar o indicar
  └──────┬──────────┘
         │
    ┌────▼───┐   ┌────────────┐
    │Detector│   │ Indicador  │  ← Detector capta eventos; Indicador responde
    └────────┘   └────────────┘
```

> 📝 **Lectura:** Los arquetipos son las "clases esenciales" del dominio. En CasaSegura, cualquier elemento del sistema es un Nodo, que puede ser Detector (detecta eventos) o Indicador (emite respuestas). El Controlador orquesta todo. Estos cuatro arquetipos son el esqueleto conceptual de la arquitectura.
> 

---

### Refinamiento de Arquitectura → Componentes (CasaSegura)

```
  NIVEL ARQUITECTÓNICO              NIVEL DE COMPONENTES
  ─────────────────────             ──────────────────────────────────
                                    ┌─────────────────────┐
  ┌──────────────────┐              │  Ejecutivo CasaSegura│
  │  Sistema         │  refinar →  └──────────┬──────────┘
  │  CasaSegura      │                         │ Selección de función
  └──────────────────┘              ┌──────────┼──────────────────────┐
                                    │          │                      │
                              ┌─────▼───┐ ┌───▼──────┐ ┌────────────▼──┐
                              │Seguridad│ │Vigilancia│ │Administración │
                              └────┬────┘ └──────────┘ │  del hogar    │
                                   │                    └───────────────┘
                         ┌─────────┼──────────────┐
                    ┌────▼───┐ ┌───▼──────┐ ┌─────▼──────┐
                    │  GUI   │ │Interfaz  │ │Procesa     │
                    │        │ │internet  │ │panel ctrl  │
                    └────────┘ └──────────┘ └────────────┘
```

> 📝 **Lectura:** La arquitectura parte de un nivel alto (el sistema entero) y se refina hasta identificar los componentes concretos. El ejecutivo principal coordina subsistemas (Seguridad, Vigilancia…) que a su vez se descomponen en módulos (GUI, Interfaz de internet, etc.).
> 

---

### Método de Evaluación — ATAM (6 pasos)

```
  ┌─────────────────────────────────────────────────────────┐
  │  MÉTODO DE NEGOCIACIÓN PARA ANALIZAR LA ARQUITECTURA    │
  ├─────┬───────────────────────────────────────────────────┤
  │  1  │ Desarrollar los casos de uso del sistema          │
  ├─────┼───────────────────────────────────────────────────┤
  │  2  │ Obtener requisitos, restricciones y entorno       │
  ├─────┼───────────────────────────────────────────────────┤
  │  3  │ Describir estilos/patrones arquitectónicos        │
  ├─────┼───────────────────────────────────────────────────┤
  │  4  │ Evaluar cada atributo de calidad:                 │
  │     │  fiabilidad, seguridad, mantenibilidad,           │
  │     │  portabilidad, rendimiento…                       │
  ├─────┼───────────────────────────────────────────────────┤
  │  5  │ Identificar la criticidad de los atributos        │
  │     │ (¿qué es más importante para este sistema?)       │
  ├─────┼───────────────────────────────────────────────────┤
  │  6  │ Criticar y comparar las arquitecturas candidatas  │
  │     │ → seleccionar la más adecuada                     │
  └─────┴───────────────────────────────────────────────────┘
  Se repite iterativamente hasta obtener la arquitectura final
```

> 📝 **Lectura:** El método es iterativo: no termina en el paso 6, sino que se repite refinando la arquitectura seleccionada. La clave está en el paso 5: no todos los atributos pesan igual. Un sistema médico prioriza fiabilidad; uno bancario prioriza seguridad; uno de entretenimiento prioriza rendimiento.
> 

---

### Mapeo DFD → Arquitectura (6 pasos)

```
  DFD DEL ANÁLISIS                  ARQUITECTURA DE SOFTWARE
  ─────────────────                 ──────────────────────────
                                          ┌──────────┐
  ┌──────────────────┐                    │ Módulo   │
  │  Flujo de datos  │   6 pasos →       │ Raíz     │
  │  (Entrada-Proc   │                   └───┬───┬──┘
  │  -Salida)        │               ┌───────┘   └───────┐
  └──────────────────┘          ┌────▼────┐         ┌────▼────┐
                                │ Módulo  │         │ Módulo  │
                                │ Entrada │         │ Salida  │
                                └─────────┘         └─────────┘

  Paso 1: Tipo de flujo (transform flow / transaction flow)
  Paso 2: Fronteras del flujo (dónde empieza/termina cada tipo)
  Paso 3: Mapear DFD a estructura jerárquica de módulos
  Paso 4: Definir la jerarquía de control
  Paso 5: Refinar la estructura
  Paso 6: Elaborar la descripción arquitectónica
```

> 📝 **Lectura:** El mapeo DFD→Arquitectura es la técnica del diseño estructurado para obtener la arquitectura directamente del análisis. Cada proceso del DFD tiende a convertirse en un módulo. La jerarquía del DFD (niveles) se transforma en una jerarquía de llamadas entre módulos.
> 

---

## 🛠️ Procedimiento: Diseño Arquitectónico

**Idea general:** Partir del contexto del sistema y los requisitos para construir una representación estructural completa, refinándola hasta los componentes.

**Pasos:**

1. **Definir el contexto:** identificar actores, sistemas superiores, subordinados y pares. Definir las interfaces de comunicación.
2. **Identificar los arquetipos:** las abstracciones críticas del dominio.
3. **Seleccionar el estilo arquitectónico** más adecuado para el problema.
4. **Refinar hacia los componentes:** descomponer la arquitectura hasta identificar los módulos que la construirán.
5. **Evaluar alternativas** con el método ATAM o la complejidad arquitectónica.
6. **Elaborar la descripción arquitectónica** formal.

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **La arquitectura no es el código:** Es una representación abstracta de la estructura. Cambiarlo después de empezar a codificar es extremadamente costoso.
- ❗ **Género ≠ Estilo:** El género dice *para qué dominio* es el software (médico, financiero…); el estilo dice *cómo se estructuran* los componentes (capas, flujo de datos…). Un mismo género puede usar distintos estilos.
- ✅ **La complejidad arquitectónica se mide por dependencias:** Cuantas más dependencias entre componentes, más difícil de mantener y evolucionar. Reducir el acoplamiento es el objetivo.
- ✅ **El arquetipo es la clave conceptual:** Antes de pensar en componentes concretos, identificar los arquetipos del dominio ayuda a estructurar mejor la arquitectura.
- 🔁 **Conexión con Tema 7:** Los estilos arquitectónicos aplican directamente los conceptos de modularidad, ocultamiento de información y bajo acoplamiento vistos en el Tema 7. La arquitectura en capas es el ejemplo más claro: cada capa oculta su implementación a las demás.
- 🔁 **Conexión con Tema 6 (DFD):** El mapeo DFD→Arquitectura usa directamente el DFD construido en el análisis. El análisis estructurado y el diseño estructurado forman un flujo continuo.

---

## 🔗 Conexiones con otros temas

- **Tema 6 — DFD:** El DFD del análisis es la entrada del mapeo arquitectónico del apartado 8.7. Son el mismo sistema visto primero desde el análisis y luego desde el diseño.
- **Tema 7 — Conceptos de diseño:** La arquitectura aplica a gran escala los principios del Tema 7: modularidad (componentes), ocultamiento de información (interfaces), bajo acoplamiento (pocas dependencias entre componentes).
- **Temas siguientes — Componentes e Interfaz:** La arquitectura definida en este tema es el marco donde se ubicarán el diseño de componentes (módulos individuales) y el diseño de interfaz (UI/API).
- **Patrones de Diseño GoF:** Los estilos arquitectónicos son "meta-patrones". Los patrones GoF (Factory, Observer, Facade…) operan a nivel de componentes dentro de la arquitectura.

---

## 📝 Preguntas de repaso

1. ¿Para qué sirve la arquitectura del software? Enumera al menos tres utilidades.
2. ¿Qué diferencia hay entre un género arquitectónico y un estilo arquitectónico?
3. Describe brevemente los 5 estilos arquitectónicos. ¿Cuándo usarías cada uno?
4. ¿Qué es un arquetipo en el contexto del diseño arquitectónico? Da un ejemplo.
5. ¿Cómo se diseña un sistema en su contexto? ¿Qué tipos de sistemas externos hay que considerar?
6. ¿En qué consiste el método de negociación (ATAM) para evaluar arquitecturas? ¿Cuántos pasos tiene?
7. ¿Qué mide la complejidad arquitectónica?
8. ¿Cuáles son los 6 pasos para mapear un DFD a una arquitectura software?

---

## 📌 Resumen en una página

> La **arquitectura del software** es la visión estructural del sistema: define los componentes, cómo se organizan, cómo interactúan y qué datos manejan. Sirve para analizar alternativas, reducir riesgos y facilitar la comunicación del equipo antes de escribir código. El **género** determina el dominio del software (médico, financiero, IA…) y condiciona las prioridades de diseño. El **estilo arquitectónico** define el patrón estructural: centrado en datos, flujo de datos, llamada y retorno, orientado a objetos o en capas. El **diseño arquitectónico** comienza ubicando el sistema en su contexto (actores, sistemas superiores, subordinados y pares), identificando los **arquetipos** (abstracciones críticas del dominio) y refinando progresivamente hasta los componentes concretos. Para **evaluar** las alternativas se usa el método ATAM (6 pasos iterativos que ponderan atributos de calidad) o la **complejidad arquitectónica** (medida de dependencias entre componentes). Finalmente, el **mapeo DFD→Arquitectura** permite transformar el DFD del análisis directamente en una estructura jerárquica de módulos en 6 pasos.
>