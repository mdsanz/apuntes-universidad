# Historia de los Sistemas Operativos

> **Unidad:** Tema 2  
> **Materia:** Administración de Sistemas Operativos — UNIR México  
> **Palabras clave:** `generaciones`, `mainframe`, `UNIX`, `Linux`, `Windows`, `multiprogramación`, `tiempo compartido`, `MULTICS`, `MS-DOS`, `kernel`, `POSIX`, `distribución`

---

## 🎯 Objetivo de la clase

Situar la historia de los sistemas operativos dentro de la evolución del hardware, identificando las cuatro generaciones y sus innovaciones clave. Conocer en detalle la historia de Linux y Windows, los dos SO protagonistas del curso.

---

## 🧠 Conceptos principales

### Las cuatro generaciones en perspectiva

La historia de los SO sigue de cerca la historia del hardware. Se distinguen **cuatro generaciones**, cada una marcada por una innovación tecnológica que cambió radicalmente qué era posible hacer con un SO.

```
  1ª Gen         2ª Gen         3ª Gen         4ª Gen
  1945-55        1955-65        1965-80        1980-hoy
     │              │              │              │
  Válvulas      Transistores  Circ. integrados  Chips VLSI
  de vacío       (tubos)       (mainframes)     (PCs, móviles)
     │              │              │              │
  Sin SO        Lotes y       Multiprogr.      PCs, GUIs,
  real          mainframes    Tiempo comp.     Internet, móviles
```

> 📝 **Lectura:** Cada generación resuelve el problema de eficiencia de la anterior. El hilo conductor es siempre: ¿cómo aprovechar mejor el tiempo del procesador?

---

## 🕰️ 1ª Generación (1945–1955): Válvulas de vacío

### Hardware
- Máquinas construidas con **válvulas de vacío** (miles de ellas por máquina)
- Ocupaban salas enteras; disipaban calor enorme; muy poco fiables
- Ciclos de instrucción que tardaban **segundos**
- Pioneros: Howard Aiken, John von Neumann, Eckert & Mauchley (USA), Konrad Zuse (Alemania)

### Sistema operativo
- **Prácticamente inexistente.** Cada usuario reservaba tiempo en la máquina y la programaba directamente con conmutadores e interruptores
- Programación en **lenguaje máquina** de cada equipo
- Gran avance de la época: la creación de las **tarjetas perforadas** para introducir programas sin manipular físicamente los conmutadores

> 💡 **Intuición:** Era como conducir un auto sin volante: cada persona tenía que conectar cables directamente al motor. No había capa de abstracción alguna.

---

## ⚙️ 2ª Generación (1955–1965): Transistores y Mainframes

### Hardware
- Sustitución de válvulas por **transistores**: menor tamaño, menor consumo, más fiables
- Aparece el concepto de **mainframe**: grandes máquinas que sí podían fabricarse industrialmente
- Lenguajes de programación: **FORTRAN** y **ensamblador**
- Entrada: pilas de **tarjetas perforadas**

### Sistema operativo y avances
- Aparecen los primeros SO primitivos orientados al **trabajo por lotes** (*batch processing*)
- Máquinas menos potentes leían las tarjetas y las pasaban a **cintas magnéticas** para que las grandes las procesaran
- Se desarrollan los primeros **comandos** para automatizar operaciones repetidas → antecesores del shell

> 💡 **Intuición:** Imagina dejar tu ropa en la tintorería en un lote. Todos los trabajos se acumulan y se procesan juntos, sin interacción directa.

| Característica | 1ª Generación | 2ª Generación |
|---|---|---|
| Hardware | Válvulas de vacío | Transistores |
| Tamaño | Salas enteras | Grandes salas |
| Programación | Conmutadores / lenguaje máquina | FORTRAN / ensamblador |
| Entrada | Conmutadores → tarjetas perforadas | Tarjetas perforadas → cintas |
| SO | Sin SO | Procesamiento por lotes |

---

## 🔄 3ª Generación (1965–1980): Circuitos Integrados y Multiprogramación

