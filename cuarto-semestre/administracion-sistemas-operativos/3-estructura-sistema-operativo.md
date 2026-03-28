# Estructura de un Sistema Operativo

> **Unidad:** Tema 3  
> **Materia:** Administración de Sistemas Operativos — UNIR México  
> **Palabras clave:** `monolítico`, `capas`, `microkernel`, `virtualización`, `exokernel`, `cliente-servidor`, `distribuido`, `máquina virtual`, `JVM`, `THE`

---

## 🎯 Objetivo de la clase

Conocer los principales modelos de arquitectura interna bajo los cuales se diseñan los sistemas operativos: monolítico, en capas, virtualización, exokernel, cliente-servidor y distribuido. Entender las ventajas, desventajas y casos de uso de cada modelo.

---

## 🧠 Visión general: los modelos de estructura

No todos los SO tienen la misma organización interna. La elección de arquitectura impacta directamente en la eficiencia, la seguridad, el mantenimiento y la extensibilidad del sistema.

```
  MODELOS DE ESTRUCTURA DE UN SO
  ┌──────────────────────────────────────────────────────────────┐
  │                                                              │
  │  Monolítico  →  En capas  →  Microkernel  →  Distribuido     │
  │                     │                                        │
  │               Virtualización  ←─── Exokernel                 │
  │                                                              │
  │  (menos estructurado)          (más estructurado/modular)    │
  └──────────────────────────────────────────────────────────────┘
```

---

## 🧱 Sistema Monolítico

### Descripción

Es el modelo **más extendido** en la práctica, aunque paradójicamente es el modelo de la **falta de arquitectura**. El kernel es un único bloque ejecutable donde todas las rutinas pueden invocarse entre sí sin restricción alguna.

El ejemplo más conocido es el **kernel de Linux** (por ser código abierto).

### Estructura interna

```
  ┌─────────────────────────────────────────────────┐
  │              KERNEL MONOLÍTICO                  │
  │                                                 │
  │  rutina_A() ◀──────────────────▶ rutina_D()    │
  │      │               ▲               │          │
  │      ▼               │               ▼          │
  │  rutina_B() ◀────────┼───────▶ rutina_E()      │
  │      │               │               │          │
  │      └───────▶ rutina_C() ◀─────────┘          │
  │                                                 │
  │  Cualquier rutina puede llamar a cualquier otra │
  │  en cualquier momento. Sin ocultamiento.        │
  └─────────────────────────────────────────────────┘
```

> 📝 **Lectura:** Es como una oficina donde todos los empleados pueden interrumpirse entre sí en cualquier momento. Eficiente, pero caótico para mantener.

### Compilación y enlace

La construcción de una nueva versión del kernel se reduce a:
1. Compilar cada procedimiento/función (cada fichero `.c`)
2. Unirlos en **un solo bloque ejecutable** con el enlazador (*linker*)

### Niveles de protección (estructura mínima)

Aun los sistemas monolíticos mantienen una separación mínima de dos niveles:

| Nivel de núcleo | Nivel de usuario |
|---|---|
| Máxima protección; acceso al hardware, planificación y memoria | Código de las aplicaciones; sin acceso directo a dispositivos |

La comunicación entre niveles se realiza mediante **llamadas al sistema** (mecanismo similar a las interrupciones de hardware).

### Ventajas y desventajas

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Alta eficiencia: acceso directo entre cualquier parte del núcleo sin pasos intermedios | Difícil de mantener: actualizar una función requiere saber cuántas otras la invocan |
| Código más simple de construir inicialmente | Sin ocultamiento de información ni protección interna |
| Ampliamente probado en la práctica (Linux, BSD) | Un fallo en cualquier rutina puede comprometer todo el kernel |

> 💡 **Intuición:** Un monolito es como un programa gigante: todo junto, todo accesible. Rápido de ejecutar, pesadilla de depurar.

---

## 🏛️ Sistema en Capas

### Descripción

En 1968, **E. W. Dijkstra** y sus alumnos construyeron el **sistema operativo THE**: el primer SO organizado como una jerarquía de capas donde **cada capa solo puede usar la capa inmediatamente inferior**.

### Las 6 capas del sistema THE

