# Llamadas al Sistema

> **Unidad:** Tema 4  
> **Palabras clave:** `syscall`, `kernel`, `API`, `POSIX`, `Win32`, `IPC`, `procesos`, `ficheros`, `memoria compartida`

---

## 🎯 Objetivo de la clase

Comprender el mecanismo de las llamadas al sistema (system calls) en los sistemas operativos modernos: qué son, cómo funcionan internamente, cómo se clasifican y cuáles son las más relevantes para la administración de procesos, ficheros y comunicación entre procesos.

---

## 🧠 Conceptos principales

### ¿Qué son las llamadas al sistema?

Las llamadas al sistema (system calls) son la **interfaz** que permite a las aplicaciones de usuario acceder a la funcionalidad del núcleo (kernel) del sistema operativo. Son necesarias porque, por razones de **seguridad y eficiencia**, ciertas tareas están reservadas exclusivamente al SO: acceso a dispositivos externos, gestión de memoria, planificación de la CPU, etc.

> 💡 **Intuición:** Son como el "mostrador de atención" de un banco: tú (la aplicación) no puedes entrar a la bóveda directamente; siempre debes pedirle al empleado (el kernel) que haga el trabajo por ti.

---

### El kernel como conjunto de rutinas

El kernel puede entenderse como un enorme conjunto de rutinas escritas principalmente en **C o C++** (por eficiencia y versatilidad). Cuando se necesita código aún más cercano al hardware, se usa **lenguaje ensamblador**.

---

### API (Application Programming Interface)

Todos los sistemas operativos modernos exponen a los programadores una **API** que especifica:
- El nombre de cada llamada al sistema
- Sus parámetros de entrada
- Los valores que retorna
- Cómo obtener información de errores

Las dos APIs más importantes son:

| API | Sistema operativo |
|-----|-------------------|
| **POSIX** | Linux, macOS |
| **Win32 / Win64** | Windows |
| **Java API** | Máquina Virtual de Java (JVM) |

> ⚠️ Nota: Windows no llama "llamadas al sistema" a sus interfaces, sino **funciones**, porque Win32/Win64 añade una capa extra sobre las verdaderas syscalls del núcleo.

---

### Sistema de soporte en tiempo de ejecución (Runtime Support System)

Para implementar correctamente las llamadas al sistema, los compiladores incluyen una **biblioteca** que enlaza las llamadas de la API con las rutinas del kernel. Su misión es **interceptar las llamadas a funciones** que pertenecen a la API e invocar la rutina correspondiente en el núcleo.

```
Aplicación de usuario
      │
      │  llama a función de API (ej. read())
      ▼
┌─────────────────────────────────┐
│  Runtime Support System         │
│  (biblioteca del compilador)    │
└──────────────┬──────────────────┘
               │  traduce a código numérico → índice en tabla
               ▼
┌─────────────────────────────────┐
│         K E R N E L             │
│  Tabla de punteros a rutinas    │
│  [ 0 ] → rutina_exit            │
│  [ 1 ] → rutina_read            │
│  [ 2 ] → rutina_write           │
│   ...                           │
└─────────────────────────────────┘
```

> 📝 **Lectura:** Cada llamada al sistema tiene un **código numérico** que sirve como índice en una tabla de punteros a las rutinas del kernel. El compilador traduce el nombre de la función API a ese código en tiempo de compilación; en ejecución, el runtime lo usa para invocar la rutina correspondiente.

---

### Paso de parámetros a las llamadas al sistema

Existen tres métodos para pasar parámetros junto con una llamada:

| Método | Descripción | Limitación |
|--------|-------------|------------|
| **Registros** | Se guardan los parámetros en los registros del procesador | Número limitado de registros disponibles |
| **Bloque en memoria** | Se compactan todos los parámetros en una estructura/tabla en memoria; se pasa la dirección del bloque por registro | Sin límite práctico de tamaño |
| **Pila (stack)** | El runtime inserta los parámetros en la pila; el SO los extrae al completar la llamada | Sin límite en número ni tamaño |

---

## 📐 Definiciones formales