### El problema al inicio de los 60

Existían dos tipos de mercado incompatibles:
- Máquinas **científicas/ingenieriles**: grandes, caras, para cálculo numérico
- Máquinas **comerciales**: pequeñas, para procesar texto (bancos, aseguradoras)

### IBM System/360: la solución unificadora

IBM creó la serie **System/360**, una familia de máquinas con **circuitos integrados** que:
- Compartían la misma arquitectura e instrucciones
- Un programa escrito para una podía ejecutarse en todas
- Cubrían tanto el mercado científico como el comercial

```
  IBM System/360 → 370 → 4300 → 3080 → 3090 → Z (serie actual)
  
  ✅ Primera familia compatible entre sí
  ✅ Todavía en uso en centros de datos actuales
```

### OS/360 y la multiprogramación

El SO diseñado para estas máquinas, el **OS/360**, fue un hito por introducir la **multiprogramación**:

**Problema anterior:** El procesador estaba inactivo ~90% del tiempo esperando E/S.

**Solución:** Particionar la memoria para tener **varios trabajos cargados simultáneamente**. Mientras uno espera E/S, otro usa la CPU.

```
  SIN multiprogramación:
  [Trabajo A]──────────────[espera E/S]──────────────[fin]
  CPU:        ████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░

  CON multiprogramación:
  [Trabajo A]───[espera E/S]──────────────────[reanuda]
  [Trabajo B]           ──────────────[fin]
  [Trabajo C]                    ───────────────[...]
  CPU:        ████████████████████████████████████████
```

> 📝 **Lectura:** La multiprogramación es el primer paso para hacer que la CPU nunca esté ociosa. El OS/360 fue el primer SO en implementarla a gran escala.

### Spooling

> **Spooling** (*Simultaneous Peripheral Operations On Line*): capacidad de leer programas **directamente desde tarjetas perforadas** sin intermediarios, enviando la E/S a disco mientras se procesa otra cosa.

Esto eliminó prácticamente las cintas magnéticas como intermediario.

### Tiempo compartido (*time-sharing*)

La siguiente innovación: en vez de procesar lotes, dar a **cada usuario un terminal interactivo**. Como los humanos tecleamos muy lento en comparación con la CPU, el procesador puede atender a muchos usuarios secuencialmente sin que ninguno note retraso.

### MULTICS: el proyecto visionario

MIT + Bell Labs + General Electric crearon **MULTICS** (*MULTiplexed Information and Computing Service*): un superordenador que atendería a cientos de usuarios simultáneos, como una red eléctrica de computación.

- **No tuvo el éxito esperado** comercialmente, pero sí fue un laboratorio de ideas
- General Electric abandonó el hardware
- Las últimas máquinas MULTICS se apagaron en el año **2000**

> 💡 **Intuición:** La idea de MULTICS es exactamente lo que hoy llamamos "cloud computing": acceder a potencia de cómputo ilimitada desde cualquier lugar. Llegó 40 años antes de tiempo.

### Nacimiento de UNIX

**Ken Thompson** (ex Bell Labs, proyecto MULTICS) tomó un PDP-7 y desarrolló una **versión mínima de MULTICS para un solo usuario**: **UNIX**.

```
  MULTICS (1965) ──[simplificado]──▶ UNIX (~1969)
  (para cientos de usuarios)         (para un usuario)
  
  Ken Thompson + Dennis Ritchie (Bell Labs)
```

UNIX se hizo popular en universidades, empresas y agencias gubernamentales porque su **código fuente era abierto y modificable**.

#### El árbol de versiones de UNIX

```
              UNIX (Bell Labs)
                    │
        ┌───────────┴───────────┐
    System V                   BSD
    (AT&T)           (UC Berkeley)
        │                   │
   Muchos derivados    FreeBSD, OpenBSD, NetBSD
   comerciales             │
                        macOS (hereda de BSD)
```

> **POSIX** (IEEE): estándar que define un **subconjunto mínimo de llamadas al sistema** que deben funcionar igual en todos los UNIX. Tan útil que otros SO (incluido Windows) se unieron a él.

