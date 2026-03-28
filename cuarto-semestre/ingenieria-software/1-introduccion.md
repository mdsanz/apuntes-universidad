# Introducción a la IS y al Modelado

> **Unidad:** Tema 1
> 
> 
> **Palabras clave:** `software`, `ingeniería del software`, `proceso`, `UML`, `orientación a objetos`, `webapps`, `mitos`, `modelado`
> 

---

## 🎯 Objetivo de la clase

Comprender qué es el software y la ingeniería del software, conocer su proceso y práctica, identificar los mitos comunes del sector, y obtener una introducción al paradigma orientado a objetos y al lenguaje de modelado UML.

---

## 🧠 Conceptos principales

### 1.2 La Naturaleza del Software

El **software** es un conjunto de instrucciones, estructuras de datos e información descriptiva.

A diferencia del hardware, el software tiene tres características únicas:

1. Se **desarrolla o modifica con intelecto** (no se fabrica en el sentido clásico)
2. **No se «desgasta»** con el uso (aunque sí se deteriora por cambios mal gestionados)
3. Se **construye para uso individualizado** (aunque puede distribuirse masivamente)

### Dominios de aplicación del software

| Dominio | Descripción |
| --- | --- |
| **Sistemas** | Software base: SO, drivers, utilerías |
| **Aplicación** | Software para procesar información de negocio |
| **Ingeniería y ciencias** | Algoritmos numéricos, simulación |
| **Incrustado** | Software dentro de productos (electrodomésticos, automóviles) |
| **Línea de productos** | Funcionalidades reutilizables para múltiples clientes |
| **Aplicaciones web (webapps)** | Aplicaciones accesibles vía red/internet |
| **Inteligencia artificial** | Algoritmos no numéricos para problemas complejos |

### Software heredado (*legacy software*)

Es el software desarrollado en décadas pasadas (años 60, 70, 80) que:

- Contiene funciones **críticas para el negocio**
- Se caracteriza frecuentemente por su **mala calidad**
- Se ha ido modificando para adaptarse a nuevas necesidades

> 💡 **Regla de oro:** Si funciona y satisface al usuario → no tocar. Si necesita adaptación tecnológica o nuevos requisitos → hacer **reingeniería**.
> 

---

### 1.3 La Naturaleza Única de las Webapps

Las aplicaciones web tienen características diferenciadoras respecto al resto del software:

- 🌐 **Uso intensivo de redes**
- 🔀 **Concurrencia** (muchos usuarios simultáneos)
- ⚡ **Rendimiento y disponibilidad críticos**
- 📊 **Orientadas a datos**
- 🎨 **Contenido y estética muy importantes**
- 🔄 **Evolución continua** con plazos cortos de desarrollo
- 🔒 **Seguridad** fundamental

---

### 1.4 Ingeniería del Software

Antes de afrontar un desarrollo software se debe:

1. **Entender el problema** a resolver
2. El **diseño previo** es crucial
3. Garantizar **alta calidad**
4. Asegurar un **mantenimiento posterior sencillo**

La ingeniería del software se estructura en **cuatro capas**:

```
  ┌────────────────────────────────────────────────────┐
  │               Herramientas (Tools)                  │  ← Soporte automático a métodos y proceso
  ├────────────────────────────────────────────────────┤
  │               Métodos (Methods)                     │  ← Cómo construir software
  ├────────────────────────────────────────────────────┤
  │               Proceso (Process)                     │  ← Marco de trabajo
  ├────────────────────────────────────────────────────┤
  │         Compromiso con la calidad (Quality)         │  ← Base fundamental
  └────────────────────────────────────────────────────┘
```

> 📝 **Lectura del diagrama:** La calidad es la base de todo. Sobre ella se construye el proceso, los métodos y las herramientas. Sin compromiso con la calidad, las demás capas no tienen fundamento sólido.
> 

---

### 1.5 El Proceso del Software

> *"Un proceso define quién hace qué, cuándo y cómo, para alcanzar cierto objetivo."*
> 
> 
> — Jacobson, Booch y Rumbaugh
> 

### Cinco actividades principales (aplicadas de forma iterativa)

```
  ┌───────────────┐   ┌────────────────┐   ┌───────────┐   ┌───────────────┐   ┌────────────┐
  │ COMUNICACIÓN  │──▶│  PLANIFICACIÓN │──▶│ MODELADO  │──▶│ CONSTRUCCIÓN  │──▶│ DESPLIEGUE │
  └───────────────┘   └────────────────┘   └───────────┘   └───────────────┘   └────────────┘
```

> 📝 **Lectura:** El proceso no es rígidamente lineal; en la práctica se aplica de manera **iterativa e incremental**, volviendo a etapas anteriores según sea necesario.
> 

