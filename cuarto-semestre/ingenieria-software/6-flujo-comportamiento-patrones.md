# Flujo, Comportamiento y Patrones

> **Unidad:** Tema 6
> 
> 
> **Palabras clave:** `DFD`, `diagrama de estados`, `diagrama de secuencia`, `diagrama de comunicación`, `modelo de comportamiento`, `patrones de software`, `modelado orientado al flujo`
> 

---

## 🎯 Objetivo de la clase

Completar el modelado de requisitos con tres perspectivas adicionales: el **flujo de datos** (DFD), el **comportamiento** ante eventos externos (diagramas de estado y secuencia) y la **reutilización del conocimiento** mediante patrones. Estos modelos complementan los vistos en el Tema 5 (escenarios y clases).

---

## 🧠 Conceptos principales

### Análisis Estructurado vs. Orientado a Objetos

Estas dos estrategias son las grandes corrientes del análisis de requisitos:

- **Análisis estructurado:** modela los datos y sus procesos como entidades *independientes*. Herramienta principal: DFD.
- **Análisis orientado a objetos:** se centra en las clases y sus interrelaciones. Herramienta principal: diagramas de clases UML (Tema 5).

> 💡 **Intuición:** El análisis estructurado ve el sistema como una "fábrica" con materias primas (datos) y máquinas (procesos). El orientado a objetos lo ve como un conjunto de "actores" que colaboran entre sí.
> 

---

### Diagrama de Flujo de Datos (DFD)

Modela los requisitos del sistema desde el punto de vista del **flujo de datos**, siguiendo el modelo **Entrada → Proceso → Salida**. Se construye de forma jerárquica, empezando por el nivel más abstracto:

- **Nivel 0 (Diagrama de contexto):** representa el sistema como una única burbuja, mostrando todas las entradas y salidas externas. Da una visión global sin detalles internos.
- **Niveles siguientes:** cada burbuja del nivel anterior se descompone en sub-procesos con mayor detalle.

> 💡 **Intuición:** Es como hacer zoom en un mapa. Primero ves el país entero (nivel 0), luego la ciudad (nivel 1), luego el barrio (nivel 2)…
> 

### Diagrama de Estado

Modela el **flujo de control** del sistema, definiendo los estados por los que pasa un objeto y las transiciones entre ellos al ocurrir eventos.

Cada estado puede tener acciones asociadas:

- **Entrar/Fijar** → acción al entrar al estado
- **Hacer** → acción continua mientras se está en el estado
- **Salir** → acción al salir del estado

> 💡 **Intuición:** Piensa en un semáforo: tiene estados (rojo, amarillo, verde) y transiciones (eventos de tiempo) entre ellos. Un diagrama de estado captura exactamente ese comportamiento.
> 

### Modelo de Comportamiento

Describe cómo responderá el software a **eventos o estímulos externos**. Se construye a partir de los casos de uso y se representa con diagramas de estado y de secuencia.

### Diagrama de Secuencia

Modela las **comunicaciones entre objetos** para la ejecución de una tarea, mostrando el **orden temporal** de los mensajes. Cada objeto tiene una línea de vida vertical; los mensajes son flechas horizontales entre líneas de vida.

### Diagrama de Comunicación

Modela la misma información que el diagrama de secuencia, pero desde un punto de vista diferente: muestra la **red de objetos** y los mensajes entre ellos con numeración para indicar el orden (ej: `1.1`, `1.2`, `1.2.1`…).

### Patrones de Software

Soluciones documentadas a problemas recurrentes en el modelado. Cada patrón debe documentar:

- El **problema** al que es aplicable
- La **solución** propuesta
- Las **suposiciones** y **restricciones** de uso

> 💡 **Intuición:** Un patrón es como una receta de cocina: no resuelve tu problema exacto, pero te da una guía probada que puedes adaptar.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **DFD** | Diagrama de Flujo de Datos. Representa el sistema como entradas, procesos y salidas. Modelo del análisis estructurado. |
| **Diagrama de contexto (Nivel 0)** | DFD que representa el sistema completo como una única burbuja, con todas sus interfaces externas |
| **Diagrama de estado** | Modela el flujo de *control* de un objeto: sus estados posibles y las transiciones (eventos) entre ellos |
| **Modelo de comportamiento** | Conjunto de diagramas que describen cómo responde el software a eventos externos |
| **Diagrama de secuencia** | Diagrama UML que muestra el orden temporal de los mensajes entre objetos para una tarea específica |
| **Diagrama de comunicación** | Diagrama UML equivalente al de secuencia, pero organizado como una red de objetos con mensajes numerados |
| **Patrón de software** | Conocimiento documentado sobre una solución a un problema recurrente, reutilizable en contextos similares |