### Los minicomputadores DEC PDP

DEC lanzó el **PDP-1** (1961): 20 veces más barato que un IBM 7094, casi tan rápido para ingeniería. Evolucionó hasta el **PDP-11**. Fue el hardware donde nació UNIX.

---

## 💻 4ª Generación (1980–hoy): PCs, GUIs e Internet

### La revolución del microprocesador

Los **circuitos integrados de alta escala** (chips con miles de transistores por cm²) hicieron posibles los **computadores personales** (PCs).

**Procesadores clave y sus SO:**

| Procesador | Bits | SO principal |
|---|---|---|
| Intel 8080 / Zilog Z80 | 8 | CP/M (Digital Research) |
| Motorola 6502 | 8 | Apple DOS (base del Apple II) |
| Intel 8086 | 16 | MS-DOS (Microsoft) |
| Intel 80386 | 32 | Windows NT, Linux |

### La historia de Microsoft y MS-DOS

- IBM necesitaba un SO para su IBM PC (basado en el 8086)
- Microsoft **compró DOS a su creador** y lo rebautizó **MS-DOS**
- MS-DOS dominó el mercado de los IBM PC durante años
- Microsoft también vendía el intérprete BASIC para CP/M antes de MS-DOS

### La GUI: de Stanford al Macintosh

La **interfaz gráfica de usuario (GUI)** fue inventada por **Doug Engelbart** en Stanford. Todos los SO de la época (CP/M, MS-DOS, Apple DOS) usaban solo línea de comandos.

**Steve Jobs** vio en la GUI (ventanas, iconos, menús, ratón) la oportunidad de crear el primer PC verdaderamente amigable:

| Año | Hito |
|-----|------|
| 1984 | Apple lanza el **Macintosh** (procesador Motorola 68000, 64KB ROM) |
| 2001 | Apple lanza **Mac OS X** (basado en el UNIX de Berkeley) |
| 2005 | Apple migra a procesadores **Intel** |

Microsoft creó **Windows** para competir con Macintosh. Al principio era solo una capa gráfica sobre MS-DOS de 16 bits; con **Windows NT** se convirtió en un SO completo de 32 bits.

### Sistemas operativos distribuidos y de red

Desde mediados de los 80 se desarrollaron SO donde los usuarios acceden a recursos de red **sin ser conscientes de ello**. Objetivo: transparencia total sobre qué máquina ejecuta qué tarea.

### Clasificación actual de los SO

| Categoría | Ejemplos |
|---|---|
| **Mainframes** | IBM OS/390 zSeries, Siemens Fujitsu BS 2000/OSD |
| **Servidores** | UNIX, Windows Server |
| **PCs** | Windows, Linux, macOS |
| **Tiempo real** | VxWorks, QNX, Embedded Linux |
| **Empotrados** | VxWorks, QNX, Windows CE |
| **Móviles** | Android, iOS, Symbian, Windows Mobile |
| **Tarjetas inteligentes** | Tarjetas con JVM |

> **Sistema de tiempo real:** garantiza un **tiempo de respuesta mínimo** ante eventos externos (señales industriales, sensores de aviones). La latencia máxima es un requisito hard.

> **Sistema empotrado (*embedded*):** SO que vive **dentro de un aparato** (lavadora, router, automóvil). Tamaño y recursos mínimos; sistema cerrado dedicado a una única función.

---

## 🐧 Historia de Linux

### Origen

**1991** — **Linus Torvalds**, estudiante finlandés, escribe un kernel pequeño para el **Intel 80386** (primer procesador de 32 bits de Intel para PCs) y lo llama **Linux**.

```
  UNIX (1969)
     │
  Ken Thompson  ──────────────────────────────────▶
     │
  Linus Torvalds aprende UNIX en la universidad
     │
  1991: Linux 0.01 (kernel para 80386, sin red)
     │
  1994: Linux 1.0 (red, TCP/IP, sockets BSD, SCSI)
     │
  1995: Linux 1.2 (PCI, soporte múltiples arquitecturas)
     │
  2000+: Dominio en servidores, Android, supercomputadoras
```

