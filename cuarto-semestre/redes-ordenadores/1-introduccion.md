# Redes de Computadores e Internet

> **Unidad:** Tema 1
> 
> 
> **Palabras clave:** `redes`, `Internet`, `protocolo`, `OSI`, `TCP/IP`, `topología`, `LAN`, `WAN`, `simplex`, `duplex`, `IEEE`
> 

---

## 🎯 Objetivo de la clase

Introducir los conceptos fundamentales de las redes de computadores: componentes de un sistema de comunicación, tipos de flujo de datos, estructuras físicas y topologías de red, categorías de redes según su extensión, y arquitectura de protocolos (modelo OSI y TCP/IP).

---

## 🧠 Conceptos principales

### 1.2 Redes e Internet

---

### Componentes de un sistema de comunicación de datos

Una red de comunicación de datos permite el intercambio de información entre dispositivos mediante hardware y software. Todo sistema de comunicación tiene **cinco componentes**:

```
  ┌──────────┐    [Mensaje]     ┌──────────┐
  │  EMISOR  │ ───────────────▶ │ RECEPTOR │
  └──────────┘                  └──────────┘
       │       [Medio de transmisión]    │
       └──────────[  Protocolo  ]────────┘
```

> 📝 **Lectura del diagrama:** El mensaje viaja del emisor al receptor a través del medio de transmisión, y ambos deben hablar el mismo protocolo para entenderse.
> 

| # | Componente | Descripción | Ejemplos |
| --- | --- | --- | --- |
| 1 | **Emisor** | Dispositivo que envía la información | PC, smartphone, tableta, cámara IP |
| 2 | **Receptor** | Dispositivo que recibe la información | Cualquier dispositivo electrónico |
| 3 | **Mensaje** | Datos que se intercambian; debe tener formato | Texto, imagen, vídeo, audio |
| 4 | **Canal / Medio de transmisión** | Medio físico (tangible o inalámbrico) por donde viaja el mensaje | Fibra óptica, cable coaxial, radiofrecuencia, infrarrojos |
| 5 | **Protocolo** | Conjunto de normas preestablecidas que hacen posible la comunicación | TCP/IP, HTTP, Ethernet |

---

### Flujo de datos (modos de transmisión)

El flujo de datos entre emisor y receptor puede ser de tres tipos:

```
  SIMPLEX      A ──────────────────────▶ B
               (solo un sentido, siempre)

  SEMI-DUPLEX  A ──────────────────────▶ B
               A ◀────────────────────── B
               (ambos pueden, pero NO a la vez)

  FULL-DUPLEX  A ──────────────────────▶ B
               A ◀────────────────────── B
               (ambos transmiten simultáneamente)
```

> 📝 **Lectura:** Las flechas muestran la dirección posible del flujo de datos en cada modo.
> 

| Modo | Dirección | Simultaneidad | Ejemplo real |
| --- | --- | --- | --- |
| **Simplex** | Unidireccional (fija) | — | Ratón de ordenador, teclado |
| **Semi-duplex** | Bidireccional | ❌ No simultánea | Walkie-talkie |
| **Full-duplex** | Bidireccional | ✅ Simultánea | Llamada telefónica, videollamada |

> 💡 **Intuición:** Simplex es una calle de un solo sentido. Semi-duplex es un puente de un carril donde los coches esperan turno. Full-duplex es una autopista de doble vía.
> 

---

### Tipos de conexión entre dispositivos

Los dispositivos se conectan mediante **enlaces** (caminos físicos). Existen dos tipos:

| Tipo | Descripción | Capacidad del canal |
| --- | --- | --- |
| **Punto a punto** | Enlace dedicado y exclusivo entre dos dispositivos | 100% para esa pareja |
| **Multipunto** | Un enlace compartido entre más de dos dispositivos | Compartida; requiere mecanismo de turno (espacial o temporal) |

---

### Tipos de redes según su extensión

**Redes cableadas** (de menor a mayor cobertura):

```
  ┌─────────────────────────────────────────┐
  │               WAN                       │  ← Área Extensa (país, mundo)
  │   ┌─────────────────────────────┐       │
  │   │           MAN               │       │  ← Área Metropolitana (ciudad)
  │   │   ┌─────────────────┐       │       │
  │   │   │       LAN       │       │       │  ← Área Local (edificio, campus)
  │   │   └─────────────────┘       │       │
  │   └─────────────────────────────┘       │
  └─────────────────────────────────────────┘
```

