# Conceptos de Diseño

> **Unidad:** Tema 7
> 
> 
> **Palabras clave:** `diseño de software`, `arquitectura`, `modularidad`, `abstracción`, `ocultamiento de información`, `independencia funcional`, `modelo del diseño`, `patrones de diseño`
> 

---

## 🎯 Objetivo de la clase

Entender qué es el diseño de software, cómo se ubica en el ciclo de vida del desarrollo y cuáles son los conceptos fundamentales que guían un buen diseño. El diseño responde a la pregunta **¿cómo se resuelve el problema?**, a diferencia del análisis que responde al *¿qué hay que hacer?*

---

## 🧠 Conceptos principales

### Diseño de Software en el Ciclo de Vida

El diseño ocupa la posición central entre el análisis y la construcción:

```
Análisis/Requisitos  →  Diseño  →  Construcción (código)
     ¿Qué hacer?       ¿Cómo?       Implementación
```

Se realiza a **cuatro niveles** que van de lo más abstracto a lo más concreto, y cada nivel del modelo de análisis alimenta un nivel del modelo de diseño.

> 💡 **Intuición:** Piensa en construir una casa. Los requisitos dicen "necesito 3 habitaciones y un baño". El diseño dice "aquí va la pared, de este material, con estas dimensiones". El código es cuando los albañiles construyen.
> 

---

### Los 9 Conceptos Clave del Diseño

### 1. Abstracción

Capacidad de enunciar una solución en términos generales (alto nivel) o detallados (bajo nivel). El diseño comienza con alta abstracción y se refina progresivamente.

> 💡 **Intuición:** "Vehículo de transporte" es alta abstracción. "Toyota Corolla 2024 motor 1.8L 4 cilindros" es baja abstracción.
> 

### 2. Arquitectura del Software

Estructura de los módulos del sistema, cómo interactúan entre ellos y qué estructuras de datos utilizan. Es el esqueleto del sistema.

### 3. Patrón de Diseño

Solución documentada a un problema particular de diseño en un contexto específico. Reutilizable y probado. Complementa los patrones de requisitos (Tema 6).

### 4. División de Problemas (*Divide y vencerás*)

Un problema complejo se subdivide en problemas más pequeños, cada uno más fácil de resolver de forma independiente.

> 💡 **Intuición:** Ordenar un mazo de 1000 cartas de golpe es difícil. Dividirlo en montones de 10, ordenar cada uno y luego combinarlos es mucho más manejable — eso es exactamente lo que hace MergeSort.
> 

### 5. Modularidad

División del software en **componentes** identificables por un nombre, que pueden desarrollarse y probarse de forma independiente. Cada módulo encapsula una responsabilidad.

### 6. Ocultamiento de Información (*Information Hiding*)

Los módulos deben diseñarse para que su información interna (datos y algoritmos) sea **inaccesible** a quienes no la necesiten. Solo se expone una interfaz pública mínima.

> 💡 **Intuición:** Cuando usas `list.sort()` en Python, no necesitas saber si usa Timsort internamente. La implementación está oculta; solo te importa el resultado.
> 

### 7. Independencia Funcional

Cada módulo debe resolver un **conjunto específico** de requisitos y ofrecer una interfaz sencilla. Se mide con dos métricas:

- **Cohesión:** grado en que las funciones de un módulo están relacionadas entre sí → alta cohesión es deseable.
- **Acoplamiento:** grado de interdependencia entre módulos → bajo acoplamiento es deseable.

### 8. Refinamiento Sucesivo

Proceso de incrementar el nivel de detalle del diseño a medida que se avanza. Se empieza con una descripción abstracta y se añaden detalles iterativamente.

### 9. Rediseño (*Refactoring*)

Reorganización del diseño de un componente para **simplificarlo** sin modificar su funcionalidad externa. Mejora la legibilidad, mantenibilidad y estructura interna.

> 💡 **Intuición:** Refactorizar es como reorganizar tu cuarto: no compras muebles nuevos ni cambias su función, solo los colocas mejor para que el espacio sea más funcional.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Diseño de software** | Proceso iterativo que interpreta los requisitos del sistema para responder a *¿cómo se resuelve el problema?* |
| **Abstracción** | Nivel de generalidad con que se describe una solución: alto nivel = términos generales; bajo nivel = detalles específicos |
| **Arquitectura del software** | Estructura de módulos del sistema, sus interacciones y las estructuras de datos que utilizan |
| **Patrón de diseño** | Estructura de diseño reutilizable que resuelve un problema particular en un contexto específico |
| **Modularidad** | División del software en componentes identificables e independientes |
| **Ocultamiento de información** | Principio por el cual los detalles internos de un módulo son inaccesibles desde el exterior |
| **Cohesión** | Grado en que las funciones dentro de un módulo están relacionadas. Alta cohesión → buen diseño |
| **Acoplamiento** | Grado de interdependencia entre módulos. Bajo acoplamiento → buen diseño |
| **Rediseño** | Reorganización del software para simplificar su diseño sin alterar su funcionalidad |
| **Refinamiento sucesivo** | Técnica que consiste en añadir progresivamente más detalle al diseño |