Desde el principio, el **código fuente fue publicado gratis en Internet**, haciendo de Linux un proyecto colaborativo global.

### Kernel Linux vs. Sistema Linux vs. Distribución Linux

```
┌─────────────────────────────────────────────────────┐
│                  DISTRIBUCIÓN LINUX                  │
│  (Ubuntu, Fedora, Debian, Arch, RHEL…)              │
│  ┌───────────────────────────────────────────────┐  │
│  │              SISTEMA LINUX                    │  │
│  │  (herramientas GNU, librerías, shell, GUIs…)  │  │
│  │  ┌─────────────────────────────────────────┐  │  │
│  │  │           KERNEL LINUX                  │  │  │
│  │  │  (gestión procesos, memoria, drivers…)  │  │  │
│  │  └─────────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────┘
```

> **Distribución Linux:** incluye el kernel + herramientas estándar + gestor de paquetes + software adicional (oficina, multimedia, navegadores, etc.). Simplifica instalación y actualizaciones.

### Versiones del kernel (convención de numeración)

```
  Número de versión:  X . Y . Z
                      │   │   └─ Parches de corrección
                      │   └───── Menor: PAR = estable | IMPAR = desarrollo
                      └───────── Mayor

  Ejemplo:
  1.2 (par)  → kernel estable para distribución
  1.3 (impar) → kernel de desarrollo con nuevas funcionalidades
```

### Hitos del kernel Linux

| Versión | Fecha | Novedad principal |
|---|---|---|
| 0.01 | Mayo 1991 | Primer kernel: solo x86, sin red, sin CD-ROM |
| 1.0 | Marzo 1994 | TCP/IP, sockets BSD, sistema de archivos mejorado, SCSI |
| 1.2 | Marzo 1995 | Bus PCI, modo virtual 8086 (emulación DOS), IPX, cortafuegos |

**Linux 1.0 también incorporó:**
- Memoria virtual con paginación a ficheros de intercambio (*swap*)
- IPC estilo System V: **memoria compartida, semáforos y colas de mensajes**
- Carga y descarga dinámica de módulos del kernel

---

## 🪟 Historia de Windows

### Los orígenes: NT (1988)

Microsoft abandona OS/2 en 1988 y comienza a desarrollar **NT** (*New Technology*) de forma independiente, con el objetivo de crear un SO portátil que soportara múltiples APIs.

Contratan a **Dave Cutler** (creador del VAX/VMS de DEC) para liderar el proyecto.

### Evolución de Windows NT → hoy

```
  OS/2 (1987, IBM + Microsoft)
       │
       └── Microsoft se sale en 1990
  
  NT (1988, Microsoft solo)
       │
  Windows NT 3.1 (primera versión NT, API Win32)
       │
  Windows NT 4.0 (GUI de Win95, componentes gráficos en kernel)
       │
  Windows 2000 (Active Directory, plug and play, solo Intel)
       │
  Windows XP (oct 2001: NT al mercado de usuarios finales, 64 bits)
       │
  Windows Vista (ene 2007: 3D, DirectX 10 — criticado por pesado)
       │
  Windows 7 (oct 2009: Vista mejorado, planificador multiprocesador)
```

### Windows versión a versión

#### Windows NT 3.1 / 4.0
- NT 3.1: primera versión, API **Win32**
- NT 4.0: adoptó la GUI de Windows 95; **movió el subsistema gráfico al kernel** para mejorar rendimiento (pero redujo la fiabilidad)

#### Windows 2000 (feb 2000)
- Solo compatible con Intel (abandona otras arquitecturas)
- Añadió: **Active Directory** (servicio de directorio basado en X.500), plug and play, sistema de ficheros distribuido, mejor soporte de red y portátiles