```
  ┌──────────────────────────────────────────────┐
  │  Capa 6 — Intérprete de comandos del operador│  ← más externa
  ├──────────────────────────────────────────────┤
  │  Capa 5 — Aplicaciones de usuario            │
  ├──────────────────────────────────────────────┤
  │  Capa 4 — E/S de dispositivos externos       │
  ├──────────────────────────────────────────────┤
  │  Capa 3 — E/S de consola                     │
  ├──────────────────────────────────────────────┤
  │  Capa 2 — Gestión de memoria                 │
  ├──────────────────────────────────────────────┤
  │  Capa 1 — Planificación de procesos,         │  ← más interna
  │           temporizadores e interrupciones    │
  └──────────────────────────────────────────────┘
           ↑ cada capa usa solo la inferior ↑
```

> 📝 **Lectura del diagrama:** La capa 1 gestiona el hardware directamente. Cada capa superior puede programar su procedimiento "como si fuera el único en ejecutarse en el sistema", porque la capa inferior ya abstrae toda la complejidad.

### MULTICS: variante en anillos concéntricos

MULTICS usó un diseño similar pero en forma de **anillos concéntricos**:
- Los anillos **más internos** tienen **mayor privilegio**
- Un anillo puede invocar código de un anillo más interno via llamadas al sistema
- El hardware también controlaba la **segmentación de memoria**: el código de un anillo no puede leer, escribir o ejecutar fuera de su segmento

```
  ┌──────────────────────────────────────┐
  │  Anillo 3 (usuarios - menos priv.)   │
  │  ┌────────────────────────────────┐  │
  │  │  Anillo 2 (servicios)          │  │
  │  │  ┌──────────────────────────┐  │  │
  │  │  │  Anillo 1 (SO)           │  │  │
  │  │  │  ┌────────────────────┐  │  │  │
  │  │  │  │  Anillo 0 (kernel) │  │  │  │
  │  │  │  │  máximo privilegio │  │  │  │
  │  │  │  └────────────────────┘  │  │  │
  │  │  └──────────────────────────┘  │  │
  │  └────────────────────────────────┘  │
  └──────────────────────────────────────┘
```

### Ventajas y desventajas

| ✅ Ventajas | ❌ Desventajas |
|---|---|
| Cada capa puede probarse y verificarse de forma independiente | Definir los límites de cada capa es difícil en la práctica |
| Mayor ocultamiento de información que el monolítico | Múltiples cruces de capas pueden reducir la eficiencia |
| Facilita el mantenimiento y la depuración | No siempre es claro dónde pertenece cada función |

---

## 🖥️ Virtualización

### Origen: IBM CP/CMS → VM/370

IBM separó las dos tareas principales del SO:
1. Gestionar la multiprogramación
2. Proporcionar una interfaz más simple del hardware a los programadores

El resultado fue el **monitor de la máquina virtual** (VMM o *hypervisor*):

> **Monitor de máquina virtual:** componente que se ejecuta directamente sobre el hardware real, gestionando la multiprogramación, pero presentando al resto del sistema **copias independientes de la misma máquina real** (máquinas virtuales).

### Qué ofrece una máquina virtual

A diferencia de la "máquina extendida" (abstracción más simple), la máquina virtual ofrece **una copia de la máquina real completa**, incluyendo:
- Interrupciones
- E/S directa
- Llamadas al sistema
- Todo el hardware subyacente

Esto permite ejecutar **sistemas operativos distintos** sobre la misma máquina virtual.

```
  ┌──────────┐  ┌──────────┐  ┌──────────┐
  │  Linux   │  │ Windows  │  │  BSD     │   ← SO invitados
  ├──────────┤  ├──────────┤  ├──────────┤
  │  VM 1    │  │  VM 2    │  │  VM 3    │   ← Máquinas virtuales
  └────────────────────────────────────────┘
  ┌──────────────────────────────────────────┐
  │    Monitor / Hypervisor (VMM)            │   ← Capa de virtualización
  ├──────────────────────────────────────────┤
  │    Hardware físico                       │
  └──────────────────────────────────────────┘
```

### Ejemplo moderno: Pentium y MS-DOS

Los procesadores **Pentium** incluían un **modo virtual 8086**: emulaban un procesador 8086 con 16 bits y 1 MB de direccionamiento. Windows usaba esto para ejecutar programas MS-DOS dentro de una máquina virtual.

