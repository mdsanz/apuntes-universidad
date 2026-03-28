# Concepto de Sistema Operativo

> **Unidad:** Tema 1
> **Materia:** Administración de Sistemas Operativos — UNIR México
> **Palabras clave:** `kernel`, `proceso`, `memoria virtual`, `shell`, `llamadas al sistema`, `modo usuario`, `modo kernel`, `multiplexación`, `seguridad`, `sistema de ficheros`

---

## 🎯 Objetivo de la clase

Entender qué es un sistema operativo, cuáles son sus funciones principales (como máquina extendida y gestor de recursos), y cómo gestiona procesos, memoria, ficheros, seguridad e interactividad a través del shell. Comprender las decisiones de diseño que hacen posible todo esto.

---

## 🧠 Conceptos principales

### El Sistema Operativo como Software Fundamental

Sin software, un ordenador es solo hardware inerte. El SO es el programa más importante: coordina todo para que las aplicaciones de usuario puedan funcionar. Los programas que corren en segundo plano sin intervención del usuario se llaman **servicios** (Windows) o **demonios** (UNIX/Linux).

> 💡 **Intuición:** El SO es el gerente de un hotel: los huéspedes (aplicaciones) no hablan directamente con las tuberías o el generador eléctrico; el gerente se encarga de todo eso y les da una habitación limpia y funcional.
> 

---

### El Kernel

> El **kernel** (núcleo) es el programa central del SO. Su tarea fundamental es **gestionar todos los recursos del equipo, internos y externos**.
> 

El kernel trabaja en **modo kernel** o **modo supervisor** (máximo nivel de privilegio). Las aplicaciones de usuario lo hacen en **modo usuario** (privilegios limitados). Esta separación impide que una aplicación manipule directamente el hardware o corrompa el sistema.

```
┌──────────────────────────────────────────────┐
│           APLICACIONES DE USUARIO            │  ← Modo usuario
│  (navegador, editor, compilador, shell…)     │
├──────────────────────────────────────────────┤
│              LLAMADAS AL SISTEMA             │  ← Interfaz de comunicación
├──────────────────────────────────────────────┤
│                   KERNEL                    │  ← Modo kernel / supervisor
│  (gestión de procesos, memoria, E/S, etc.)  │
├──────────────────────────────────────────────┤
│                  HARDWARE                   │
│  (CPU, RAM, disco, red, teclado…)           │
└──────────────────────────────────────────────┘
```

> 📝 **Lectura del diagrama:** El kernel actúa como intermediario obligatorio entre las aplicaciones y el hardware. Ninguna app puede acceder al hardware directamente.
> 

⚠️ **Excepción:** Sistemas embebidos y de tiempo real pueden prescindir de esta separación de modos porque están diseñados para aplicaciones específicas, no para uso general. Las máquinas virtuales tampoco la necesitan porque el proceso de interpretación ya provee los mecanismos de separación.

---

### Dos visiones del Sistema Operativo

| Visión | Descripción | Nombre |
| --- | --- | --- |
| **De arriba a abajo** | El SO ofrece una interfaz amigable a usuarios y programadores: la **máquina extendida** o máquina virtual | Proveedor de abstracción |
| **De abajo a arriba** | El SO gestiona y controla todos los recursos del hardware de forma ordenada | Gestor de recursos |

Ambas visiones son válidas y complementarias.

---

### El SO como Máquina Extendida (Abstracción)

La arquitectura real del hardware (instrucciones máquina, registros, bloques de disco) es extremadamente compleja. El SO la oculta y ofrece abstracciones más simples:

- Un disco no se ve como bloques físicos → se ve como un árbol de **archivos con nombres**
- Los dispositivos de E/S no se programan con registros → se usan **llamadas al sistema** simples
- La memoria no se gestiona manualmente → el SO la administra de forma transparente

> 💡 **Intuición:** Es como el volante de un auto. El conductor no necesita saber cómo funciona la dirección hidráulica por dentro; solo gira el volante. El SO es ese volante entre tú y el hardware.
> 

Las abstracciones se exponen a través de **llamadas al sistema** (*system calls*): comandos especiales que los programas usan para solicitar servicios al SO.

