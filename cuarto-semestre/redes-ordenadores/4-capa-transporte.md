# La Capa de Transporte

> **Unidad:** Tema 4
> 
> 
> **Palabras clave:** `TCP`, `UDP`, `SCTP`, `puertos`, `3-way handshake`, `control de flujo`, `control de congestión`, `datagrama`, `segmento`, `capa de transporte`
> 

---

## 🎯 Objetivo de la clase

Entender cómo la capa de transporte permite la **comunicación entre procesos** en distintas máquinas. Estudiar los dos grandes protocolos: **UDP** (rápido, no fiable, sin conexión) y **TCP** (fiable, orientado a conexión), más el protocolo moderno **SCTP**. La clave es saber cuándo usar cada uno y por qué.

---

## 🧠 Conceptos principales

### El Puerto: Identificador de Proceso

La dirección IP identifica una máquina, pero en esa máquina pueden correr cientos de procesos simultáneamente. El **puerto** es el número que identifica exactamente a qué proceso va dirigido cada paquete.

La IANA gestiona los **65 536 puertos** (2¹⁶) distribuidos en tres rangos:

| Rango | Nombre | Quién lo gestiona |
| --- | --- | --- |
| 0 – 1023 | **Bien conocidos** | IANA (asignación oficial) |
| 1024 – 49151 | **Registrados** | No controlados, pero registrables para evitar duplicados |
| 49152 – 65535 | **Dinámicos** | Cualquier proceso los puede usar libremente |

> 💡 **Intuición:** El puerto es como el número de apartamento en un edificio. La IP es la dirección del edificio; el puerto es la puerta exacta a la que llaman. Sin puerto, el cartero (sistema operativo) no sabe a qué proceso entregar el mensaje.
> 

---

### Dos Dimensiones del Servicio de Transporte

La capa de transporte ofrece dos elecciones independientes:

**1. Tipo de conexión:**

- **Orientado a conexión:** los dos extremos negocian antes de comunicarse (establecimiento → datos → liberación). Ejemplo: TCP.
- **No orientado a conexión:** los paquetes se envían directamente sin negociación previa ni liberación. Ejemplo: UDP.

**2. Fiabilidad:**

- **Fiable:** garantiza entrega, orden y ausencia de errores. Mayor overhead.
- **No fiable:** no garantiza nada. Más rápido y ligero.

> 💡 **Intuición:** Orientado a conexión es como llamar por teléfono: primero marcas, esperas que contesten, hablas, y al final cuelgas. No orientado a conexión es como enviar un mensaje en una botella al mar: lo lanzas y ya, sin saber si llegará.
> 

---

### UDP — User Datagram Protocol

Protocolo **no orientado a conexión** y **no fiable**. Su gran ventaja es la simplicidad y el mínimo overhead.

**Características clave:**

- Los paquetes se llaman **datagramas de usuario**.
- Cabecera de **tamaño fijo: 8 bytes** (puerto origen, puerto destino, longitud, checksum).
- Los datagramas son **independientes entre sí** y **no están numerados**.
- No hay establecimiento ni liberación de conexión.
- Ideal cuando la velocidad importa más que la fiabilidad: streaming de voz/vídeo, DNS, juegos online.

---

### TCP — Transmission Control Protocol

Protocolo **orientado a conexión** y **fiable**. Es uno de los pilares de Internet (TCP/IP).

**Características clave:**

- Los paquetes se llaman **segmentos**, y van **numerados** (flujo de bytes).
- Comunicación **bidireccional (full-duplex)**.
- Tres mecanismos de fiabilidad:
    - **Control de flujo:** evita que un emisor rápido sature a un receptor lento.
    - **Control de errores:** detecta segmentos corruptos o perdidos y los retransmite.
    - **Control de congestión:** ajusta el volumen enviado según el estado de la red, no solo del receptor.

**Tres fases del ciclo de vida de una conexión TCP:**

1. **Establecimiento:** negociación de 3 vías (*3-way handshaking*).
2. **Transferencia de datos:** bidireccional con acuses de recibo (ACK).
3. **Liberación:** negociación de 3 o 4 vías, normalmente iniciada por el cliente.

---

### SCTP — Stream Control Transmission Protocol

Protocolo más reciente que combina características de TCP y UDP:

| Característica | TCP | UDP | SCTP |
| --- | --- | --- | --- |
| Orientado a conexión | ✅ | ❌ | ✅ |
| Fiable | ✅ | ❌ | ✅ |
| Basado en flujo de bytes | ✅ | ❌ | ❌ |
| Basado en mensajes | ❌ | ✅ | ✅ |
| Control de congestión | ✅ | ❌ | ✅ |
| Multistream | ❌ | ❌ | ✅ |