### Aplicaciones económicas de la virtualización

En vez de dedicar un servidor físico por servicio, se pueden crear **múltiples máquinas virtuales** (con herramientas como VMware o Virtual PC) sobre un único servidor físico. Esto reduce drásticamente costes de hardware.

### La JVM: virtualización para portabilidad

Java creó la **Java Virtual Machine (JVM)** con un objetivo diferente: **portabilidad**, no eficiencia de hardware.

```
  Código Java
      │
      ▼
  Compilador Java
      │
      ▼
  Bytecode (.class)  ← No es código nativo, es código intermedio
      │
      ├──▶ JVM Windows  ──▶ Ejecuta en Windows
      ├──▶ JVM Linux    ──▶ Ejecuta en Linux
      └──▶ JVM macOS    ──▶ Ejecuta en macOS
  
  "Write once, run anywhere"
```

**Ventajas de la JVM:**
- El mismo bytecode funciona en cualquier plataforma con su JVM
- La JVM puede analizar el código antes de ejecutarlo para detectar problemas de seguridad

---

## ⚡ Exokernel

### Descripción

El exokernel lleva la idea de la virtualización un paso más allá: en vez de dar a cada proceso una **copia exacta** de la máquina real (VM/370) o una copia de otra máquina (modo 8086), el exokernel crea una copia con **solo un subconjunto de los recursos**.

> **Exokernel:** programa que reside en el nivel más cercano al hardware y se dedica a **asignar recursos reales a cada máquina virtual**, garantizando que cada una solo acceda a su parte correspondiente.

### Comparación de modelos de virtualización

```
  VM/370 (máquina virtual clásica):
  Proceso A → copia COMPLETA de la máquina real
  Proceso B → copia COMPLETA de la máquina real
  
  Exokernel:
  Proceso A → subconjunto de recursos (p.e., 30% CPU, 256MB RAM, disco X)
  Proceso B → subconjunto de recursos (p.e., 50% CPU, 512MB RAM, disco Y)
  
  Exokernel garantiza separación y asignación justa de recursos reales.
```

### Ventajas

- **Más eficiente** que la máquina virtual completa en la organización de recursos
- Permite mantener el código del SO en **espacio de usuario**, eliminando la necesidad de que el kernel gestione la multiprogramación directamente
- Más flexible: cada proceso puede tener su propio "mini-SO" adaptado a sus necesidades

---

## 🔄 Modelo Cliente-Servidor (Microkernel)

### La tendencia central del diseño moderno

La evolución natural del diseño de SO tiende a **aislar el kernel** dejando en él solo lo estrictamente necesario, y mover todo lo demás al espacio de usuario.

> **Microkernel:** kernel que solo contiene los servicios imprescindibles para el funcionamiento básico del sistema (gestión de procesos, comunicación entre procesos, gestión básica de memoria). Todo lo demás corre como procesos de usuario.

### Arquitectura

```
  ┌─────────────────────────────────────────────────────────┐
  │                  ESPACIO DE USUARIO                     │
  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐    │
  │  │ Servidor │ │ Servidor │ │ Servidor │ │ Cliente  │    │
  │  │ ficheros │ │  memoria │ │  red     │ │ (app)    │    │
  │  └────┬─────┘ └────┬─────┘ └────┬─────┘ └────┬─────┘    │
  │       │            │            │             │         │
  └───────┼────────────┼────────────┼─────────────┼─────────┘
          │            │            │             │
  ┌───────▼────────────▼────────────▼─────────────▼─────────┐
  │                    MICROKERNEL                          │
  │  (IPC, planificación básica, gestión mínima de memoria) │
  ├─────────────────────────────────────────────────────────┤
  │                      HARDWARE                           │
  └─────────────────────────────────────────────────────────┘
```

### Funcionamiento cliente-servidor

En un instante dado:
- Un **componente que necesita un servicio** actúa como **cliente**
- El componente que provee el servicio actúa como **servidor**
- El **microkernel** es el gestor de los **canales de comunicación** entre ellos

Los roles no son fijos: un mismo componente puede ser cliente de uno y servidor de otro.

### Beneficios de aislar en espacio de usuario