#### Windows XP (oct 2001)
- Llevó la arquitectura NT al **mercado de consumo masivo** (sustituye a Win 95/98)
- **Primera versión de Windows en 64 bits**
- Arquitectura cliente-servidor tipo Mach para subsistemas (Win32, POSIX)
- Sistema multiusuario con Terminal Server
- Desde el **Service Pack 2** (2004) se convirtió en el SO dominante de Microsoft

#### Windows Vista (ene 2007)
- Nuevo escritorio 3D (Aero), DirectX 10
- **Muy criticado:** ocupaba demasiado disco, requería mucha RAM, incompatible con muchas apps
- La versión x64 fue la mejor recibida

#### Windows 7 (oct 2009)
- Refinamiento de Vista SP2
- Kernel reestructurado con **planificador optimizado para multiprocesador**
- Modelo gráfico WDDM 1.1, DirectX 11
- UAC (gestor de cuentas de usuario) menos intrusivo

### El caso de OS/2

| Aspecto | Detalle |
|---------|---------|
| **Creadores** | IBM + Microsoft (anunciado dic 1987) |
| **Objetivo** | Superar a DOS y Windows |
| **Fortalezas** | Multitarea, multihilos, protección de memoria, compatible con DOS |
| **Versiones clave** | 1.0 (sin GUI), 1.1 (con GUI), Warp 4 (1996: Java + voz) |
| **Fracaso** | Microsoft abandonó el proyecto en 1990; Windows 3 fue un éxito inesperado |
| **Dato curioso** | La plataforma NT de Windows (base de XP) fue originalmente llamada **OS/2 3** |

---

## 📐 Definiciones formales

| Término | Definición |
|---------|-----------|
| **Mainframe** | Computadora de gran tamaño y capacidad, diseñada para procesar enormes volúmenes de datos; usada en entornos corporativos y gubernamentales |
| **Multiprogramación** | Técnica que carga varios trabajos en memoria simultáneamente para mantener la CPU ocupada mientras unos esperan E/S |
| **Tiempo compartido** | Técnica en la que múltiples usuarios tienen terminales interactivos; la CPU los atiende secuencialmente tan rápido que nadie nota espera |
| **Spooling** | Lectura directa de programas desde tarjetas perforadas a disco, eliminando la necesidad de cintas magnéticas intermedias |
| **MULTICS** | MULTiplexed Information and Computing Service; proyecto pionero de time-sharing para cientos de usuarios simultáneos (MIT/Bell/GE) |
| **UNIX** | SO creado por Ken Thompson en Bell Labs (~1969), basado en una versión simplificada de MULTICS; origen de Linux, macOS y muchos otros |
| **POSIX** | Portable Operating System Interface; estándar IEEE que define llamadas al sistema comunes a todos los UNIX |
| **Distribución Linux** | Empaquetado completo de: kernel Linux + herramientas del sistema + gestor de paquetes + software adicional |
| **NT** | New Technology; arquitectura de SO de Microsoft iniciada en 1988, base de Windows 2000, XP y todos los Windows modernos |
| **Win32 API** | Interfaz de programación de 32 bits de Windows; adoptada como entorno nativo de NT reemplazando la API de OS/2 |
| **Active Directory** | Servicio de directorio distribuido de Microsoft (basado en X.500), introducido en Windows 2000 |
| **Sistema empotrado** | SO que corre dentro de un aparato (móvil, lavadora, automóvil) dedicado a una función específica con recursos mínimos |
| **Sistema de tiempo real** | SO que garantiza tiempos de respuesta máximos ante eventos externos; crítico en industria, aviación, medicina |
| **GUI** | Graphical User Interface; interfaz gráfica con ventanas, iconos, menús y ratón; inventada por Doug Engelbart en Stanford |
| **CP/M** | Control Program for Microcomputers; SO dominante para PCs de 8 bits (Intel 8080, Z80) antes de MS-DOS |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **UNIX no fue creado desde cero:** es una simplificación de MULTICS para un solo usuario.
- ❗ **Linux no es UNIX:** es un sistema similar a UNIX pero su kernel fue escrito **enteramente desde cero** por Linus Torvalds y la comunidad.
- ✅ **POSIX** es la "lingua franca" de los sistemas tipo UNIX. Programas escritos con la API POSIX son portables entre versiones de UNIX y Linux.
- ❗ **OS/2 no fracasó por problemas técnicos:** era un buen SO. Fracasó porque **Windows 3.x tuvo un éxito comercial inesperado** y Microsoft retiró su apoyo.
- 🔁 **Windows NT** fue co-desarrollado como "OS/2 3" antes de que Microsoft lo renombrara. La base de Windows XP viene directamente de ahí.
- ✅ Los números **impares** de versión del kernel Linux son de **desarrollo**; los **pares** son **estables**.
- ❗ La versión 1.0 de Linux (1994) ya incluía IPC estilo UNIX System V: memoria compartida, semáforos y colas de mensajes.
- 🔁 **Mac OS X** (2001) está basado en el UNIX de Berkeley (BSD), por eso es compatible con POSIX.