---

## 📊 Diagramas

### Posición del Diseño en el Ciclo de Vida

```
  ┌──────────────────────────────────────────────────────────────┐
  │                  CICLO DE VIDA DEL SOFTWARE                  │
  │                                                              │
  │  ANÁLISIS       →      DISEÑO       →     CONSTRUCCIÓN       │
  │                                                              │
  │  Modelo de            Modelo del          Código fuente      │
  │  Requisitos           Diseño                                 │
  │  ──────────           ────────────        ─────────────      │
  │  • Casos de uso  ──▶  • Arquitectura      • Implementación   │
  │  • Clases        ──▶  • Datos/Clases      • Pruebas          │
  │  • DFD           ──▶  • Componentes       • Integración      │
  │  • D. Estado     ──▶  • Interfaz                             │
  │  • D. Secuencia  ──▶  • Despliegue                          │
  │                                                              │
  │       ¿Qué?                ¿Cómo?              Hacerlo       │
  └──────────────────────────────────────────────────────────────┘
```

> 📝 **Lectura:** El modelo de análisis (Temas 5 y 6) es la entrada del diseño. Cada elemento del análisis se transforma en un elemento del modelo de diseño. El diseño es el puente entre los requisitos y el código.
> 

---

### Los 4 Niveles del Modelo de Diseño

```
                     MODELO DE ANÁLISIS
         ┌─────────────┬─────────────┬─────────────┐
         │  Escenario  │    Clases   │Comportamiento│
         └──────┬──────┴──────┬──────┴──────┬───────┘
                │             │             │
                ▼             ▼             ▼
  ┌─────────────────────────────────────────────────────┐
  │                  MODELO DEL DISEÑO                  │
  │                                                     │
  │  ┌───────────────┐  ← más abstracto                 │
  │  │ Diseño de     │  Estructura de clases y datos     │
  │  │ Datos/Clases  │                                   │
  │  └───────┬───────┘                                   │
  │          │                                           │
  │  ┌───────▼───────┐                                   │
  │  │  Diseño de la │  Componentes y sus relaciones     │
  │  │  Arquitectura │                                   │
  │  └───────┬───────┘                                   │
  │          │                                           │
  │  ┌───────▼───────┐                                   │
  │  │  Diseño de la │  Pantallas, flujos UI             │
  │  │    Interfaz   │                                   │
  │  └───────┬───────┘                                   │
  │          │                                           │
  │  ┌───────▼───────┐  ← más concreto                  │
  │  │  Diseño de    │  Módulos individuales (código)    │
  │  │  Componentes  │                                   │
  │  └───────────────┘                                   │
  │                                                     │
  │  + Diseño de Despliegue (entorno físico/servidores) │
  └─────────────────────────────────────────────────────┘
```

> 📝 **Lectura:** El diseño avanza de lo más abstracto (estructura de datos) a lo más concreto (módulos de código). Cada capa refina la anterior. El despliegue define en qué infraestructura física vivirá el sistema.
> 

---

### Cohesión vs. Acoplamiento

```
  COHESIÓN (dentro del módulo)         ACOPLAMIENTO (entre módulos)
  ─────────────────────────────        ──────────────────────────────

  ┌─────────────────┐                  ┌──────────┐     ┌──────────┐
  │    Módulo A     │                  │ Módulo A │────▶│ Módulo B │
  │  ┌───┐  ┌───┐  │                  └──────────┘     └──────────┘
  │  │f1 │  │f2 │  │  ← Alta cohesión:    Bajo acoplamiento: ↑ BIEN
  │  └───┘  └───┘  │     f1 y f2 hacen    solo una dependencia
  │  relacionadas  │     cosas relacionadas
  └─────────────────┘
                                       ┌──────────┐ ──▶ ┌──────────┐
  ┌─────────────────┐                  │ Módulo A │ ──▶ │ Módulo B │
  │    Módulo B     │                  └──────────┘ ──▶ └──────────┘
  │  ┌───┐  ┌───┐  │  ← Baja cohesión:    Alto acoplamiento: ↓ MAL
  │  │f1 │  │f2 │  │     f1 y f2 hacen    muchas dependencias entre
  │  └───┘  └───┘  │     cosas sin         módulos → cambiar uno
  │  no relacionadas│    relación          rompe el otro
  └─────────────────┘

  REGLA DE ORO: Alta Cohesión + Bajo Acoplamiento = Buen Diseño
```