- Un fallo en un servidor (p.e., el servidor de ficheros) **no derrumba todo el sistema**
- Cada componente es más pequeño → **más fácil de probar y mantener**
- Mayor seguridad: los componentes no tienen acceso al hardware directamente

### Reto: acceso a dispositivos de E/S

Algunos servidores necesitan escribir en registros de dispositivos. Soluciones posibles:
- Darles permisos limitados para acceder a sus dispositivos específicos
- El microkernel traduce mensajes especiales en comandos para los dispositivos

---

## 🌐 Sistemas Operativos Distribuidos

### Definición

> Un **sistema operativo distribuido** tiene sus componentes repartidos entre **diferentes máquinas físicas** conectadas en red, diseñados para dar al usuario la ilusión de que trabaja con un **único sistema local** transparente.

### Objetivo: transparencia total

El usuario no debe saber:
- En qué máquina física reside su fichero
- Qué procesador de la red ejecuta su tarea
- Cómo se mueven los datos entre nodos

```
  Usuario
     │ ve un solo sistema
     ▼
  ┌────────────────────────────────────────┐
  │       SO DISTRIBUIDO (capa lógica)     │
  ├────────────────────────────────────────┤
  │  Máquina A  │  Máquina B  │  Máquina C │  ← Hardware real
  └─────────────┴─────────────┴────────────┘
          interconectadas por red
```

### Diferencia con sistemas de red

| Sistema de red | Sistema distribuido |
|---|---|
| El usuario sabe que hay varias máquinas | El usuario ve un sistema único transparente |
| Accede explícitamente a recursos remotos | Accede igual que a recursos locales |
| Coordinación manual | Coordinación automática por el SO |

---

## 📐 Comparativa general de modelos

| Modelo | Dónde corre el SO | Eficiencia | Seguridad | Mantenibilidad | Ejemplo |
|--------|---|---|---|---|---|
| **Monolítico** | Todo en kernel | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐ | Linux, BSD |
| **En capas** | Kernel estructurado | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | THE, MULTICS |
| **Máquina virtual** | Hypervisor + SO invitado | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | VM/370, VMware |
| **Exokernel** | Kernel mínimo + libOS | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | MIT Exokernel |
| **Microkernel / C-S** | Mayormente espacio usuario | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Mach, QNX, L4 |
| **Distribuido** | Múltiples máquinas | Variable | Variable | ⭐⭐ | Plan 9, Amoeba |

---

## 📐 Definiciones formales

| Término | Definición |
|---------|-----------|
| **Sistema monolítico** | Kernel en el que todas las rutinas pueden invocarse entre sí sin restricción; se compila como un único bloque ejecutable |
| **Sistema en capas** | SO organizado en niveles jerárquicos donde cada capa solo puede invocar la capa inmediatamente inferior |
| **Sistema THE** | Primer SO en capas (Dijkstra, 1968); 6 capas desde planificación hasta intérprete de comandos |
| **Anillos de MULTICS** | Variante del modelo en capas usando anillos concéntricos de privilegio; más privilegio = anillo más interno |
| **Máquina virtual (VM)** | Copia de la máquina real (o de otra máquina) sobre el hardware, gestionada por un monitor/hypervisor |
| **Monitor de máquina virtual** | Componente que corre directamente sobre el hardware y gestiona múltiples VMs; también llamado hypervisor |
| **Exokernel** | Kernel minimalista que asigna subconjuntos de recursos reales a cada proceso/VM; más eficiente que VM completa |
| **Microkernel** | Kernel que solo contiene IPC, planificación básica y gestión mínima de memoria; todo lo demás en espacio usuario |
| **Modelo cliente-servidor** | Arquitectura donde los componentes del SO se comunican entre sí como clientes y servidores, usando el microkernel como canal |
| **Sistema distribuido** | SO cuyos componentes están en múltiples máquinas físicas en red, presentando al usuario una imagen de sistema único |
| **JVM** | Java Virtual Machine; máquina virtual de software que interpreta bytecode Java, permitiendo portabilidad entre plataformas |
| **Llamada al sistema** | Mecanismo por el cual las aplicaciones solicitan servicios al kernel, usando un mecanismo similar a las interrupciones |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **El modelo monolítico no significa que no tenga ninguna estructura**: sigue existiendo la separación entre nivel de núcleo y nivel de usuario. Lo que no existe es estructura *interna* del kernel.
- ✅ **Linux es monolítico**, pero admite **módulos cargables dinámicamente**, lo que le da algo de la flexibilidad del microkernel sin perder eficiencia.
- ❗ **Exokernel ≠ Máquina virtual**: el exokernel da *subconjuntos* de recursos; la VM clásica da *copias completas* de la máquina.
- ✅ En el modelo **cliente-servidor**, los roles de cliente y servidor son **transaccionales**, no fijos: un componente puede ser servidor para uno y cliente para otro.
- ❗ El **microkernel es más seguro pero más lento** que el monolítico por el overhead de la comunicación entre componentes.
- 🔁 Los **anillos de MULTICS** son el ancestro directo de los niveles de privilegio (rings 0-3) de los procesadores Intel modernos.
- ✅ La **JVM** no es una VM de hardware: no simula una máquina física real, sino una máquina abstracta que interpreta bytecode.