---

### El SO como Gestor de Recursos (Multiplexación)

Cuando múltiples programas compiten por los mismos recursos (CPU, memoria, impresora), el SO arbitra el acceso. La gestión de recursos se hace de dos formas:

### Multiplexación en el tiempo

Los programas se turnan el uso del recurso. Ejemplo: la CPU se comparte entre varios procesos → **multiprogramación**.

### Multiplexación en el espacio

Cada programa obtiene una *parte* del recurso simultáneamente. Ejemplo: la RAM se divide entre todos los procesos activos.

```
Multiplexación en el TIEMPO (CPU):
  ──────────────────────────────────────▶ tiempo
  [ Proceso A ][ Proceso B ][ Proceso C ][ Proceso A ][ ... ]

  El SO decide cuánto tiempo recibe cada proceso (quantum).

Multiplexación en el ESPACIO (RAM):
  ┌───────────┬──────────┬────────────┬────────────┐
  │  Kernel   │Proceso A │ Proceso B  │  Proceso C │
  └───────────┴──────────┴────────────┴────────────┘

  Cada proceso tiene su propio segmento de memoria al mismo tiempo.
```

> 📝 **Lectura:** La CPU se turna (tiempo), la RAM se divide (espacio). El disco también se divide espacialmente: múltiples usuarios comparten bloques distintos.
> 

---

## 🔄 Gestión de Procesos

### ¿Qué es un proceso?

> Un **proceso** es básicamente un **programa en ejecución**.
> 

Cada proceso tiene asociado:

- **Espacio de direcciones:** rango de posiciones de memoria que puede usar (contiene el programa ejecutable, los datos y la pila)
- **Registros del proceso:** contador de instrucciones, puntero a pila, registros de hardware
- **Información de estado:** archivos abiertos, señales pendientes, etc.

### Tabla de Procesos

En muchos SO, toda la información de un proceso (excepto su espacio de direcciones) se almacena en la **tabla de procesos**: un array o lista enlazada con un registro por cada proceso activo.

Un **proceso suspendido** conserva:

- Su imagen en memoria (**imagen core**)
- Su entrada en la tabla de procesos (con el valor de todos sus registros)

### Creación y terminación de procesos

Las llamadas al sistema más importantes son las de **crear** y **terminar** procesos.

Ejemplo del Shell: cuando escribes un comando, el shell crea un proceso hijo para ejecutarlo. Cuando termina, el shell sigue esperando el siguiente comando.

Si un proceso crea procesos hijos, y estos crean más, surge una **estructura en árbol de procesos**.

```
        Shell (proceso padre)
           │
     ┌─────┴──────┐
  gcc (hijo)   ls (hijo)
     │
  linker (nieto)
```

> 📝 **Lectura:** El shell es el proceso raíz de lo que lanzas tú. Cada comando genera un proceso hijo que vive mientras se ejecuta.
> 

### Comunicación entre procesos (IPC)

Los procesos relacionados frecuentemente necesitan comunicarse y sincronizarse. Esta comunicación se llama **IPC** (*Inter-Process Communication*). Mecanismos comunes: tuberías (pipes), mensajes, señales.

### Señales

Las **señales** son el análogo software a las interrupciones de hardware. Cuando el SO envía una señal a un proceso:

1. El proceso suspende lo que hace
2. Guarda el estado de sus registros en la pila
3. Ejecuta un procedimiento especial (manejador de señal)
4. Cuando termina el manejador, el proceso se reanuda desde donde estaba

Causas de señales: expiración de temporizadores, errores de hardware (instrucción ilegal, dirección inválida), comunicación entre procesos.

---

## 💾 Gestión de Memoria

### El problema

Los programadores siempre querrían memoria infinita, rapidísima y no volátil. Eso no existe, así que el SO usa una **jerarquía de memoria**:

```
  ┌────────────────────────────────────┐
  │  Registros / Caché                 │  ← Muy rápida, muy cara, volátil, pequeña
  ├────────────────────────────────────┤
  │  RAM (memoria principal)           │  ← Rápida, precio medio, volátil, cientos de MB/GB
  ├────────────────────────────────────┤
  │  Almacenamiento (SSD/HDD)          │  ← Lenta, barata, no volátil, terabytes
  └────────────────────────────────────┘
         ▲ velocidad                  ▲ capacidad
```