---

## 📊 Diagramas

### Mapa del Modelado de Requisitos — Tema 6

```
              MODELADO DE LOS REQUISITOS (Tema 6)
   ┌───────────────────────┬───────────────────────────┐
   │        FLUJO          │      COMPORTAMIENTO       │   PATRONES
   ├───────────────────────┼────────────┬──────────────┤
   │ DFD (Nivel 0→N)       │ D. Estado  │ D. Secuencia │
   │ Entrada-Proceso-Salida│            │ D. Comunic.  │
   └───────────────────────┴────────────┴──────────────┘
```

> 📝 **Lectura:** El Tema 6 completa el modelo de requisitos con tres enfoques: flujo de datos (DFD), comportamiento dinámico (estado + secuencia + comunicación) y reutilización de conocimiento (patrones).
> 

---

### DFD Nivel 0 — Diagrama de Contexto (Sistema CasaSegura)

```
                     ┌──────────────────────┐
  Panel de  ─────────▶                      │
  Control   ◀─────────  Software CasaSegura ├─────────▶ Pantalla del
            Comandos /  │                   │           panel de control
            datos usuario  (sistema como    │
                        │   una burbuja)    ├─────────▶ Alarma
  Sensores  ─────────▶  │                  │
  (estado   ◀─────────  │                  ├─────────▶ Línea telefónica
   del sensor)          └──────────────────┘
```

> 📝 **Lectura:** En el nivel 0 el sistema completo es una sola burbuja. Solo se muestran las entidades externas (Panel, Sensores) y los flujos de datos que entran y salen. Ningún proceso interno es visible aún.
> 

---

### DFD Nivel 1 — Descomposición de CasaSegura

```
  Panel de ──Comandos──▶ ┌─────────────────────┐  Configuración ──▶ Pantalla
  Control                │  Configurar sistema  │
                         └──────────┬──────────┘
                          Config. datos
                                    │
  Panel de ──Password──▶ ┌──────────▼──────────┐  Msg. identificación
  Control                │  Procesar password  │  válida
                         └──────────┬──────────┘
                                    │ Activar/Detener
                         ┌──────────▼──────────┐  Tipo de alarma ──▶ Alarma
  Sensores ─Estado──────▶│  Vigilar sensores   │
                         └──────────┬──────────┘  Tonos tel. ──▶ Línea tel.
                          Info. sensores
                         ┌──────────▼──────────┐
                         │  Mostrar mensajes   ├───────────────▶ Pantalla
                         └─────────────────────┘
```

> 📝 **Lectura:** En el nivel 1 se descompone el sistema en sub-procesos (burbujas). Ahora se puede ver cómo fluyen los datos entre ellos internamente.
> 

---

### Diagrama de Estado — Sistema CasaSegura

```
         ● (inicio)
         │ [Interruptor encendido]
         ▼
  ┌─────────────────┐   [Sistema bien]    ┌──────────────┐
  │   REINICIANDO   │────────────────────▶│    OCIOSO    │
  │                 │◀────────────────────│              │
  │ Hacer: activar  │    [Reiniciar]      │ Espera input │
  │ diagnóstico     │                     └──────┬───────┘
  └─────────────────┘                            │ [Activar]
                                                 ▼
  ┌─────────────────┐  [FalsaAlarma] ┌───────────────────────┐
  │  VIGILANDO EL   │◀───────────────│     ACTIVAR ALARMA    │
  │    SISTEMA      │                │                       │
  │                 │                │ Hacer: SonarAlarma    │
  │ Hacer: Vigilar  │                │ Hacer: Notificar      │
  │ y Controlar     │────────────────▶│ Responsables         │
  └─────────────────┘ [SensorDisp./  └───────────────────────┘
                        ComienzaCron.]       │ [SensorDisparado/
                                             │  ReiniciarCronómetro]
                                             └──────────▶ (bucle)
```