| Término | Definición |
|---------|-----------|
| **Llamada al sistema** | Mecanismo que proporciona una forma sencilla para que las aplicaciones de usuario accedan a la funcionalidad del núcleo del SO |
| **Kernel** | Núcleo del SO; conjunto de rutinas escritas en C/C++ o ensamblador que realizan tareas fundamentales del sistema |
| **API** | Application Programming Interface; conjunto de especificaciones que define cómo invocar correctamente las llamadas al sistema |
| **Runtime Support System** | Biblioteca del compilador que intercepta llamadas de la API e invoca las rutinas del kernel |
| **Código numérico de syscall** | Identificador numérico asociado a cada llamada; sirve como índice en la tabla de punteros del kernel |
| **Core dump** | Volcado del contenido de memoria de un proceso abortado, guardado en un fichero para diagnóstico |
| **IPC** | Inter-Process Communication; mecanismos que permiten a los procesos comunicarse y sincronizarse |

---

## 🗂️ Clasificación de las llamadas al sistema

Las syscalls se agrupan en cuatro categorías:

```
Llamadas al sistema
├── 1. Control de procesos
│       exit, abort, createprocess, wait, getpid, ...
├── 2. Administración de ficheros y directorios
│       open, close, read, write, mkdir, setfilepointer, ...
├── 3. Comunicación entre procesos (IPC)
│       createmailbox, sendmessage, createsharedmemory, kill, ...
└── 4. Seguridad
        getusersecurity, setprocesssecurity, setfilesecurity, ...
```

---

## 🛠️ Llamadas al sistema para administración de procesos

### Terminación de procesos

Un proceso puede terminar de dos formas:

```
┌──────────────────────────────────────┐
│  Terminación normal (libera recursos)│  →  exit
│  Terminación abrupta (aborta ahora)  │  →  abort
└──────────────────────────────────────┘
```

Cuando se aborta un proceso, el SO puede volcar el contenido de la memoria a un fichero llamado **core** (históricamente), que puede analizarse con un depurador (debugger).

### Ciclo de vida y creación de procesos

- Un proceso puede **crear otros procesos** (`createprocess`) que ejecuten el mismo código u otro diferente.
- El proceso creador puede **controlar al hijo**: definir su prioridad, propietario, recursos máximos, tiempo de CPU, etc.
- Ambos procesos (padre e hijo) pueden ejecutarse **en paralelo** → escenario típico de multiprogramación.

### Sincronización entre procesos

- `wait` → un proceso espera a que otro termine (puede definirse un timeout).
- El kernel puede extender la espera a **sucesos** (events) asociados a objetos del sistema: ficheros, bloqueos, semáforos, secciones críticas, etc.

### Otras llamadas útiles de gestión de procesos

- Obtener/definir fecha y hora del sistema
- Conocer/modificar datos del sistema
- Cambiar atributos de procesos, ficheros y dispositivos
- Depuración: volcado de memoria, trazado de instrucciones, estadísticas de rendimiento

---

## 🛠️ Llamadas al sistema para administración de ficheros y directorios

### Operaciones básicas sobre ficheros

```
CICLO DE VIDA DE UN FICHERO
────────────────────────────────────────────────────────
createfile("ruta/fichero")         ← crear
    │
    ▼
open("ruta/fichero", modo)         ← abrir (establece estructuras en memoria)
    │
    ├──▶ read(fd, buffer, n)        ← leer n bytes
    ├──▶ write(fd, buffer, n)       ← escribir n bytes
    └──▶ setfilepointer(fd, pos)    ← mover puntero de posición
    │
    ▼
close(fd)                          ← cerrar (libera estructuras si nadie más lo usa)
    │
    ▼
deletefile("ruta/fichero")         ← eliminar
────────────────────────────────────────────────────────
```

> 📝 **Lectura:** `fd` (file descriptor) es el identificador numérico que el SO asigna al fichero al abrirlo. Todas las operaciones posteriores usan ese fd, no la ruta original.

### Lectura/escritura secuencial vs. aleatoria

Las operaciones de lectura y escritura son **secuenciales** por defecto (se leen/escriben segmentos contiguos). Para acceso aleatorio se utiliza un puntero de fichero que se actualiza con `setfilepointer`.