### Actividades sombrilla (transversales a todo el proyecto)

Son actividades que **se aplican durante todo el proyecto**, sin limitarse a una fase concreta:

- Seguimiento y control del proyecto
- Gestión del riesgo
- Aseguramiento de la calidad
- Revisiones técnicas
- Medición
- Gestión de la configuración
- Gestión de la reutilización
- Preparación y producción del producto del trabajo

> ⚠️ Las actividades sombrilla **no son fases** del proceso; son prácticas continuas.
> 

---

### 1.6 La Práctica de la Ingeniería del Software

Para resolver cualquier problema (incluyendo los de software) se debe:

1. **Entender el problema**
2. **Planificar la solución**
3. **Ejecutar el plan**
4. **Examinar el resultado**

### Los 7 Principios de David Hooker

| # | Principio |
| --- | --- |
| 1 | Dar valor a los usuarios |
| 2 | Sencillez |
| 3 | Mantener la visión clara del proyecto |
| 4 | Otros consumirán lo producido |
| 5 | Apertura al futuro |
| 6 | Planificación anticipada de la reutilización |
| 7 | Pensar en todo con claridad antes de actuar |

---

### 1.7 Mitos del Software

Son creencias falsas y peligrosas que deben evitarse:

### 🏢 Mitos de la administración

- ❌ *"Tenemos un libro de estándares, eso basta."* → Los estándares no reemplazan el juicio profesional ni la comunicación activa.
- ❌ *"Si nos atrasamos, agregamos más programadores."* → Añadir personas a un proyecto tardío lo retrasa aún más (Ley de Brooks).
- ❌ *"Puedo subcontratar y descansar."* → La supervisión y comunicación siguen siendo responsabilidad del cliente.

### 👤 Mitos del cliente

- ❌ *"Con un enunciado general es suficiente para empezar."* → Los requisitos vagos generan retrabajo costoso.
- ❌ *"Los cambios de requisitos se asimilan fácilmente."* → Los cambios tardíos son exponencialmente más costosos.

### 💻 Mitos del profesional

- ❌ *"Una vez funcionando, el trabajo terminó."* → El mantenimiento puede representar el 60-80% del costo total.
- ❌ *"No se puede evaluar la calidad hasta que se ejecute."* → Existen técnicas de revisión y análisis estático.
- ❌ *"El único producto es el programa que funciona."* → La documentación, los casos de prueba y los modelos también son productos.
- ❌ *"La IS genera documentación innecesaria que retrasa."* → La documentación bien gestionada reduce costos a largo plazo.

---

### 1.8 Cómo Comienza Todo

Los proyectos de software surgen por tres tipos de necesidades:

1. **Ampliación de funcionalidades** o corrección de defectos en una aplicación existente
2. **Adaptación de un sistema heredado** a un nuevo entorno tecnológico
3. **Creación de un nuevo producto**, servicio o sistema

---

### 1.9 Conceptos Orientados a Objetos

### Elementos fundamentales

| Concepto | Definición |
| --- | --- |
| **Clase** | Estructura que encapsula atributos (datos) y métodos (operaciones) |
| **Objeto** | Instancia concreta de una clase |
| **Atributos** | Datos o propiedades que describen el estado del objeto |
| **Métodos** | Funciones u operaciones que define el comportamiento |
| **Mensaje** | Llamada de un objeto emisor para ejecutar un método del objeto receptor |

### Propiedades características de la OO

**Herencia:** Una clase hija (subclase) hereda automáticamente todos los atributos y métodos de la clase padre (superclase).

```
  ┌──────────────────┐
  │    Vehículo      │  ← Superclase (padre)
  ├──────────────────┤
  │ + velocidad      │
  │ + marca          │
  │ + acelerar()     │
  └────────┬─────────┘
           │  hereda  ▷
  ┌────────▼─────────┐
  │    Automóvil     │  ← Subclase (hija)
  ├──────────────────┤
  │ + numPuertas     │  ← atributos propios
  │ + abrirMaletero()│  ← métodos propios
  └──────────────────┘
```

> 📝 **Lectura:** `Automóvil` hereda todo de `Vehículo` y añade sus propias características.
> 

**Polimorfismo:** Permite posponer a tiempo de ejecución qué tipo de objeto se usará y qué código de método se ejecutará.

> 💡 **Intuición:** El polimorfismo es como un enchufe universal — el mismo conector, pero el comportamiento varía según el dispositivo que conectas.
> 

### Principios de diseño de una buena clase

- **Encapsulamiento:** Los atributos deben ser privados; el acceso, controlado por métodos.
- **Alta cohesión:** Una clase debe tener una única responsabilidad bien definida.
- **Bajo acoplamiento:** Las clases deben depender lo menos posible entre sí.