| Sigla | Nombre completo | Cobertura típica |
| --- | --- | --- |
| **LAN** | Local Area Network | Edificio, campus universitario |
| **MAN** | Metropolitan Area Network | Ciudad |
| **WAN** | Wide Area Network | País, continente, mundo |

**Redes inalámbricas** (de menor a mayor cobertura):

| Sigla | Nombre completo | Tecnología típica |
| --- | --- | --- |
| **WPAN** | Wireless Personal Area Network | Bluetooth, Zigbee |
| **WLAN** | Wireless LAN | WiFi (IEEE 802.11) |
| **WMAN** | Wireless MAN | WiMAX |
| **WWAN** | Wireless WAN | Redes móviles (4G, 5G) |

> 💡 **Internet** es la **red de redes** más extensa: interconecta sistemas heterogéneos (distintos hardware, SO y conexiones) gracias a un protocolo común: **TCP/IP**.
> 

---

### 1.3 Arquitectura de protocolos

### ¿Qué es un protocolo?

> Un **protocolo** es un conjunto de reglas y normas que permiten a dos entidades de un sistema de comunicación intercambiar información.
> 

Los protocolos actúan a **distintos niveles**: desde el nivel físico (cómo se transmiten los bits) hasta niveles más altos (cómo se empaqueta la información, con direcciones de origen y destino).

---

### El modelo OSI

El **modelo OSI** (*Open Systems Interconnection* — Interconexión de Sistemas Abiertos) organiza la complejidad de las comunicaciones en **7 capas**. Cada capa proporciona servicios a la capa superior y usa los servicios de la capa inferior.

```
  Emisor                                        Receptor
  ┌─────────────────┐                        ┌─────────────────┐
  │ 7. Aplicación   │ ◀── datos ──────────── │ 7. Aplicación   │
  ├─────────────────┤                        ├─────────────────┤
  │ 6. Presentación │ ◀── datos ──────────── │ 6. Presentación │
  ├─────────────────┤                        ├─────────────────┤
  │ 5. Sesión       │ ◀── datos ──────────── │ 5. Sesión       │
  ├─────────────────┤                        ├─────────────────┤
  │ 4. Transporte   │ ◀── segmentos ──────── │ 4. Transporte   │
  ├─────────────────┤                        ├─────────────────┤
  │ 3. Red          │ ◀── paquetes/datagram. │ 3. Red          │
  ├─────────────────┤                        ├─────────────────┤
  │ 2. Enlace datos │ ◀── tramas ─────────── │ 2. Enlace datos │
  ├─────────────────┤                        ├─────────────────┤
  │ 1. Física       │ ◀── bits ───────────── │ 1. Física       │
  └─────────────────┘                        └─────────────────┘
```

> 📝 **Lectura del diagrama:** La comunicación real ocurre solo en la capa física (abajo). Las capas superiores son abstracciones lógicas: cada capa "habla" directamente con su par en el otro extremo, pero los datos físicamente bajan por todas las capas del emisor y suben por todas las del receptor.
> 

| Capa | Nombre | PDU | Función principal |
| --- | --- | --- | --- |
| 7 | **Aplicación** | Datos | Interfaz con el usuario; HTTP, FTP, DNS |
| 6 | **Presentación** | Datos | Formato, cifrado, compresión |
| 5 | **Sesión** | Datos | Control de diálogo entre aplicaciones |
| 4 | **Transporte** | Segmentos | Entrega extremo a extremo; TCP, UDP |
| 3 | **Red** | Paquetes / Datagramas | Encaminamiento entre redes; IP |
| 2 | **Enlace de datos** | Tramas (*frames*) | Transmisión fiable entre nodos adyacentes |
| 1 | **Física** | Bits | Transmisión de señales eléctricas/ópticas |

> 💡 **Regla mnemotécnica (de arriba a abajo):** **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing → Aplicación, Presentación, Sesión, Transporte, Red (*Network*), Datos (*Data link*), Física (*Physical*).
> 

---

### TCP/IP vs OSI

El modelo TCP/IP es el que realmente se usa en Internet. Simplifica las 7 capas OSI en **4 capas**:

```
  Modelo OSI (7 capas)        Modelo TCP/IP (4 capas)
  ┌──────────────────┐        ┌──────────────────────┐
  │  7. Aplicación   │        │                      │
  ├──────────────────┤   ──▶  │    4. Aplicación     │
  │  6. Presentación │        │   (HTTP, FTP, DNS…)  │
  ├──────────────────┤        │                      │
  │  5. Sesión       │        └──────────────────────┘
  ├──────────────────┤        ┌──────────────────────┐
  │  4. Transporte   │   ──▶  │   3. Transporte      │
  ├──────────────────┤        │   (TCP, UDP)         │
  ├──────────────────┤        └──────────────────────┘
  │  3. Red          │   ──▶  ┌──────────────────────┐
  ├──────────────────┤        │   2. Internet (IP)   │
  ├──────────────────┤        └──────────────────────┘
  │  2. Enlace datos │   ──▶  ┌──────────────────────┐
  ├──────────────────┤        │   1. Acceso a red    │
  │  1. Física       │        │   (Ethernet, WiFi…)  │
  └──────────────────┘        └──────────────────────┘
```

> 📝 **Lectura:** Las capas 5, 6 y 7 del OSI se fusionan en la capa de Aplicación de TCP/IP. Las capas 1 y 2 se fusionan en Acceso a red.
> 

---

### Estándares IEEE

El **IEEE** (*Instituto de Ingenieros Eléctricos y Electrónicos*) regula los estándares para redes locales, metropolitanas y de área extensa.

| Estándar | Tipo de red | Tecnología / Velocidades |
| --- | --- | --- |
| **IEEE 802.3** | LAN cableada (Ethernet) | 10 Mbps / 100 Mbps (Fast) / 1 Gbps (Gigabit) / 10 Gbps |
| **IEEE 802.11** | LAN inalámbrica (WiFi) | 802.11a: 54 Mbps @5GHz · 802.11b: 11 Mbps @2.4GHz · 802.11g: 54 Mbps · 802.11n: 300 Mbps · 802.11ac: 866 Mbps |
| **IEEE 802.15** | WPAN (área personal) | Bluetooth, UWB, Zigbee |
| **IEEE 802.16** | WMAN (área metropolitana) | WiMAX, LMDS |

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Red de comunicación de datos** | Sistema de hardware y software que permite el intercambio de información entre dispositivos |
| **Protocolo** | Conjunto de reglas y normas preestablecidas que permiten la comunicación entre dos entidades |
| **Simplex** | Modo de transmisión unidireccional; solo un extremo emite |
| **Semi-duplex** | Ambos extremos pueden emitir/recibir, pero no simultáneamente |
| **Full-duplex** | Ambos extremos transmiten y reciben al mismo tiempo |
| **Enlace punto a punto** | Canal dedicado exclusivamente a la comunicación entre dos dispositivos |
| **Enlace multipunto** | Canal compartido entre más de dos dispositivos |
| **LAN** | Local Area Network; red de área local (edificio, campus) |
| **MAN** | Metropolitan Area Network; red de área metropolitana (ciudad) |
| **WAN** | Wide Area Network; red de área extensa (país, mundo) |
| **ISP** | *Internet Service Provider*; proveedor de acceso a Internet |
| **Modelo OSI** | Modelo de 7 capas que describe la arquitectura de comunicaciones en redes |
| **PDU** | *Protocol Data Unit*; unidad de datos de cada capa (bits, tramas, datagramas, segmentos, datos) |
| **Jitter** | Variabilidad en los tiempos de llegada de los paquetes; perjudicial en streaming de vídeo/audio |
| **IEEE** | Instituto de Ingenieros Eléctricos y Electrónicos; organismo que regula los estándares de red |

---

## 🛠️ Topologías de red

La **topología** describe la estructura física o lógica de cómo están interconectados los dispositivos de una red.

```
  MALLA                   ANILLO
  A───B                   A
  │╲ ╱│                   │
  │ X │                B──┤──D
  │╱ ╲│                   │
  C───D                   C

  ESTRELLA                BUS
      A                A──B──C──D──E
      │                ─────────────── (cable compartido)
  B───HUB───C
      │
      D

  HÍBRIDA: combinación de dos o más topologías anteriores
```

> 📝 **Lectura:** Cada topología tiene ventajas y desventajas en términos de cableado necesario, tolerancia a fallos y complejidad de gestión.
> 