### Directorios

Se necesitan llamadas equivalentes para directorios: `mkdir`, `rmdir`, `opendir`, `readdir`, `closedir`, etc.

### Atributos de ficheros y directorios

Llamadas para consultar/modificar atributos: nombre, tipo, permisos (quién puede leer, escribir, ejecutar, eliminar).

### Acceso directo a dispositivos

Las aplicaciones también pueden acceder a dispositivos **directamente** (sin pasar por el sistema de ficheros):
1. Solicitar el uso del dispositivo
2. Posicionarse en un byte concreto
3. Leer o escribir
4. Liberar el dispositivo

Los dispositivos pueden ser **físicos** (un disco real) o **lógicos** (un disco en memoria RAM).

### Bloqueos de ficheros en entornos multiprogramación

En sistemas multi-proceso, varios procesos pueden intentar acceder al mismo fichero simultáneamente:

- **Solo lectura concurrente:** el SO lo permite sin mayor problema.
- **Escritura concurrente:** requiere mecanismos de bloqueo para garantizar la integridad de los datos.

Las syscalls de bloqueo permiten:
- Bloquear una porción de un fichero (o todo el fichero)
- Consultar si un fichero está bloqueado
- Esperar a que se libere el bloqueo, reintentar más tarde, o pedir notificación al kernel

---

## 🛠️ Llamadas al sistema de comunicación entre procesos (IPC)

Los SO aíslan los espacios de memoria de cada proceso por seguridad. Para que puedan cooperar, existen dos grandes esquemas de IPC:

### Esquema 1: Paso de mensajes

```
Proceso A                  Proceso B
    │                          │
    │   enviarmensaje(msg)     │
    │ ─────────────────────▶   │
    │                          │  leermensaje(buffer)
    │                          │
```

**Flujo de establecimiento de conexión:**

```
Cliente (inicia)                     Servidor (espera)
      │                                    │
      │                          esperarconexion()
      │                                    │ (dormido)
      │  abrirconexion(nombre/IP, puerto)  │
      │ ──────────────────────────────────▶│
      │                          aceptarconexion()
      │◀──────────────────────────────────│
      │  (canal establecido)              │
      │                                   │
      │  enviarmensaje(msg)               │
      │ ─────────────────────────────────▶│
      │                          leermensaje(buffer)
      │                                   │
      │  cerrarconexion()                 │
      │ ─────────────────────────────────▶│
```

> 📝 **Clientes:** procesos que activamente inician la comunicación.  
> 📝 **Servidores:** procesos receptores que esperan pasivamente una petición de conexión.

**Identificación de procesos remotos:** se usan direcciones IP (para protocolos de Internet) y puertos. Las llamadas `getpeername` y equivalentes permiten obtener estas identificaciones.

### Esquema 2: Memoria compartida

```
  Proceso A                Proceso B
  ┌────────────┐           ┌────────────┐
  │ espacio A  │           │ espacio B  │
  │            │           │            │
  │ ┌────────┐ │           │ ┌────────┐ │
  │ │ seg.   │ │           │ │ seg.   │ │
  │ │ comp.  │◀┼──────────┼▶│ comp.  │ │
  │ └────────┘ │           │ └────────┘ │
  └────────────┘           └────────────┘
         ▲                       ▲
         └──────────────────────┘
           Región reservada por el kernel
           (crearmemoriacompartida / conectarmemoriacompartida)
```

> 📝 **Lectura:** Ambos procesos ven la misma región de memoria como si fuera parte de su propio espacio de direcciones. El kernel NO controla el formato ni el orden de acceso → los procesos deben sincronizarse por su cuenta (semáforos, mutex, etc.).

### Comparación de esquemas IPC