> 📝 **Lectura:** Los rectángulos son estados; las flechas son transiciones disparadas por eventos (entre corchetes). El sistema CasaSegura cicla entre Reiniciando → Ocioso → Vigilando → ActivarAlarma según los eventos que ocurran.
> 

---

### Diagrama de Secuencia — Ejemplo MouseListener

```
  :MouseListener    :Drawing     aFigure:Figure    :Graphics
       │                │               │                │
  ●────▶ mouseClicked(punto)            │                │
       │                │               │                │
       │───getFigureAt(punto)──▶        │                │
       │                │               │                │
       │◀─── aFigure ───│               │                │
       │                │               │                │
       │──────────────highlight(gráfico)▶               │
       │                │               │                │
       │                │               │──setColor(rojo)▶
       │                │               │                │
       │                │               │──drawRect(x,y,w,h)▶
       │                │               │                │
       │                │               │──drawString(s)─▶
       │                │               │                │
  tiempo ↓ (el eje vertical representa el orden temporal)
```

> 📝 **Lectura:** Cada columna es un objeto (con su línea de vida vertical). Las flechas horizontales son mensajes enviados de izquierda a derecha; las flechas discontinuas son respuestas. El tiempo avanza hacia abajo, por lo que el orden de los mensajes es el orden de ejecución.
> 

---

### Diagrama de Comunicación — Mismo ejemplo

```
          1: mouseClicked(punto)
    ──────────────────▼
               ┌──────────────┐
               │ MouseListener│
               └──┬───────────┘
    1.1: getFigureAt(punto) ↙    ↘ 1.2: highlight(graphics)
          ┌─────────┐              ┌─────────┐
          │ Drawing │              │  Figure │
          └─────────┘              └────┬────┘
                                        │ 1.2.1: setColor(rojo)
                                        │ 1.2.2: drawRect(x,y,w,h)
                                        │ 1.2.3: drawString(s)
                                        ▼
                                   ┌──────────┐
                                   │ Graphics │
                                   └──────────┘
```

> 📝 **Lectura:** Aquí la misma interacción del diagrama de secuencia se muestra como una red. Los números (1, 1.1, 1.2, 1.2.1…) indican el orden de los mensajes. Es más fácil ver *quién se comunica con quién*, pero menos fácil ver el orden temporal exacto que en el diagrama de secuencia.
> 

---

### Secuencia vs. Comunicación — Comparación

```
  ┌────────────────────────────┬──────────────────────────────────┐
  │    DIAGRAMA DE SECUENCIA   │    DIAGRAMA DE COMUNICACIÓN      │
  ├────────────────────────────┼──────────────────────────────────┤
  │ Eje vertical = tiempo      │ Sin eje de tiempo explícito      │
  │ Fácil ver el ORDEN         │ Fácil ver la RED de objetos      │
  │ temporal de los mensajes   │ y quién se comunica con quién    │
  ├────────────────────────────┼──────────────────────────────────┤
  │ Layout: columnas paralelas │ Layout: grafo de nodos           │
  │ (líneas de vida)           │ (objetos conectados)             │
  ├────────────────────────────┼──────────────────────────────────┤
  │ Numeración: implícita      │ Numeración: explícita (1, 1.1,   │
  │ (posición vertical)        │  1.2, 1.2.1…)                   │
  └────────────────────────────┴──────────────────────────────────┘
       Ambos modelan la MISMA información, diferente perspectiva
```

> 📝 **Lectura:** Elige secuencia cuando quieras enfatizar el *cuándo* de los mensajes; elige comunicación cuando quieras enfatizar el *quién* interactúa con quién.
> 

---

## 🛠️ Procedimiento: Crear un Modelo de Comportamiento

**Idea general:** Construir el modelo de comportamiento a partir de los casos de uso, identificando cómo responde el sistema a eventos externos.

**Pasos:**

1. **Evaluar los casos de uso** del sistema.
2. **Identificar los eventos** que desencadenan las interacciones entre objetos (ej: clic, timeout, sensor activado).
3. **Crear una secuencia** para cada caso de uso → diagrama de secuencia.
4. **Construir el diagrama de estado** para el sistema completo.
5. **Revisar** el modelo de comportamiento para detectar inconsistencias.

---

## 🛠️ Procedimiento: Documentar un Patrón

**Idea general:** Capturar el conocimiento de una solución probada para reutilizarla en problemas similares futuros.

