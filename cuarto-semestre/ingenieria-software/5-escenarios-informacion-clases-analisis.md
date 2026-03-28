# Escenarios, Información y Clases de Análisis

> **Unidad:** Tema 5
> 
> 
> **Palabras clave:** `UML`, `casos de uso`, `diagrama de clases`, `modelado de requisitos`, `análisis orientado a objetos`, `entidad-relación`
> 

---

## 🎯 Objetivo de la clase

Aprender a modelar los requisitos de un sistema software utilizando tres enfoques complementarios: basado en escenarios (casos de uso), basado en datos (Entidad-Relación) y basado en clases (UML). La fase de análisis responde a la pregunta **¿qué hay que hacer?**, no cómo hacerlo.

---

## 🧠 Conceptos principales

### Modelado de Requisitos

Es la actividad que transforma lo que el cliente quiere en una representación formal que sirva de base para el diseño del software. Tiene tres objetivos fundamentales:

1. Describir lo que quiere el cliente.
2. Establecer la base para el posterior diseño del software.
3. Definir el conjunto de requisitos que habrá que validar una vez construido el software.

> 💡 **Intuición:** Piensa en el modelado de requisitos como el plano de un arquitecto antes de construir una casa: no dice cómo se fabricarán los ladrillos, solo qué habitaciones necesita el cliente y cómo se relacionan entre sí.
> 

### Tipos de Modelado de Requisitos

| Tipo | Descripción | Se centra en... |
| --- | --- | --- |
| **Basado en escenarios** | Representa cómo interactúa el usuario con el sistema | Los actores del sistema |
| **Modelado de datos** | Desde el punto de vista de la información | Los datos y sus relaciones |
| **Orientado a clases** | Paradigma orientado a objetos | Clases, atributos, métodos e interacciones |
| **Orientado al flujo** | Funcionalidades y transformación de datos | Los procesos del sistema |
| **Del comportamiento** | Comportamiento ante eventos externos | Los estados del sistema |

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Análisis estructurado** | Analiza independientemente los datos y los procesos que los transforman |
| **Análisis orientado a objetos** | Analiza las clases del sistema: datos, operaciones e interrelaciones entre ellas |
| **Modelado de datos** | Define los objetos de datos procesados en el sistema, sus atributos y las relaciones entre ellos |
| **Objeto de datos** | Representación de información compuesta por atributos que permiten identificar la instancia, describirla o hacer referencia a otra instancia de datos |
| **Modelado basado en clases** | Representa las clases del sistema, identificando sus datos (atributos) y operaciones (métodos), así como las interrelaciones entre ellas |

---

## 📊 Diagramas

### Estructura del Modelo de Requisitos

```
                    MODELO DE LOS REQUISITOS
        ┌───────────────┬───────────────┬───────────────┐
        │  ANÁLISIS DE  │  ESCENARIOS   │     DATOS     │   CLASES
        │  REQUISITOS   │               │               │
        └───────────────┼───────────────┼───────────────┼───────────┐
                        │ D. Casos      │ D. Entidad-   │ D. Clase  │
                        │   de uso      │   Relación    │           │
                        │ D. Actividades│               │           │
                        │ D. Canal      │               │           │
                        └───────────────┴───────────────┴───────────┘
```

> 📝 **Lectura:** El modelo de requisitos se construye desde tres perspectivas: escenarios (cómo interactúa el usuario), datos (qué información maneja el sistema) y clases (cómo se estructura el software en objetos).
> 

---

### Estructura de una Clase UML

```
┌─────────────────────────┐
│       NombreClase       │  ← [1] Nombre de la clase
├─────────────────────────┤
│ - atributo1: Tipo       │  ← [2] Atributos
│ # atributo2: Tipo       │       - privado
│ + atributo3: Tipo       │       # protegido
│                         │       + público
├─────────────────────────┤
│ + operacion1(): TipoRet │  ← [3] Operaciones (métodos)
│ - operacion2(): void    │
│ + operacion3(): TipoRet │
└─────────────────────────┘
```

> 📝 **Lectura:** Cada clase en UML se representa con una caja de tres compartimentos: nombre arriba, atributos en el medio y métodos abajo. Los símbolos de visibilidad (`-`, `#`, `+`) indican quién puede acceder a cada miembro.
> 

---

### Relaciones entre Clases UML