La característica más destacada de SCTP es la **asociación**: permite establecer **múltiples flujos de comunicación** simultáneos entre los mismos dos procesos (*multistream service*).

> 💡 **Intuición:** SCTP es como TCP pero con "carreteras múltiples" entre los mismos dos puntos. Si un flujo se bloquea, los otros siguen funcionando sin interrumpir toda la conexión.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Puerto** | Identificador numérico (0–65535) que designa un proceso específico en una máquina |
| **IANA** | Internet Assigned Numbers Authority — organismo que gestiona la asignación de puertos bien conocidos |
| **Datagrama de usuario** | Paquete UDP. Cabecera fija de 8 bytes, independiente, no numerado |
| **Segmento TCP** | Paquete TCP. Numerado, parte de un flujo de bytes ordenado |
| **3-way handshake** | Mecanismo de establecimiento de conexión TCP en tres pasos: SYN → SYN+ACK → ACK |
| **ACK** | Acknowledgement — acuse de recibo usado por TCP para confirmar la recepción de segmentos |
| **Control de flujo** | Mecanismo TCP para que el emisor no sature al receptor |
| **Control de congestión** | Mecanismo TCP para adaptar la velocidad de envío al estado de la red |
| **Asociación SCTP** | Establecimiento de múltiples flujos de comunicación simultáneos entre dos procesos |
| **Multiplexación** | Transmisión de paquetes de distintos procesos en una única conexión de transporte |

---

## 📊 Diagramas

### Clasificación de Puertos (IANA)

```
  ┌─────────────────────────────────────────────────────────────┐
  │                    65 536 PUERTOS (2¹⁶)                     │
  ├──────────────────┬──────────────────────┬───────────────────┤
  │   0 — 1023       │   1024 — 49151       │  49152 — 65535    │
  │                  │                      │                   │
  │  BIEN CONOCIDOS  │    REGISTRADOS       │   DINÁMICOS       │
  │                  │                      │                   │
  │  Asignados por   │  No controlados por  │  Uso libre por    │
  │  la IANA         │  IANA, pero          │  cualquier        │
  │                  │  registrables        │  proceso          │
  │  FTP:    21      │                      │                   │
  │  SSH:    22      │  Ejemplos de uso:    │  Asignados        │
  │  SMTP:   25      │  aplicaciones de     │  dinámicamente    │
  │  DNS:    53      │  terceros            │  por el SO        │
  │  HTTP:   80      │                      │                   │
  │  HTTPS: 443      │                      │                   │
  └──────────────────┴──────────────────────┴───────────────────┘
```

> 📝 **Lectura:** Los puertos bien conocidos son los "reservados" para servicios estándar de Internet. Cualquier proceso cliente usa un puerto dinámico como origen, y se comunica con el puerto bien conocido del servicio destino (ej: cliente → puerto aleatorio 52341 → servidor HTTP puerto 80).
> 

---

### Puertos TCP vs UDP para Servicios Comunes

```
  Puerto │ Protocolo │ Servicio
  ───────┼───────────┼──────────────────────────────
    21   │   TCP     │ FTP (transferencia de archivos)
    22   │   TCP     │ SSH (conexión remota segura)
    23   │   TCP     │ Telnet
    25   │   TCP     │ SMTP (envío de correo)
    53   │ TCP + UDP │ DNS (resolución de nombres)
    69   │   UDP     │ TFTP (FTP trivial, sin conexión)
    80   │   TCP     │ HTTP (web)
   161   │   UDP     │ SNMP (gestión de red)
   443   │   TCP     │ HTTPS (web segura)

  TCP: servicios que necesitan fiabilidad y orden
  UDP: servicios que priorizan velocidad sobre fiabilidad
```

> 📝 **Lectura:** La elección de TCP o UDP para cada servicio refleja su necesidad de fiabilidad. DNS usa ambos: UDP para consultas rápidas (99% de los casos), TCP para transferencias de zona completas.
> 

---

### Estructura del Datagrama UDP

```
  ┌──────────────────────────────────────────────┐
  │                DATAGRAMA UDP                 │
  │           Cabecera fija: 8 bytes             │
  ├─────────────────┬────────────────────────────┤
  │  Puerto origen  │     Puerto destino          │
  │    (16 bits)    │       (16 bits)             │
  ├─────────────────┼────────────────────────────┤
  │    Longitud     │    Checksum (suma control)  │
  │    (16 bits)    │       (16 bits)             │
  ├─────────────────┴────────────────────────────┤
  │                                              │
  │          DATOS (longitud variable)           │
  │                                              │
  └──────────────────────────────────────────────┘

  Sin numeración │ Sin establecimiento de conexión
  Sin ACK        │ Cada datagrama es completamente independiente
```