> 💡 **Intuición:** Es como tu escritorio (RAM = mesa de trabajo) vs. tus cajones (disco). Lo que usas ahora está en la mesa; lo demás, en los cajones. El SO mueve cosas entre ellos según necesites.
> 

### Funciones del gestor de memoria

El SO (generalmente el kernel) se encarga de:

1. **Contabilizar** qué partes de la memoria están libres u ocupadas
2. **Asignar** memoria a los procesos cuando la necesitan
3. **Liberar** memoria cuando un proceso termina
4. **Intercambiar** (*swap*) partes de memoria al disco cuando escasea el espacio

> ⚠️ **Ley de Parkinson aplicada a la memoria:** "Los programas y sus datos crecen hasta ocupar toda la memoria disponible." Siempre habrá presión sobre la memoria, sin importar cuánta tengas.
> 

---

## 📁 Gestión de Directorios y Archivos

### Archivos

La abstracción principal para almacenar datos son los **ficheros**: colecciones de bytes con nombre, independientes del dispositivo físico donde se guardan.

El SO ofrece llamadas al sistema para: crear, eliminar, abrir, cerrar, leer y escribir ficheros.

> 💡 **Intuición:** El SO es como un bibliotecario: tú pides el libro por nombre ("informe.pdf"), él sabe en qué estante físico (bloque del disco) está y te lo trae. Tú no necesitas saber la ubicación física.
> 

### Directorios

Para organizar la gran cantidad de ficheros, el SO provee **directorios** (carpetas). Un directorio puede contener ficheros y otros directorios → estructura de **árbol**.

```
  / (raíz)
  ├── home/
  │   └── marcos/
  │       ├── documentos/
  │       │   └── tesis.pdf
  │       └── proyectos/
  │           └── portfolio/
  ├── etc/
  └── usr/
      └── bin/
          └── bash
```

> 📝 **Lectura:** Cada fichero se identifica por su **ruta** (*path*) desde la raíz: `/home/marcos/documentos/tesis.pdf`
> 

### Rutas absolutas