**Contenido mínimo de un patrón documentado:**

```
PATRÓN: [Nombre]
─────────────────────────────────────
Problema:      ¿A qué situación/problema es aplicable?
Solución:      Descripción de la solución propuesta
Suposiciones:  Condiciones que deben cumplirse para aplicarlo
Restricciones: Limitaciones o contextos donde NO aplica
Ejemplo:       Caso concreto donde se aplicó con éxito
```

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **DFD ≠ diagrama de flujo:** El DFD muestra *flujo de datos*, no flujo de control. No tiene condiciones (if/else) ni bucles explícitos. Para control, usa diagramas de actividad o de estado.
- ❗ **Diagrama de contexto:** Nunca mostrar procesos internos en el nivel 0. Solo el sistema como caja negra + entidades externas + flujos de datos.
- ✅ **Diagrama de secuencia vs. comunicación:** Son **equivalentes en información** pero con diferente énfasis visual. Ninguno es "mejor"; se elige según lo que se quiera comunicar.
- ✅ **Diagrama de estado:** Cada transición debe tener un evento que la dispara. Sin evento, no hay transición válida.
- 🔁 **Relación con Tema 5:** Los diagramas de secuencia y comunicación *complementan* a los diagramas de clases del Tema 5. Las clases son estáticas; la secuencia muestra cómo interactúan *dinámicamente*.
- ❗ **Patrones ≠ soluciones genéricas:** Un patrón debe documentar sus restricciones. Aplicar un patrón fuera de su contexto puede empeorar el diseño.

---

## 🔗 Conexiones con otros temas

- **Tema 5 — Clases y escenarios:** Los diagramas de secuencia y comunicación usan los objetos definidos en el diagrama de clases (Tema 5) y se derivan de los casos de uso (Tema 5).
- **Tema siguiente — Diseño del software:** El modelo de comportamiento (estados, secuencias) es entrada directa para el diseño de la arquitectura: define qué clases necesitan qué métodos y cómo se llaman entre sí.
- **Patrones de Diseño (GoF):** Los patrones de modelado de requisitos son el antecedente de los patrones de diseño (Gamma et al.). Comparten la misma filosofía de reutilización del conocimiento.
- **Análisis estructurado:** El DFD es la herramienta principal del análisis estructurado clásico (años 70-80). Aunque hoy domina el análisis orientado a objetos, el DFD sigue siendo útil para sistemas legacy o para una visión de flujo de datos complementaria.

---

## 📝 Preguntas de repaso

1. ¿Qué es un DFD de nivel 0 y qué información contiene? ¿Por qué se llama "diagrama de contexto"?
2. ¿Cuál es la diferencia entre el análisis estructurado y el análisis orientado a objetos?
3. ¿Qué modela un diagrama de estado y qué elementos lo componen?
4. ¿Cuáles son los pasos para crear un modelo de comportamiento?
5. ¿En qué se diferencian un diagrama de secuencia y uno de comunicación? ¿Cuándo usar cada uno?
6. ¿Qué información debe incluir un patrón documentado?
7. ¿Puede un DFD mostrar condiciones (if/else)? ¿Por qué sí o por qué no?
8. ¿Qué tipo de análisis usa los DFD como herramienta principal?

---

## 📌 Resumen en una página

> El Tema 6 completa el modelo de requisitos con el **modelado orientado al flujo** (DFD), el **modelado del comportamiento** y los **patrones**. El DFD sigue el esquema Entrada-Proceso-Salida y se construye jerárquicamente: el nivel 0 o diagrama de contexto muestra el sistema como una caja negra con sus interfaces externas; los niveles siguientes descomponen cada proceso en mayor detalle. Los **diagramas de estado** modelan el flujo de *control*, definiendo los estados de un objeto y las transiciones entre ellos disparadas por eventos. El **modelo de comportamiento** se construye evaluando los casos de uso, identificando eventos, creando diagramas de secuencia por cada escenario y consolidando en un diagrama de estado. Los **diagramas de secuencia** muestran los mensajes entre objetos en orden temporal (eje vertical); los **diagramas de comunicación** muestran la misma información como red de objetos con mensajes numerados. Ambos son equivalentes pero con énfasis diferente. Finalmente, los **patrones** son soluciones documentadas a problemas recurrentes: problema, solución, suposiciones y restricciones.
>