---

### 1.10 Introducción a UML

El **Lenguaje de Modelado Unificado (UML)** es un lenguaje estándar para modelar software mediante diagramas.

### Principales diagramas UML

| Diagrama | Qué representa |
| --- | --- |
| **Clases** | Clases con atributos/métodos y sus relaciones |
| **Implementación** | Distribución física del sistema en hardware |
| **Casos de uso** | Interacción del usuario con las funciones del sistema |
| **Secuencia** | Comunicación dinámica entre objetos en orden temporal |
| **Comunicación** | Relaciones entre objetos para realizar una tarea |
| **Actividad** | Flujo de control de acciones (comportamiento dinámico) |
| **Estado** | Estados de un objeto y transiciones entre ellos |

### Ejemplo de Diagrama de Clases (UML)

```
  ┌─────────────────────────┐          ┌─────────────────────────┐
  │        Cliente          │          │         Pedido          │
  ├─────────────────────────┤          ├─────────────────────────┤
  │ - id: int               │  1    *  │ - numeroPedido: int      │
  │ - nombre: String        │◆────────▶│ - fecha: Date           │
  │ - email: String         │          │ - total: double          │
  ├─────────────────────────┤          ├─────────────────────────┤
  │ + registrar(): void     │          │ + calcularTotal(): double│
  │ + obtenerPedidos(): List │          │ + confirmar(): void      │
  └─────────────────────────┘          └─────────────────────────┘
```

> 📝 **Lectura:** Un `Cliente` puede tener muchos (`*`) `Pedidos`. La composición (`◆`) indica que los pedidos pertenecen al cliente.
> 

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **No confundir actividades principales con actividades sombrilla**: las sombrillas son transversales, no fases.
- ❗ **El software no se "desgasta"**, pero sí puede degradarse con cambios mal gestionados.
- ✅ El proceso del software se aplica de forma **iterativa**, no en cascada rígida.
- ✅ Un buen diseño OO debe garantizar: encapsulamiento + alta cohesión + bajo acoplamiento.
- 🔁 UML es el lenguaje de modelado de la IS moderna, que casi siempre usa el paradigma OO.
- ❗ Nunca confundir **clase** (plantilla) con **objeto** (instancia concreta de la clase).

---

## 🔗 Conexiones con otros temas

- **Temas posteriores del curso:** Cada tema profundizará en un tipo específico de diagrama UML.
- **Programación OO:** Los conceptos de herencia, polimorfismo y encapsulamiento son los mismos vistos en programación, pero ahora aplicados a nivel de diseño/modelado.
- **Crisis del Software:** El origen histórico de la IS como disciplina surge directamente de la necesidad de controlar la complejidad del software mal planificado.

---

## 📝 Preguntas de repaso

1. ¿Cuáles son las cuatro capas de la ingeniería del software y en qué orden se estructuran?
2. ¿Cuáles son las cinco actividades principales del proceso del software?
3. ¿Qué diferencia hay entre actividades principales y actividades sombrilla?
4. ¿Por qué el software heredado suele ser problemático y cuándo hay que hacer reingeniería?
5. ¿Cuáles son las tres diferencias fundamentales entre software y hardware?
6. Menciona tres mitos del software y explica por qué son falsos.
7. ¿Qué es la herencia y el polimorfismo en OO? Da un ejemplo de cada uno.
8. ¿Qué diagrama UML usarías para mostrar los estados de un semáforo? ¿Y para mostrar cómo un usuario inicia sesión?
9. ¿Cuáles son los 7 principios de Hooker para la ingeniería del software?
10. ¿Qué características hacen únicas a las webapps frente a otros tipos de software?

---

## 📌 Resumen en una página

La Ingeniería del Software es la aplicación de un enfoque sistemático, disciplinado y cuantificable al desarrollo, operación y mantenimiento del software. El software, a diferencia del hardware, no se desgasta y se construye intelectualmente; sus dominios van desde sistemas operativos hasta inteligencia artificial. Las webapps tienen características únicas como concurrencia, disponibilidad crítica y evolución continua. El proceso de desarrollo se estructura en cinco actividades iterativas (comunicación, planificación, modelado, construcción, despliegue), complementadas por actividades sombrilla que se aplican durante todo el proyecto. Existen mitos peligrosos en administración, cliente y equipo de desarrollo que deben conocerse para evitarlos. La IS moderna se apoya en el paradigma OO (clases, objetos, herencia, polimorfismo, encapsulamiento) y en UML como lenguaje estándar de modelado, con diagramas como el de clases, casos de uso, secuencia, estado y actividad, que se verán en profundidad a lo largo del curso.