> **Ruta absoluta:** lista de directorios que hay que recorrer desde el directorio raíz para llegar al fichero. Cada elemento separado por `/` (UNIX) o `\` (Windows).
> 

### Comparación: procesos vs. ficheros

| Característica | Jerarquía de procesos | Árbol de directorios |
| --- | --- | --- |
| Profundidad típica | < 4 niveles | Muchos niveles |
| Duración | Minutos | Años |
| Control de acceso | Solo el proceso padre | Puede compartirse ampliamente |

### Tipos de ficheros especiales

Un **fichero especial** no contiene datos: es una abstracción para tratar dispositivos de E/S como si fueran archivos normales.

| Tipo | Uso típico |
| --- | --- |
| **Tipo bloque** | Discos: dispositivos de bloques direccionables individualmente |
| **Tipo carácter** | Impresoras, conexiones de red, dispositivos que producen/consumen flujos de caracteres |

### Tuberías (Pipes)

> Una **tubería** es un pseudoarchivo que conecta dos procesos: uno escribe en un extremo, el otro lee del otro. La comunicación es unidireccional y FIFO (sin releer lo ya leído).
> 

```
  ┌──────────┐   PIPE   ┌──────────┐
  │ Proceso A│ ────────▶│ Proceso B│
  │  (escribe│          │  (lee)   │
  └──────────┘          └──────────┘

  Ejemplo en shell: ls -la | grep ".md"
  ls escribe → pipe → grep lee
```

### Sistemas de ficheros montados (mount)

En UNIX, un dispositivo externo (USB, segundo disco) se **monta** en un directorio del árbol principal. Así, su contenido se accede como parte del árbol normal:

```
  mount /dev/sdb1 /mnt/usb

  Antes del mount:  /mnt/usb/  (directorio vacío)
  Después:          /mnt/usb/  (contiene el sistema de ficheros del USB)
```

---

## 💻 El Shell: Interactividad

> El **Shell** es la **interfaz primaria para los usuarios** de los sistemas operativos. No es parte del kernel, pero hace un uso tan extensivo del SO que es esencial entenderlo.
> 

Los editores, compiladores y enlazadores tampoco son parte del SO, aunque sean herramientas fundamentales.

### Funcionamiento básico del Shell

1. El usuario inicia sesión → el shell arranca
2. Muestra un **prompt** (`$`, `>`, `#`...) indicando que espera un comando
3. El usuario teclea un comando → el shell crea un **proceso hijo** para ejecutarlo
4. El shell espera a que termine el proceso hijo
5. Vuelve a mostrar el prompt y repite

### Redirección y tuberías en el Shell

```bash
# Redirigir salida a fichero
ls -la > listado.txt

# Leer entrada desde fichero
sort < datos.txt

# Tubería: salida de uno → entrada de otro
ps aux | grep java | wc -l

# Ejecutar en segundo plano (sin esperar)
compilar-proyecto &
```

### GUI vs Shell

Las interfaces gráficas (GUI) son más amigables, pero **no pueden ofrecer toda la versatilidad del shell**. Los usuarios avanzados siempre vuelven al terminal porque:

- La automatización (scripts) es prácticamente imposible con GUI
- El shell combina comandos de formas ilimitadas
- Acceso a toda la funcionalidad del SO sin restricciones de interfaz

---

## 🔐 Seguridad

### Importancia

Con computadores almacenando desde información gubernamental secreta hasta datos bancarios personales, la seguridad del SO es crítica.

### Terminología de seguridad

| Término | Definición |
| --- | --- |
| **Actor** | Entidad que puede realizar una acción (usuario, proceso, hilo) |
| **Objeto** | Entidad que puede ser manipulada (fichero, clave de registro, campo de BD, dispositivo) |
| **Acción / Derecho** | Lo que un actor puede hacer sobre un objeto (leer, escribir, ejecutar, eliminar) |

> 💡 **Intuición:** Piénsalo como una matrix 3D: `[actor][objeto][acción]`. ¿Puede el usuario "marcos" leer el fichero "nóminas.csv"? El SO consulta esta matriz para decidir.
> 

### Mecanismos de control de acceso

**Listas de control de acceso (ACL):** por cada objeto, una lista de actores que pueden manipularlo.

**Sistemas basados en capacidades:** por cada actor, una lista de objetos sobre los que tiene permiso.

```
┌─────────────────────────────────────────────┐
│           LA SEGURIDAD TIENE DOS PARTES     │
├──────────────────────────┬──────────────────┤
│ Evitar que usuarios      │ Permitir acceso  │
│ accedan sin permisos     │ a quienes sí     │
│ (denegación correcta)    │ tienen derechos  │
└──────────────────────────┴──────────────────┘
```

### Tipos de control

- **Discrecional:** el propietario del objeto decide quién más accede (modelo más común en UNIX/Windows)
- **Mandatorio / No discrecional:** reglas fijas de seguridad que los usuarios no pueden saltarse (usado en entornos gubernamentales y militares)

### Modo kernel y protección de memoria

Los primeros mecanismos de seguridad fueron estrategias de protección de memoria: impedir que procesos de usuario sobrescribieran zonas del kernel o rutinas de interrupciones.

Desde el procesador Z80, las CPUs fueron diseñadas para impedir que procesos de usuario sobrescribieran áreas del sistema. Mecanismo: **espacios de direcciones separados** para sistema y usuario, con interrupciones software para cambiar entre modos.

---

## 🏗️ Objetivos de Diseño de un Sistema Operativo

Los SO exitosos han aplicado decisiones de diseño sólidas, pero siguen evolucionando. El SO ideal buscaría estos objetivos (aunque no todos son compatibles entre sí):

| Objetivo | Descripción |
| --- | --- |
| **Seguridad ortogonal** | La seguridad no es un añadido, está integrada en cada aspecto del diseño |
| **Elegancia** | Soluciones simples y limpias a problemas complejos |
| **Simplicidad** | Fácil de entender, usar y mantener |
| **Integridad** | Los datos y el sistema siempre están en un estado consistente |
| **Resiliencia** | Capacidad de recuperarse de fallos |
| **Robustez** | Predictibilidad ante errores, incluyendo fallos de hardware |
| **Modularidad** | Componentes independientes y reemplazables |
| **Uniformidad** | Consistencia en interfaces y comportamientos |
| **Extensibilidad** | Facilidad para añadir soporte a nuevo hardware o tecnologías |
| **Distribución** | Soporte nativo para sistemas distribuidos |
| **Reflexión** | Capacidad de monitorearse y adaptarse a sí mismo |
| **Orientación a objetos** | Modelo de objetos para recursos y entidades del sistema |
| **Fiabilidad / Tolerancia a fallos** | Resistencia y recuperación ante errores |
| **Auto-documentación** | El sistema explica su propio estado y comportamiento |
| **Predictibilidad** | Comportamiento determinista y reproducible |
| **Reversibilidad** | Posibilidad de deshacer operaciones |
| **Corrección probada** | Verificación formal del código (muy costosa, pero ideal) |

> ⚠️ **No todos los objetivos son compatibles entre sí.** Por ejemplo, maximizar la seguridad puede reducir la simplicidad. La pregunta clave siempre es: *¿cuáles objetivos priorizar según el contexto?*
> 

> 💡 **Compatibilidad:** Capacidad de un SO para ejecutar programas escritos para otros SO o versiones anteriores. Es otro objetivo deseable pero que puede entrar en conflicto con la elegancia o la seguridad.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Kernel** | Programa central del SO; gestiona todos los recursos del equipo, internos y externos |
| **Proceso** | Programa en ejecución; tiene espacio de direcciones, registros y estado |
| **Tabla de procesos** | Estructura del SO (array o lista enlazada) que almacena información de cada proceso activo |
| **Imagen core** | Espacio de direcciones de un proceso suspendido |
| **Llamada al sistema** | Instrucción extendida que los programas usan para solicitar servicios al SO |
| **Modo kernel** | Nivel de ejecución con máximos privilegios (solo el SO) |
| **Modo usuario** | Nivel de ejecución con privilegios limitados (aplicaciones) |
| **Shell** | Intérprete de comandos; interfaz primaria del usuario con el SO |
| **Multiplexación en el tiempo** | Varios programas se turnan el uso de un recurso |
| **Multiplexación en el espacio** | El recurso se divide entre varios programas simultáneamente |
| **Fichero especial** | Abstracción que permite tratar dispositivos E/S como archivos |
| **Tubería (pipe)** | Pseudoarchivo FIFO que conecta la salida de un proceso con la entrada de otro |
| **Señal** | Análogo software a una interrupción hardware; notifica eventos a un proceso |
| **ACL** | Lista de Control de Acceso; define qué actores pueden operar sobre cada objeto |
| **ISA** | *Instruction Set Architecture*; conjunto de instrucciones disponibles en una CPU |
| **Ruta absoluta** | Camino completo desde el directorio raíz hasta un fichero |
| **Jerarquía de memoria** | Organización escalonada: caché → RAM → disco, con trade-off velocidad/capacidad/costo |

---

## 📊 Diagrama: Arquitectura general del SO

```
  ┌──────────────────────────────────────────────────┐
  │                 APLICACIONES                     │
  │  (navegador, IDE, reproductor, shell, etc.)      │
  └────────────────────┬─────────────────────────────┘
                       │ llamadas al sistema
  ┌────────────────────▼─────────────────────────────┐
  │                    KERNEL                        │
  │  ┌──────────┐ ┌──────────┐ ┌──────────────────┐  │
  │  │ Gestión  │ │ Gestión  │ │  Sistema de      │  │
  │  │ procesos │ │ memoria  │  │  ficheros        │  │
  │  └──────────┘ └──────────┘ └──────────────────┘  │
  │  ┌──────────────────────────────────────────────┐ │
  │  │     Drivers / Controladores de dispositivos  │ │
  │  └──────────────────────────────────────────────┘ │
  └────────────────────┬─────────────────────────────┘
                       │
  ┌────────────────────▼─────────────────────────────┐
  │                  HARDWARE                        │
  │   CPU    │   RAM    │   Disco   │   Red   │ E/S  │
  └──────────────────────────────────────────────────┘
```

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ El **Shell NO es parte del kernel** ni del SO; es una aplicación de usuario que lo usa intensamente.
- ❗ Los **compiladores, editores y enlazadores tampoco son parte del SO**, aunque sean herramientas de sistema.
- ✅ La separación **modo kernel / modo usuario** es fundamental para la seguridad y estabilidad del sistema.
- 🔁 Los **sistemas embebidos** pueden no tener esta separación de modos por diseño intencional.
- ❗ La **corrección formal** de un kernel sería ideal pero su costo es prohibitivo en la práctica.
- ✅ **Tuberías y señales** son mecanismos del SO (no solo del shell): los programas también las usan directamente.
- 🔁 La **tabla de procesos** es la estructura central que permite la multiprogramación.

---

## 🔗 Conexiones con otros temas

- **Multiprogramación y scheduling:** el gestor de procesos decide qué proceso ejecuta la CPU y por cuánto tiempo (se estudiará en profundidad en temas siguientes).
- **Memoria virtual:** extensión de la gestión de memoria; permite a un proceso tener más memoria de la que físicamente existe (RAM → disco).
- **Llamadas al sistema:** son la interfaz real entre aplicaciones y SO; se verán en detalle durante toda la asignatura.
- **Shell scripting:** el shell no solo ejecuta comandos; tiene lenguaje de scripting completo (variables, bucles, condicionales) que se estudia más adelante.
- **Sistemas de ficheros en profundidad:** organización interna de inodos, bloques, permisos, journaling.

---

## 📝 Preguntas de repaso

1. ¿Qué es el kernel y cuál es su función principal? ¿Por qué se ejecuta en modo privilegiado?
2. ¿Cuál es la diferencia entre "modo kernel" y "modo usuario"? ¿Por qué existe esta separación?
3. ¿Qué significa que el SO es una "máquina extendida"? Da un ejemplo concreto con el sistema de ficheros.
4. ¿Qué es la multiplexación en el tiempo? ¿Y en el espacio? ¿Qué recursos se multiplexa de cada forma?
5. ¿Qué información contiene un proceso? ¿Qué ocurre cuando un proceso se suspende?
6. ¿Cuál es la diferencia entre una señal y una interrupción de hardware?
7. ¿Por qué el Shell no es parte del SO pero se estudia junto a él?
8. ¿Qué son las ACL y para qué sirven en la gestión de seguridad?
9. ¿Qué es una tubería (pipe) y cómo se usa en el Shell? ¿Qué tipo de comunicación permite?
10. Menciona 5 objetivos de diseño de un SO y explica por qué pueden entrar en conflicto entre sí.

---

## 📌 Resumen en una página

Un **sistema operativo** es el software que actúa como intermediario entre el hardware y las aplicaciones. Su núcleo, el **kernel**, opera con máximos privilegios (modo kernel) y expone servicios a las apps a través de **llamadas al sistema**, mientras que las aplicaciones corren en modo usuario con acceso restringido al hardware. El SO cumple dos roles complementarios: como **máquina extendida**, oculta la complejidad del hardware y ofrece abstracciones simples (archivos, procesos, memoria); como **gestor de recursos**, multiplexa la CPU en el tiempo y la RAM en el espacio para que múltiples procesos coexistan. Un **proceso** es un programa en ejecución con su propio espacio de direcciones y estado, administrado a través de la **tabla de procesos**. La **gestión de memoria** usa jerarquía (caché → RAM → disco) y el kernel asigna, libera e intercambia memoria según se necesite. El **sistema de ficheros** organiza los datos en archivos y directorios con estructura de árbol, accesibles por ruta absoluta. El **shell** es la interfaz principal de usuario al SO y permite lanzar procesos, redirigir E/S y encadenar comandos con tuberías. La **seguridad** se basa en la separación de modos y en mecanismos como ACL para controlar qué actores pueden realizar qué acciones sobre qué objetos.