---

## 🔗 Conexiones con otros temas

- **Tema 1 (Concepto de SO):** Las llamadas al sistema mencionadas en el monolítico son el mecanismo central del Tema 1.
- **Tema 2 (Historia):** MULTICS (anillos), VM/370 (virtualización), OS/360 (monolítico), Windows XP (arquitectura cliente-servidor tipo Mach) son todos ejemplos históricos de estos modelos.
- **Virtualización moderna:** VMware, VirtualBox, Docker y los hipervisores de nube (AWS, Azure) son implementaciones contemporáneas del modelo VM.
- **Seguridad:** El microkernel maximiza la seguridad por aislamiento; el monolítico es más vulnerable porque un bug en cualquier parte del kernel puede comprometer todo.

---

## 📝 Preguntas de repaso

1. ¿Por qué se dice que el sistema monolítico es "el modelo de la falta de arquitectura"? ¿Qué ventaja tiene pese a eso?
2. ¿Cuáles son las 6 capas del sistema THE de Dijkstra y qué hace cada una?
3. ¿Qué diferencia hay entre la "máquina extendida" del Tema 1 y la "máquina virtual" de este tema?
4. ¿Qué es el monitor de máquina virtual y qué tareas realiza?
5. ¿Cuál es la relación entre el exokernel y una máquina virtual clásica? ¿En qué se diferencian?
6. En el modelo cliente-servidor aplicado a SO, ¿qué papel juega el microkernel?
7. ¿Qué ventaja de seguridad tiene el microkernel frente al monolítico?
8. ¿Para qué se creó la JVM? ¿Es lo mismo que una máquina virtual de hardware?
9. ¿Cuál es la diferencia entre un sistema operativo de red y un sistema operativo distribuido?
10. ¿Por qué se trasladó el subsistema gráfico al interior del kernel en Windows NT 4.0 y qué consecuencia tuvo?

---

## 📌 Resumen en una página

Los SO pueden organizarse internamente de varias maneras. El **modelo monolítico** (Linux, BSD) es el más común: todas las rutinas del kernel se pueden invocar entre sí libremente; es muy eficiente pero difícil de mantener y sin protección interna. El **modelo en capas** (SO THE, Dijkstra 1968; anillos de MULTICS) estructura el kernel jerárquicamente, donde cada capa solo usa la inferior, lo que facilita la verificación independiente de cada nivel. La **virtualización** (VM/370, VMware) introduce un monitor de máquina virtual que gestiona el hardware real y presenta copias completas de la máquina a los procesos, permitiendo ejecutar múltiples SO distintos sobre el mismo hardware; la JVM aplica el mismo concepto para portabilidad de software. El **exokernel** va más lejos: en vez de copias completas, asigna subconjuntos de recursos reales a cada proceso, siendo más eficiente. El **modelo cliente-servidor** con **microkernel** (Mach, QNX) es el más modular: el kernel se reduce al mínimo (IPC y planificación básica) y el resto del SO corre como procesos de usuario que se comunican entre sí como clientes y servidores; esto maximiza la seguridad y el mantenimiento, aunque añade overhead de comunicación. Finalmente, los **SO distribuidos** reparten sus componentes entre múltiples máquinas físicas en red, presentando al usuario la ilusión de un único sistema local transparente.