| Topología | Tipo de conexión | Ventaja principal | Desventaja principal |
| --- | --- | --- | --- |
| **Malla** | Punto a punto (todos con todos) | Alta redundancia y tolerancia a fallos | Mucho cableado: n(n-1)/2 enlaces |
| **Anillo** | Punto a punto (en cadena circular) | Fácil de implementar | Si un nodo falla, la red puede caer |
| **Estrella** | Punto a punto (todos al centro) | Fácil gestión y aislamiento de fallos | Depende del nodo central (hub/switch) |
| **Bus** | Multipunto (cable compartido) | Poco cableado | Colisiones; si el bus falla, toda la red cae |
| **Híbrida** | Mixto | Flexibilidad | Mayor complejidad |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **Semi-duplex ≠ Simplex**: en semi-duplex ambos extremos *pueden* transmitir, pero no a la vez. En simplex, solo uno puede hacerlo.
- ❗ **OSI ≠ TCP/IP**: OSI es un modelo teórico de referencia con 7 capas; TCP/IP es el modelo real usado en Internet con 4 capas.
- ✅ En una **topología en malla** con n nodos se necesitan **n(n-1)/2** enlaces y cada nodo necesita **n-1** puertos.
- ✅ El **jitter** (variabilidad en tiempos de llegada) es especialmente dañino en aplicaciones en tiempo real como videoconferencias o videojuegos online.
- 🔁 Los **protocolos IEEE 802.x** que se verán en temas posteriores corresponden a la capa de Enlace de datos y Física del modelo OSI.
- ❗ **ISP** significa *Internet Service Provider* (proveedor de acceso a Internet), no es ningún protocolo.

---

## 🔗 Conexiones con otros temas

- **Tema 5 (Capa de red):** La capa de red del modelo OSI/TCP-IP usa el protocolo IP, que trabaja con datagramas. Conecta directamente con lo visto en el esquema de capas.
- **Protocolos de transporte (TCP/UDP):** La capa de transporte (Nivel 4 OSI) será estudiada en profundidad más adelante; TCP añade fiabilidad sobre IP.
- **LAN y WiFi (IEEE 802.3 / 802.11):** Los estándares del IEEE se estudiarán en detalle en los temas de capas de enlace y física.

---

## 📝 Preguntas de repaso

1. ¿Cuáles son los cinco componentes de un sistema de comunicación de datos y qué función tiene cada uno?
2. ¿Qué diferencia hay entre los modos de transmisión simplex, semi-duplex y full-duplex? Da un ejemplo de cada uno.
3. ¿En qué se diferencian las conexiones punto a punto y multipunto?
4. ¿Qué significa LAN, MAN y WAN? ¿Y WPAN, WLAN, WMAN, WWAN?
5. ¿Por qué Internet se denomina "red de redes"? ¿Qué protocolo la hace posible?
6. ¿Qué es un protocolo de comunicaciones y por qué es necesario?
7. ¿Cuántas capas tiene el modelo OSI? Nómbralas de mayor a menor nivel.
8. ¿Cómo se llama la PDU (unidad de datos) en cada capa del modelo OSI?
9. ¿Qué diferencia hay entre el modelo OSI y el modelo TCP/IP?
10. ¿Qué es el jitter y por qué afecta a las transmisiones multimedia?
11. ¿Cuántos enlaces se necesitan en una topología en malla con n dispositivos?
12. ¿Qué organismo define los estándares IEEE 802.x y para qué sirven?

---

## 📌 Resumen en una página

> Las **redes de comunicación de datos** permiten intercambiar información entre dispositivos a través de un sistema formado por cinco elementos: emisor, receptor, mensaje, medio de transmisión y protocolo. El flujo de datos puede ser **simplex** (un solo sentido), **semi-duplex** (bidireccional pero no simultáneo) o **full-duplex** (bidireccional y simultáneo). Las conexiones entre dispositivos son **punto a punto** (canal dedicado) o **multipunto** (canal compartido). Las redes se clasifican por extensión en **LAN, MAN y WAN** (cableadas) y **WPAN, WLAN, WMAN, WWAN** (inalámbricas); **Internet** es la red de redes que las une todas mediante **TCP/IP**. Para gestionar la complejidad de las comunicaciones, se usan arquitecturas de protocolos en capas: el **modelo OSI** (7 capas teóricas) y el **modelo TCP/IP** (4 capas prácticas). Cada capa tiene su PDU propia (bits, tramas, datagramas, segmentos, datos) y el organismo **IEEE** define los estándares técnicos para su implementación física (Ethernet, WiFi, Bluetooth, WiMAX).
>