```
  ┌──────────────────────────────────────────────────────────────┐
  │                   TIPOS DE RELACIONES UML                    │
  ├──────────────────┬───────────────────────────────────────────┤
  │                  │                                           │
  │  GENERALIZACIÓN  │   ClaseHija ──────────────────▷ ClasePadre│
  │  (Herencia)      │              flecha triángulo hueco        │
  │                  │                                           │
  ├──────────────────┼───────────────────────────────────────────┤
  │                  │                                           │
  │  ASOCIACIÓN      │   ClaseA ─────────────────────── ClaseB   │
  │                  │          línea simple (o con flecha)       │
  │                  │          puede incluir multiplicidad: 1..* │
  │                  │                                           │
  ├──────────────────┼───────────────────────────────────────────┤
  │                  │                                           │
  │  DEPENDENCIA     │   ClaseA - - - - - - - - - - - ▶ ClaseB  │
  │                  │          línea punteada                    │
  │                  │          cambios en B afectan a A          │
  │                  │                                           │
  ├──────────────────┼───────────────────────────────────────────┤
  │                  │                                           │
  │  AGREGACIÓN      │   Todo ◇──────────────────────── Parte   │
  │  (todo/parte)    │          rombo hueco en el "todo"          │
  │                  │                                           │
  ├──────────────────┼───────────────────────────────────────────┤
  │                  │                                           │
  │  COMPOSICIÓN     │   Todo ◆──────────────────────── Parte   │
  │  (dependencia    │          rombo relleno en el "todo"        │
  │   total)         │          las partes no existen sin el todo │
  │                  │                                           │
  └──────────────────┴───────────────────────────────────────────┘
```

> 📝 **Lectura:** Hay 5 tipos de relaciones entre clases. La herencia (▷) forma jerarquías. La asociación (─) indica que una clase "conoce" a la otra. La dependencia (---▶) es la más débil. La agregación (◇) y composición (◆) son relaciones todo/parte, siendo la composición la más fuerte (las partes no pueden existir sin el todo).
> 

---

### Diagrama de Casos de Uso (ejemplo del PDF)

```
  ┌──────────────────────────────────────────────┐
  │               Sistema CasaSegura             │
  │                                              │
  │    ╭────────────────────────────╮            │
  │    │ Acceso por internet a las  │──────────────────── [Cámaras]
  │    │ cámaras de vigilancia      │            │
  │    ╰────────────────────────────╯            │
  │             ▲                                │
  │    ╭────────────────────────────╮            │
[Propietario]──│ Configura parámetros           │
  │    │ del sistema CasaSegura     │            │
  │    ╰────────────────────────────╯            │
  │             ▲                                │
  │    ╭────────────────────────────╮            │
  │    │      Activa alarma         │            │
  │    ╰────────────────────────────╯            │
  └──────────────────────────────────────────────┘

  [ ] = Actor externo     ╭╮ = Caso de uso (óvalo)
```

> 📝 **Lectura:** El actor `Propietario` inicia tres funcionalidades del sistema. La cámara es un actor secundario que participa en el caso de uso de acceso por internet. Los óvalos representan funcionalidades; los actores están fuera del sistema.
> 

---

### Flujo de un Diagrama de Actividades (ejemplo login)

```
                    ●  (inicio)
                    │
         ┌──────────▼──────────┐
         │ Introducir password │
         │ e identificación    │
         └──────────┬──────────┘
                    │
           ┌────────◇────────┐
           │   ¿Credenciales │
           │     válidas?    │
           └────┬────────────┘
           SI   │         NO │
    ┌───────────▼─┐    ┌─────▼──────────────┐
    │  Seleccionar│    │ Mostrar mensaje de  │
    │  función    │    │ error y reintentar  │
    │  principal  │    └────────────────────┘
    └───────────┬─┘
                │
    ┌───────────▼──────────┐
    │  Seleccionar         │
    │  vigilancia          │
    └───────────┬──────────┘
                │
         ┌──────◇──────┐
         │  ¿Vista?    │
         └──┬──────────┘
   Reducida │      Específica│
  ┌─────────▼──┐  ┌──────────▼────────┐
  │ Seleccionar│  │ Seleccionar ícono │
  │ cámara,    │  │ de cámara         │
  │ vista red. │  └──────────┬────────┘
  └─────────┬──┘             │
            └────────┬───────┘
                     │
          ┌──────────▼──────────┐
          │  Ver salida de la   │
          │  cámara etiquetada  │
          └──────────┬──────────┘
                     │
                     ● (fin)
```

> 📝 **Lectura:** El flujo parte de un nodo inicial (●), pasa por acciones (rectángulos), decisiones (◇) y termina en el nodo final. Las ramas muestran los caminos alternativos del escenario.
> 

---

## 🛠️ Procedimiento: Modelado Basado en Escenarios

**Idea general:** Construir los casos de uso de forma incremental, desde lo general hasta el detalle formal.

**Pasos:**

1. Crear los **casos de uso preliminares**: identificar actores y las funcionalidades que cada uno desencadena.
2. **Refinar** cada caso de uso: analizar en profundidad para identificar escenarios adicionales, condiciones de error y comportamientos alternativos.
3. **Escribir formalmente** cada caso de uso con: actor principal, actores secundarios, objetivo, precondiciones, lista de características del escenario, excepciones y prioridad.
4. **Representar gráficamente** con diagramas UML de casos de uso.

---

## 🛠️ Procedimiento: Modelado Basado en Clases

**Idea general:** Partir de los casos de uso para identificar las clases del sistema y sus relaciones.

**Pasos:**

1. **Identificar las clases**: analizar los casos de uso y extraer los sustantivos. Evaluar cuáles representan entidades del sistema (entidades externas, cosas, eventos, roles, lugares, estructuras, etc.).
2. **Identificar los atributos**: qué datos pertenecen a cada clase.
3. **Identificar los métodos**: qué operaciones definen el comportamiento de la clase.
4. **Identificar las relaciones** entre clases: jerarquías (herencia), asociaciones, agregaciones y dependencias.
5. Si el sistema es grande, **agrupar clases en paquetes** para facilitar el manejo.