> 📝 **Lectura:** Un módulo bien diseñado hace *una cosa y la hace bien* (alta cohesión) y *depende poco de otros* (bajo acoplamiento). Esto facilita el mantenimiento y las pruebas.
> 

---

### Ocultamiento de Información — Ejemplo

```
  SIN ocultamiento          CON ocultamiento (interfaz pública)
  ──────────────────        ───────────────────────────────────

  ┌──────────────────┐      ┌──────────────────────────────────┐
  │    BDManager     │      │          BDManager               │
  │                  │      │  ┌──────────────────────────┐    │
  │  host = "localhost"     │  │  - host: String  ← PRIVADO│    │
  │  port = 5432     │      │  │  - port: int     ← PRIVADO│    │
  │  query(...)      │      │  │  - connection    ← PRIVADO│    │
  │  connection      │      │  └──────────────────────────┘    │
  │  password ="123" │      │  + query(sql): ResultSet ← PÚBLICO│
  └──────────────────┘      │  + connect(): bool ← PÚBLICO     │
                            └──────────────────────────────────┘
  Todos pueden acceder      Solo se expone lo mínimo necesario.
  a datos sensibles         Los internos están protegidos.
```

> 📝 **Lectura:** El ocultamiento de información es el fundamento del encapsulamiento en POO. Solo los métodos necesarios son públicos; todo lo demás es privado o protegido.
> 

---

### Refinamiento Sucesivo — Ejemplo

```
  NIVEL ALTO (abstracto)            NIVEL BAJO (detallado)
  ────────────────────────          ─────────────────────────────────

  procesar_pago()                   procesar_pago(monto: float, tarjeta: str):
      │                                 1. validar_tarjeta(tarjeta)
      │  refinar →                      2. verificar_fondos(monto)
      │                                 3. if fondos_ok:
      ▼                                      4. debitar(monto)
  más detalle                               5. generar_recibo()
  en cada iteración                     6. else:
                                            7. lanzar_excepcion(SinFondos)
```

> 📝 **Lectura:** Se parte de una descripción general del módulo y, en cada iteración del diseño, se añaden más detalles hasta llegar a una especificación que los programadores puedan implementar directamente.
> 

---

### Los 4 Elementos del Modelo del Diseño

```
  ┌──────────────────────────────────────────────────────────────┐
  │                    MODELO DEL DISEÑO                         │
  ├───────────────────┬──────────────────┬───────────────────────┤
  │  1. ARQUITECTURA  │   2. INTERFAZ    │   3. COMPONENTES      │
  │                   │                  │                       │
  │  Estructura de    │  • Interna       │  Módulos que forman   │
  │  los módulos      │    (entre comps) │  la arquitectura.     │
  │  del sistema.     │  • Externa       │  Especifican:         │
  │                   │    (con sistemas │  - Estructuras datos  │
  │  Fuentes:         │    externos)     │  - Algoritmos         │
  │  • Dominio        │  • Usuario (UI)  │  - Interfaces módulo  │
  │  • Requisitos     │    mockups,      │                       │
  │  • Catálogo de    │    wireframes    │                       │
  │    patrones       │                  │                       │
  ├───────────────────┴──────────────────┴───────────────────────┤
  │                   4. DESPLIEGUE                              │
  │  Entorno físico donde se implantará el sistema:             │
  │  servidores, contenedores, cloud, red, etc.                 │
  └──────────────────────────────────────────────────────────────┘
```

> 📝 **Lectura:** El modelo del diseño tiene 5 elementos (datos/clases + arquitectura + interfaz + componentes + despliegue). Cada uno responde a una pregunta diferente: *¿cómo se estructuran los datos? ¿cómo se organizan los módulos? ¿cómo se ve la UI? ¿qué hace cada módulo? ¿dónde se ejecuta?*
> 

---

### Atributos de Calidad del Diseño