---

## 🔗 Conexiones con otros temas

- **Tema 1 (Concepto de SO):** La historia muestra cómo cada generación fue implementando los conceptos vistos: abstracción, multiprogramación, tiempo compartido, seguridad.
- **Shell (tema siguiente):** Los comandos batch de la 2ª generación son los antecesores directos del shell interactivo.
- **Gestión de procesos:** La multiprogramación del OS/360 es el origen del scheduler/planificador moderno.
- **Linux en la práctica:** Las versiones del kernel que estudiamos en el curso son herederas directas de lo que empezó en 1991.

---

## 📝 Preguntas de repaso

1. ¿Por qué los primeros ordenadores de la 1ª generación no necesitaban SO?
2. ¿Qué problema resolvió la multiprogramación? ¿Por qué el procesador estaba inactivo el 90% del tiempo antes de ella?
3. ¿Qué fue MULTICS y por qué no tuvo el éxito esperado? ¿Qué legado dejó?
4. ¿Cómo nació UNIX? ¿Cuál es la relación entre MULTICS y UNIX?
5. ¿Qué es POSIX y para qué sirve? ¿Por qué fue necesario crearlo?
6. ¿Cuál es la diferencia entre el **kernel Linux**, el **sistema Linux** y una **distribución Linux**?
7. ¿Por qué fracasó OS/2 siendo técnicamente superior a Windows?
8. ¿Qué convención se usa para numerar las versiones del kernel Linux? ¿Cómo distingues una versión estable de una de desarrollo?
9. ¿Qué aportó Windows XP que no tenían versiones anteriores de Windows?
10. ¿Cuál es la diferencia entre un sistema de tiempo real y un sistema empotrado?

---

## 📌 Resumen en una página

La historia de los SO sigue la evolución del hardware en **cuatro generaciones**. La 1ª (válvulas, 1945–55) no tenía SO real: los programadores interactuaban directamente con el hardware. La 2ª (transistores, 1955–65) trajo los mainframes y el procesamiento por lotes. La 3ª (circuitos integrados, 1965–80) fue la más revolucionaria: IBM unificó su línea con el System/360 y el OS/360 popularizó la **multiprogramación**; el proyecto MULTICS fracasó comercialmente pero sembró ideas que germinarían en **UNIX** (Ken Thompson, Bell Labs, ~1969), el ancestro de Linux y macOS. La 4ª generación (chips VLSI, 1980–hoy) trajo los PCs con MS-DOS, la GUI del Macintosh y Windows. **Linux** nació en 1991 cuando Linus Torvalds publicó un kernel para 80386 compatible con UNIX, que la comunidad global hizo crecer hasta convertirlo en el SO más usado en servidores y dispositivos. **Windows** nació de la arquitectura NT (1988), pasando por XP (primera versión de 64 bits, base actual de Windows) hasta Windows 7. OS/2 fue un SO técnicamente sólido que IBM y Microsoft crearon juntos pero que fracasó porque Windows 3 se impuso en el mercado. La clasificación actual de SO incluye mainframes, servidores, PCs, tiempo real, empotrados y móviles, cada uno optimizado para requisitos radicalmente distintos.