---

## 💡 Ejemplo resuelto: Estructura de un Caso de Uso Formal

**Problema:** ¿Cómo se documenta formalmente el caso de uso "Acceso a cámaras" del sistema CasaSegura?

**Solución:**

```
CASO DE USO: Acceso por internet a las cámaras de vigilancia
─────────────────────────────────────────────────────────────
Actor principal:    Propietario
Actores secundarios: Cámaras (sistema externo)
Objetivo:           Visualizar en tiempo real las cámaras desde internet
Precondiciones:     El propietario tiene credenciales válidas registradas
                    El sistema está operativo y conectado a internet

Escenario principal:
  1. El propietario introduce su password e identificación
  2. El sistema valida las credenciales
  3. El propietario selecciona la función de vigilancia
  4. El propietario elige vista reducida o cámara específica
  5. El sistema muestra la salida de video en una ventana etiquetada

Excepciones:
  2a. Credenciales inválidas → el sistema solicita reintento
  2b. Sin intentos restantes → el sistema bloquea el acceso

Prioridad: Alta
```

> 📝 **Nota:** Este formato de caso de uso es la entrada principal para el diagrama de actividades que lo complementa visualmente.
> 

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **Análisis ≠ Diseño:** El análisis responde al *¿qué?*; el diseño responde al *¿cómo?*. No mezclarlos.
- ❗ **No todo sustantivo es una clase:** Al identificar clases desde los casos de uso, muchos sustantivos son atributos, no clases independientes. Evalúa si tienen comportamiento propio.
- ✅ **Composición vs Agregación:** La composición (◆) implica que las partes *no pueden existir* sin el todo. La agregación (◇) es una relación todo/parte más laxa: las partes pueden existir de forma independiente.
- ✅ **Multiplicidad en asociaciones:** Siempre especificar la cardinalidad (1, *, 0..1, 1..*) en las relaciones de asociación para clarificar cuántos objetos pueden participar.
- 🔁 **Diagramas de actividades vs casos de uso:** Los casos de uso identifican *qué* hace el sistema; los diagramas de actividades describen *cómo fluye* internamente cada escenario. Son complementarios.
- 🔁 **Diagramas de actividades con canal (swimlane):** Añaden la dimensión de *responsabilidad*, asignando cada acción a un actor o componente específico. Útil para ver quién hace qué dentro del flujo.

---

## 🔗 Conexiones con otros temas

- **Tema anterior — Captura de requisitos:** Los casos de uso se originan de las entrevistas y técnicas de captura de requisitos. El modelado es el paso siguiente que formaliza esa información.
- **Tema siguiente — Diseño del software:** El modelado de clases producido en el análisis es la entrada directa para el diseño detallado de la arquitectura y la implementación.
- **Bases de Datos:** El modelado Entidad-Relación (E-R) del análisis de requisitos es el precursor del modelo relacional que se usa en el diseño de bases de datos.
- **POO (Programación Orientada a Objetos):** Los conceptos de herencia, asociación, agregación y composición del diagrama de clases UML se mapean directamente a las construcciones del lenguaje de programación (clases, herencia, composición en Java/C++/Python).

---

## 📝 Preguntas de repaso

1. ¿Cuál es la diferencia entre el análisis estructurado y el análisis orientado a objetos?
2. ¿Qué información mínima debe contener un caso de uso escrito formalmente?
3. ¿En qué se diferencian un diagrama de actividades simple y uno con canal (swimlane)?
4. ¿Cuál es la diferencia entre agregación y composición? Da un ejemplo de cada una.
5. ¿Cómo se representa la herencia en un diagrama de clases UML? ¿Hacia dónde apunta la flecha?
6. ¿Para qué sirven los diagramas Entidad-Relación en el contexto del análisis de requisitos?
7. ¿Por qué se recomienda agrupar clases en paquetes cuando el sistema es muy grande?
8. ¿En qué fase del desarrollo se usa el modelado de requisitos: análisis o diseño?

---

## 📌 Resumen en una página

> El **modelado de requisitos** es la fase de análisis que transforma lo que el cliente quiere en representaciones formales del sistema, respondiendo siempre al *¿qué hay que hacer?* sin entrar en el *¿cómo?*. Se aborda desde tres perspectivas: **escenarios** (casos de uso + diagramas de actividades), **datos** (diagramas Entidad-Relación) y **clases** (diagramas UML de clases). Los casos de uso se construyen identificando actores y funcionalidades, refinándolos hasta escribirlos formalmente con precondiciones y excepciones, y luego visualizándolos con UML. El modelado de clases parte de extraer sustantivos de los casos de uso para identificar clases con sus atributos y métodos, y luego conectarlas mediante relaciones de herencia (▷), asociación (─), dependencia (---▶), agregación (◇) o composición (◆). Cada clase UML se representa con una caja de tres partes: nombre, atributos y operaciones, con visibilidad pública (+), privada (-) o protegida (#).
>