| Característica | Paso de mensajes | Memoria compartida |
|----------------|------------------|--------------------|
| **Velocidad** | Más lenta (copia de datos) | Más rápida (acceso directo) |
| **Volumen de datos** | Adecuada para mensajes pequeños | Óptima para grandes cantidades |
| **Comunicación remota** | Sí (entre máquinas distintas) | No (solo procesos con el mismo kernel) |
| **Sincronización** | Implícita en el paso del mensaje | Debe implementarse explícitamente |
| **Complejidad** | Mayor (establecer canal, protocolos) | Menor en acceso, mayor en sincronización |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ La API **oculta detalles** del SO al programador, lo que puede llevar a suposiciones incorrectas sobre su comportamiento → conocer el funcionamiento interno evita **errores de diseño** difíciles de detectar.
- ❗ Una llamada al sistema puede retornar un **código de error** implícito en su valor de retorno → siempre hay que verificarlo.
- ✅ Cada llamada tiene un **código numérico único** en el kernel; el programador trabaja con nombres simbólicos gracias al compilador y la biblioteca del runtime.
- 🔁 Algunas llamadas al sistema **están obsoletas** porque nuevas versiones del SO ofrecen alternativas mejoradas (e.g., evolución de Win32 a Win64).
- ❗ La memoria compartida **no incluye sincronización automática** por parte del kernel → es responsabilidad de los procesos coordinar el acceso con semáforos, mutex u otros mecanismos.
- ✅ La memoria compartida no puede usarse fácilmente entre procesos en **máquinas diferentes** que no comparten kernel.

---

## 🔗 Conexiones con otros temas

- **Gestión de procesos (Tema 3):** Los conceptos de `init`, procesos padres/hijos y gestión de memoria se invocan directamente a través de las syscalls de control de procesos estudiadas aquí.
- **Sistemas de ficheros:** Las llamadas `open`, `read`, `write`, `close` son la base de cómo el SO abstrae el acceso a dispositivos de almacenamiento.
- **Sincronización y concurrencia:** Las llamadas IPC (semáforos, mutex, memoria compartida) son el fundamento de la programación concurrente y paralela.
- **Redes de computadoras:** El modelo cliente-servidor visto en IPC con paso de mensajes es la base de los sockets de red y protocolos como TCP/IP.
- **Estándar POSIX:** Define un conjunto portable de llamadas al sistema adoptado por Linux y macOS, permitiendo portabilidad de código entre sistemas.

---

## 📝 Preguntas de repaso

1. ¿Qué son las llamadas al sistema y por qué son necesarias en los sistemas operativos modernos?
2. ¿Cuál es la diferencia entre la API Win32 y las verdaderas llamadas al sistema del kernel de Windows?
3. ¿Qué papel juega el *Runtime Support System* en la ejecución de una llamada al sistema?
4. Explica los tres métodos para pasar parámetros en una llamada al sistema y las ventajas/limitaciones de cada uno.
5. ¿Cuál es la diferencia entre `exit` y `abort` al terminar un proceso? ¿Qué es un core dump?
6. ¿Para qué sirve la llamada `setfilepointer`? ¿En qué se diferencia la lectura secuencial de la aleatoria?
7. ¿Por qué los bloqueos de ficheros son necesarios en sistemas multiprogramación?
8. ¿Cuál es la diferencia entre un **cliente** y un **servidor** en el contexto del paso de mensajes IPC?
9. ¿Por qué la memoria compartida es más rápida que el paso de mensajes para grandes volúmenes de datos? ¿Qué desventaja tiene?
10. ¿Por qué la memoria compartida no es práctica para comunicar procesos en máquinas distintas?

---

## 📌 Resumen en una página

Las **llamadas al sistema** son el puente controlado entre el espacio de usuario y el kernel del SO. Cada llamada tiene un código numérico que el compilador traduce a través del *runtime support system*, el cual invoca la rutina correspondiente en el kernel. Las dos grandes APIs son **POSIX** (Linux/macOS) y **Win32/Win64** (Windows). Las syscalls se clasifican en cuatro grupos: **control de procesos** (crear, terminar, sincronizar procesos), **administración de ficheros y directorios** (crear, abrir, leer, escribir, bloquear ficheros), **comunicación entre procesos** (paso de mensajes y memoria compartida) y **seguridad**. En IPC, el **paso de mensajes** es portable (funciona en red) pero más lento; la **memoria compartida** es muy rápida para grandes datos pero requiere sincronización explícita y solo funciona entre procesos del mismo sistema.