> 📝 **Lectura:** La cabecera UDP es minimalista a propósito. Solo lo esencial para enrutar el paquete al proceso correcto. Esta simplicidad es lo que hace a UDP tan rápido: sin negociaciones, sin control de orden, sin retransmisiones.
> 

---

### El 3-Way Handshake TCP

```
      CLIENTE                              SERVIDOR
         │                                    │
         │──── 1. SYN ───────────────────────▶│  "Quiero conectarme,
         │     (Synchronize)                   │   mi número de secuencia es X"
         │                                    │
         │◀─── 2. SYN + ACK ─────────────────│  "Aceptado. Mi número es Y,
         │     (Synchronize + Acknowledge)    │   confirmo tu X"
         │                                    │
         │──── 3. ACK ───────────────────────▶│  "Confirmo tu Y.
         │     (Acknowledge)                  │   Conexión establecida."
         │                                    │
         │◀═══════ TRANSFERENCIA DE DATOS ════│  Flujo bidireccional de
         │══════════════════════════════════▶ │  segmentos numerados + ACKs
         │                                    │
         │──── FIN ──────────────────────────▶│  "Quiero cerrar la conexión"
         │◀─── FIN + ACK ─────────────────────│  Liberación en 3 o 4 pasos
         │                                    │
```

> 📝 **Lectura:** El 3-way handshake sincroniza los números de secuencia de ambos extremos antes de enviar datos. Esto es lo que permite a TCP garantizar el orden de entrega y la detección de segmentos perdidos. Sin este paso, no hay forma de saber qué se recibió y qué no.
> 

---

### UDP vs TCP vs SCTP — Comparativa Completa

```
  ┌────────────────────────┬──────────┬──────────┬──────────┐
  │ Característica         │   UDP    │   TCP    │   SCTP   │
  ├────────────────────────┼──────────┼──────────┼──────────┤
  │ Orientado a conexión   │    ❌    │    ✅    │    ✅    │
  │ Fiable                 │    ❌    │    ✅    │    ✅    │
  │ Flujo de bytes         │    ❌    │    ✅    │    ❌    │
  │ Basado en mensajes     │    ✅    │    ❌    │    ✅    │
  │ Control de flujo       │    ❌    │    ✅    │    ✅    │
  │ Control de errores     │    ❌    │    ✅    │    ✅    │
  │ Control de congestión  │    ❌    │    ✅    │    ✅    │
  │ Multistream            │    ❌    │    ❌    │    ✅    │
  │ Overhead de cabecera   │  8 bytes │ 20 bytes │ 12 bytes │
  │ Velocidad relativa     │  ⚡⚡⚡  │   ⚡⚡   │  ⚡⚡⚡  │
  ├────────────────────────┼──────────┼──────────┼──────────┤
  │ Nombre del paquete     │Datagrama │ Segmento │  Chunk   │
  └────────────────────────┴──────────┴──────────┴──────────┘

  Casos de uso típicos:
  UDP  → DNS, streaming vídeo/audio, juegos online, VoIP
  TCP  → HTTP/HTTPS, FTP, SSH, email
  SCTP → Telefonía IP (SS7), señalización en redes móviles
```

> 📝 **Lectura:** La elección del protocolo es un trade-off entre fiabilidad y velocidad. UDP es ideal cuando perder un paquete es tolerable (un frame de vídeo perdido es menos grave que retransmitirlo y causar retraso). TCP es imprescindible cuando cada byte debe llegar en orden (descargar un archivo, cargar una web).
> 

---

### Ciclo Completo de una Comunicación TCP

```
  APLICACIÓN CLIENTE        CAPA TRANSPORTE       APLICACIÓN SERVIDOR
  ──────────────────        ───────────────       ───────────────────

  1. Solicita conexión  ──▶ [3-way handshake] ──▶ Acepta conexión
                                   ↕
  2. Envía datos        ──▶ [Segmentación     ] ──▶ Recibe datos
                            [Numeración       ]
                            [Control de flujo ]
                            [Control errores  ]
                            [ACKs             ]
                                   ↕
  3. Cierra conexión    ──▶ [FIN / FIN+ACK    ] ──▶ Cierra conexión
                            [4-way handshake  ]
```