```
  ┌─────────────────────────────────────────────────────────────┐
  │           ATRIBUTOS DE CALIDAD DEL DISEÑO                   │
  ├──────────────────┬──────────────────────────────────────────┤
  │  Funcionalidad   │ Cumple con todos los requisitos definidos │
  ├──────────────────┼──────────────────────────────────────────┤
  │  Usabilidad      │ Fácil de usar para el usuario final       │
  ├──────────────────┼──────────────────────────────────────────┤
  │  Fiabilidad      │ Opera correctamente sin fallos            │
  ├──────────────────┼──────────────────────────────────────────┤
  │  Rendimiento     │ Responde en tiempo y usa recursos eficaz. │
  ├──────────────────┼──────────────────────────────────────────┤
  │  Mantenibilidad  │ Fácil de modificar, ampliar y corregir    │
  │  ★ más importante│ → facilita la evolución futura del sistema│
  └──────────────────┴──────────────────────────────────────────┘
```

> 📝 **Lectura:** La mantenibilidad suele ser el atributo más valorado a largo plazo. Un software difícil de mantener encarece enormemente el ciclo de vida del producto.
> 

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **Diseño ≠ Código:** El diseño es una representación abstracta del sistema. No es código ejecutable; es la especificación de *cómo* se construirá.
- ❗ **No omitir el diseño:** Aunque el modelado de requisitos sea muy detallado, el diseño NO puede saltarse. Los requisitos dicen *qué*, el diseño dice *cómo*.
- ✅ **Alta cohesión + Bajo acoplamiento:** Es la regla de oro de la modularidad. Memorízala para el examen y para la vida.
- ✅ **Rediseño ≠ cambiar funcionalidad:** El rediseño (refactoring) mejora la estructura interna *sin* cambiar lo que hace el sistema externamente.
- 🔁 **Conceptos de POO que reaparecen:** Los conceptos del Tema 5 (clase, herencia, polimorfismo, encapsulamiento) son la base del diseño orientado a objetos. El ocultamiento de información es la aplicación de diseño del encapsulamiento.
- ❗ **Patrón de diseño ≠ patrón de requisitos:** Los patrones del Tema 6 son para modelar requisitos. Los patrones de diseño (GoF: Factory, Observer, Strategy…) son soluciones a problemas de *diseño e implementación*.

---

## 🔗 Conexiones con otros temas

- **Temas 5 y 6 — Análisis:** El modelo de requisitos es la entrada directa al diseño. Cada diagrama del análisis alimenta un nivel del modelo de diseño.
- **Temas posteriores — Arquitectura y Componentes:** Los 4 niveles del modelo de diseño (datos, arquitectura, interfaz, componentes) se estudiarán en detalle en los siguientes temas.
- **Patrones de Diseño GoF:** Los conceptos de patrón de diseño introducidos aquí se amplían con los patrones clásicos de Gamma, Helm, Johnson y Vlissides (*Design Patterns*, 1994).
- **Testing:** La modularidad y el ocultamiento de información facilitan las pruebas unitarias: si los módulos son independientes, se pueden probar de forma aislada.

---

## 📝 Preguntas de repaso

1. ¿En qué posición del ciclo de vida se encuentra el diseño de software? ¿Qué pregunta responde?
2. ¿Cuáles son los 4 niveles del diseño de software? Descríbelos brevemente.
3. ¿Qué es la modularidad y por qué es un concepto clave del diseño?
4. Explica la diferencia entre cohesión y acoplamiento. ¿Cuáles son los valores deseables y por qué?
5. ¿Qué es el ocultamiento de información? Da un ejemplo concreto.
6. ¿En qué se diferencia el rediseño del refinamiento sucesivo?
7. ¿Cuáles son los 5 atributos de calidad del diseño? ¿Cuál suele ser el más relevante a largo plazo?
8. ¿Qué elementos componen el modelo del diseño y qué modela cada uno?
9. ¿Qué pregunta responde el diseño: *¿qué hay que hacer?* o *¿cómo hay que hacerlo?*

---

## 📌 Resumen en una página

> El **diseño de software** es el proceso iterativo que transforma los requisitos (*¿qué?*) en una especificación concreta (*¿cómo?*), ubicándose entre el análisis y la construcción del código. Se realiza en **4 niveles**: datos/clases, arquitectura, interfaz y componentes (más despliegue). Todo diseño debe perseguir 5 atributos de calidad: funcionalidad, usabilidad, fiabilidad, rendimiento y mantenibilidad. Los **9 conceptos clave** son: abstracción, arquitectura, patrón de diseño, división de problemas, modularidad, ocultamiento de información, independencia funcional (cohesión alta + acoplamiento bajo), refinamiento sucesivo y rediseño. El **modelo del diseño** tiene 4 elementos: arquitectura (estructura de módulos), interfaz (UI, APIs internas/externas), componentes (detalle de cada módulo) y despliegue (entorno físico). En diseño OO se retoman los conceptos del análisis (clases, herencia, polimorfismo) y se aplican a la solución concreta.
>