> 📝 **Lectura:** Una conexión TCP siempre tiene estas tres fases. La fase de transferencia es la más larga y compleja: el emisor adapta continuamente su velocidad de envío al receptor (control de flujo) y al estado de la red (control de congestión).
> 

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **Puertos ≠ IP:** La IP identifica la máquina; el puerto identifica el proceso. Ambos juntos forman un **socket** (IP:puerto).
- ❗ **UDP no es "malo":** UDP no tiene errores de diseño. Simplemente está hecho para casos donde la velocidad y la baja latencia son más importantes que la fiabilidad. VoIP con TCP sería inutilizable por los retardos de retransmisión.
- ✅ **Fiabilidad sin TCP es posible:** Una aplicación puede usar UDP e implementar sus propios mecanismos de control de errores a nivel de aplicación. No es obligatorio depender de TCP para tener fiabilidad.
- ✅ **SCTP no reemplaza a TCP ni UDP:** Es un protocolo adicional para casos específicos donde se necesitan múltiples flujos simultáneos entre los mismos extremos.
- ❗ **SYN Flood (ataque DoS):** El 3-way handshake de TCP es vulnerable a ataques de inundación SYN: el atacante envía miles de SYN con IP de origen falsa, el servidor reserva recursos para cada conexión incompleta y se queda sin memoria. Esto demuestra el coste del overhead de TCP.
- 🔁 **Conexión con la capa de red:** La capa de transporte (TCP/UDP) opera sobre la capa de red (IP). IP se encarga del enrutamiento máquina a máquina; TCP/UDP se encargan de proceso a proceso dentro de esas máquinas.

---

## 🔗 Conexiones con otros temas

- **Tema 3 — Capa de Red (IP):** IP entrega paquetes de máquina a máquina. La capa de transporte toma el relevo y los entrega de proceso a proceso usando puertos. Son complementarias.
- **Tema 5 — Capa de Aplicación:** HTTP, FTP, DNS, SMTP son protocolos de aplicación que usan TCP o UDP como transporte. La elección del protocolo de transporte la hace el diseñador del protocolo de aplicación.
- **Seguridad (TLS/SSL):** TLS opera sobre TCP, añadiendo cifrado y autenticación. HTTPS = HTTP sobre TLS sobre TCP. La fiabilidad de TCP es un requisito para que TLS funcione correctamente.

---

## 📝 Preguntas de repaso

1. ¿Qué es un puerto y para qué sirve en la capa de transporte?
2. ¿Cuáles son los tres rangos de puertos definidos por la IANA y qué caracteriza a cada uno?
3. ¿Cuál es la diferencia entre un servicio orientado a conexión y uno no orientado a conexión?
4. ¿Por qué UDP puede ser preferible a TCP en aplicaciones de streaming de vídeo?
5. ¿Qué tamaño tiene la cabecera UDP y qué campos contiene?
6. Explica el proceso de establecimiento de conexión TCP (3-way handshake) paso a paso.
7. ¿Cuál es la diferencia entre control de flujo y control de congestión en TCP?
8. ¿En qué se parece SCTP a TCP y en qué se parece a UDP? ¿Qué lo hace único?
9. ¿Qué son los ACKs y en qué protocolo se usan?
10. Si una aplicación usa UDP pero necesita fiabilidad, ¿qué opciones tiene?

---

## 📌 Resumen en una página

> La **capa de transporte** permite la comunicación entre **procesos** (no solo entre máquinas) usando **puertos** como identificadores. La IANA divide los 65 536 puertos en bien conocidos (0–1023), registrados (1024–49151) y dinámicos (49152–65535). Los dos protocolos principales son **UDP** y **TCP**. **UDP** (*User Datagram Protocol*) es no orientado a conexión y no fiable: sus paquetes, llamados *datagramas*, tienen una cabecera mínima de 8 bytes y se envían de forma totalmente independiente. Ideal para aplicaciones que priorizan velocidad (vídeo, DNS, VoIP). **TCP** (*Transmission Control Protocol*) es orientado a conexión y fiable: trabaja con *segmentos* numerados, establece la conexión con un *3-way handshake* (SYN → SYN+ACK → ACK), y ofrece control de flujo, control de errores y control de congestión. Ideal para HTTP, FTP, SSH y cualquier aplicación donde cada byte deba llegar en orden. **SCTP** combina la fiabilidad de TCP con el modelo de mensajes de UDP, y añade el *multistream service*: múltiples flujos de comunicación simultáneos entre los mismos dos procesos.
>