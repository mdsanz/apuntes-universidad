# La Capa de Enlace

> **Unidad:** Tema 6
> 
> 
> **Palabras clave:** `capa de enlace`, `trama`, `frame`, `MAC`, `ARP`, `CRC`, `CSMA/CD`, `TDM`, `FDM`, `CDMA`, `ALOHA`, `detección de errores`, `acceso múltiple`
> 

---

## 🎯 Objetivo de la clase

Estudiar la capa de enlace (Nivel 2 OSI): su función de transmisión de tramas entre nodos adyacentes, las técnicas de detección y corrección de errores, los protocolos de acceso múltiple al medio compartido, y el direccionamiento a nivel de enlace mediante direcciones MAC y el protocolo ARP.

---

## 🧠 Conceptos principales

### 6.2 La capa de enlace y técnicas de detección y corrección de errores

### ¿Qué hace la capa de enlace?

La **capa de enlace** se ocupa de transmitir **tramas** (*frames*) binarias en cada uno de los enlaces individuales que forman una ruta de red. Toma los datagramas que le entrega la capa de red y los encapsula en tramas para enviarlos por el enlace físico correspondiente.

```
  Nodo A            Enlace 1          Router          Enlace 2          Nodo B
  ┌──────┐   trama Ethernet    ┌──────────┐   trama WiFi      ┌──────┐
  │      │──────────────────── │          │──────────────────── │      │
  │ capa │   [MAC dest][MAC or]│  encamin │[MAC dest][MAC or]  │ capa │
  │enlace│   [IP dest][IP or]  │  -ador   │[IP dest][IP or]    │enlace│
  └──────┘   [datos][CRC]      └──────────┘[datos][CRC]        └──────┘
```

> 📝 **Lectura:** Cada enlace de la ruta puede usar un protocolo de enlace distinto (Ethernet, WiFi, fibra, etc.), con tramas de formato diferente. El datagrama IP va encapsulado dentro de cada trama pero permanece igual de extremo a extremo.
> 

> 💡 **Clave:** Una misma ruta puede combinar Ethernet + fibra óptica + WiFi. Cada tramo usa el protocolo de enlace adecuado a su medio físico.
> 

---

### Detección y corrección de errores

En los enlaces físicos, la señal puede degradarse por **atenuación, interferencias o ruido electromagnético**, alterando algunos bits de las tramas. La capa de enlace ofrece dos niveles de protección:

```
  Nodo emisor                                    Nodo receptor
  ┌─────────────────────┐                       ┌─────────────────────┐
  │ Datos               │                       │ Datos               │
  │ + bits EDC          │──── enlace físico ───▶│ Verifica con EDC    │
  │ (detección/correcc) │    (puede haber ruido) │ ¿Error? Sí/No       │
  └─────────────────────┘                       └─────────────────────┘
  EDC = Error Detection and Correction bits
```

| Mecanismo | ¿Detecta errores? | ¿Corrige errores? | Complejidad |
| --- | --- | --- | --- |
| **Bits de paridad** | ✅ Sí (errores simples) | ❌ No | Baja |
| **CRC** (Redundancia cíclica) | ✅ Sí (muy fiable) | ❌ No (solo detecta) | Media |
| **Códigos correctores** | ✅ Sí | ✅ Sí (localiza el bit erróneo) | Alta |
- **Detección de errores:** el emisor añade **bits EDC** adicionales; el receptor los recalcula y compara para saber si hubo error.
- **Corrección de errores:** mecanismo más sofisticado que además identifica **en qué posición** ocurrió el error, permitiendo corregirlo sin retransmitir.

> 💡 **Intuición del CRC:** Es como una "huella digital" matemática de los datos. Si la huella al llegar no coincide con la recalculada, los datos están corruptos.
> 

> ⚠️ Ningún mecanismo garantiza detección al 100%. El objetivo es hacer la probabilidad de error no detectado **sumamente improbable**, no cero.
> 

---

### 6.3 Enlaces de acceso múltiple y protocolos

### Tipos de canales

| Tipo | Descripción | Ejemplos |
| --- | --- | --- |
| **Punto a punto** | Canal dedicado entre exactamente dos nodos | Router doméstico ↔ router del ISP |
| **Difusión** (*broadcast*) | Canal compartido por varios nodos simultáneamente | WiFi, Ethernet (con hub), satélite, HFC |

En los **canales de difusión** varios nodos comparten el medio, lo que puede producir **colisiones** si dos transmiten a la vez. Esto exige un **protocolo de control de acceso al medio (MAC)**.

---

### Clasificación de protocolos de acceso múltiple

```
  Protocolos de acceso al medio
  │
  ├── 1. Particionamiento del canal
  │        ├── TDM  (tiempo)
  │        ├── FDM  (frecuencia)
  │        └── CDMA (códigos)
  │
  ├── 2. Acceso aleatorio
  │        ├── ALOHA
  │        ├── CSMA
  │        └── CSMA/CD  ← usado en Ethernet
  │
  └── 3. Turnos
           ├── Sondeo / Polling
           └── Paso de testigo / Token
```

---

### Grupo 1: Particionamiento del canal

Dividen el canal en "porciones" asignadas a cada nodo. No hay colisiones, pero si un nodo no transmite, su porción se desperdicia.

| Protocolo | Divide por… | Funcionamiento | Ejemplo |
| --- | --- | --- | --- |
| **TDM** | Tiempo | Cada nodo transmite en su franja temporal asignada, aunque esté ocioso | Telefonía tradicional |
| **FDM** | Frecuencia | Cada nodo usa una frecuencia distinta del canal (menor capacidad individual) | Radio FM, TV por cable |
| **CDMA** | Códigos | Todos transmiten simultáneamente en la misma frecuencia, usando códigos ortogonales | Telefonía móvil 3G |

```
  FDM (Frecuencia):              TDM (Tiempo):
  ─────────────────              ─────────────────
  Nodo A: frecuencia 1           |A|B|C|A|B|C|A|B|
  Nodo B: frecuencia 2           └─────────────────▶ tiempo
  Nodo C: frecuencia 3           cada nodo espera su turno
  Todos al mismo tiempo
```

> 📝 **Lectura:** FDM divide el "pastel" en franjas de frecuencia. TDM lo divide en franjas de tiempo. CDMA permite a todos usar todo el pastel a la vez con códigos diferentes.
> 

---

### Grupo 2: Acceso aleatorio

Cada nodo transmite cuando quiere, a la velocidad total del canal. Si hay colisión, espera un tiempo aleatorio y reintenta.

**ALOHA:** el protocolo más simple. Transmite sin escuchar. Alta probabilidad de colisión.

**CSMA** (*Carrier Sense Multiple Access* — Detección de portadora antes de transmitir):

- Antes de transmitir, **escucha** el canal.
- Si está libre → transmite.
- Si está ocupado → espera.
- No elimina colisiones del todo (el retardo de propagación puede hacer que dos nodos no "oigan" la transmisión del otro a tiempo).

**CSMA/CD** (*CSMA with Collision Detection* — con detección de colisión):

- Igual que CSMA, pero **mientras transmite** sigue escuchando.
- Si detecta colisión → para de transmitir y envía una señal de **jamming** para avisar a todos.
- Espera un tiempo aleatorio (*backoff*) y reintenta.
- **Es el protocolo de Ethernet** (IEEE 802.3).

```
  CSMA/CD — Flujo de decisión:

  ┌─────────────────────────────────────────┐
  │ ¿Quiero transmitir?                     │
  └──────────────┬──────────────────────────┘
                 │
         ┌───────▼────────┐
         │ ¿Canal libre?  │
         └───┬────────┬───┘
            SÍ       NO
             │        └──▶ [Esperar y reintentar]
    ┌────────▼────────┐
    │   Transmitir    │
    └────────┬────────┘
             │
    ┌────────▼──────────────┐
    │ ¿Detecto colisión?    │
    └───┬──────────────┬────┘
       SÍ             NO
        │               └──▶ [Transmisión exitosa]
  ┌─────▼──────────────────┐
  │ Señal jamming + parar  │
  │ Esperar tiempo aleatorio│
  │ Reintentar             │
  └────────────────────────┘
```

> 💡 **Analogía CSMA/CD:** Es como una reunión donde todos hablan. CSMA es "espera a que nadie hable para hablar tú". CSMA/CD añade "si colisionas con alguien que también empezó a la vez, los dos paran, esperan un tiempo aleatorio distinto, y el que menos esperó habla primero".
> 

---

### Grupo 3: Turnos

Evitan colisiones coordinando explícitamente el acceso. Eficientes cuando el canal está cargado.

| Protocolo | Mecanismo | Ventaja | Inconveniente |
| --- | --- | --- | --- |
| **Sondeo / Polling** | Un nodo maestro invita a cada nodo a transmitir por turno | Sin colisiones | Latencia; el maestro es punto de fallo |
| **Paso de testigo / Token** | Un token circula; solo transmite quien lo tiene | Sin colisiones, equitativo | Si el token se pierde, hay que regenerarlo |

---

### 6.4 Direccionamiento a nivel de enlace

### Dirección MAC

Cada adaptador de red (tarjeta de red) tiene asignada una **dirección MAC** (*Medium Access Control*), también llamada dirección física o dirección LAN.

| Característica | Valor |
| --- | --- |
| **Tamaño** | 6 bytes (48 bits) |
| **Formato** | Hexadecimal separado por guiones o dos puntos |
| **Ejemplo** | `5C-66-23-FB-06-8A` |
| **Unicidad** | Diseñadas para ser únicas y permanentes (asignadas en fábrica) |
| **¿Se puede cambiar?** | Sí, con software (usado en hacking) |

> 💡 **Intuición:** La dirección MAC es como el número de serie grabado en el hardware — no cambia cuando el dispositivo se mueve de red, a diferencia de la IP que sí cambia.
> 

---

### MAC vs IP: diferencia clave

```
  ┌──────────────────────────────────────────────────────────────┐
  │                    Trama Ethernet                            │
  │  ┌────────────┬────────────┬────────────────────┬─────────┐ │
  │  │MAC destino │MAC origen  │   Datagrama IP     │Trailer  │ │
  │  │(6 bytes)   │(6 bytes)   │┌──────────────────┐│(CRC)    │ │
  │  │            │            ││IP dest │IP orig   ││         │ │
  │  │            │            ││(4 b)   │(4 b)     ││         │ │
  │  │            │            │└──────────────────┘│         │ │
  │  └────────────┴────────────┴────────────────────┴─────────┘ │
  └──────────────────────────────────────────────────────────────┘

  MAC → identifica el adaptador en el enlace actual (nivel 2)
  IP  → identifica el nodo en la red global (nivel 3, extremo a extremo)
```

> 📝 **Lectura:** La trama lleva MAC origen y destino (para el enlace actual) e IP origen y destino (para la ruta completa). Las MACs cambian en cada salto; las IPs permanecen constantes de extremo a extremo.
> 

---

### Dirección de difusión MAC

Para enviar una trama a **todos los nodos** del canal de difusión se usa la dirección especial:

```
FF-FF-FF-FF-FF-FF
```

Todos los adaptadores aceptan y procesan las tramas dirigidas a esta dirección.

---

### Protocolo ARP

El problema: el emisor conoce la **IP destino** (de la capa de red), pero necesita la **dirección MAC** del nodo siguiente para construir la trama. ARP resuelve esta traducción.

**ARP** (*Address Resolution Protocol*) — Protocolo de resolución de direcciones:

```
  Nodo A quiere saber la MAC del nodo con IP 192.168.1.5

  Paso 1 — Petición ARP (difusión):
  ┌──────┐  ¿Quién tiene IP 192.168.1.5?  ┌──────┐ ┌──────┐ ┌──────┐
  │Nodo A│ ──────── FF:FF:FF:FF:FF:FF ──▶ │Nodo B│ │Nodo C│ │Nodo D│
  └──────┘  (todos la reciben)             └──────┘ └──────┘ └──────┘
                                               │
  Paso 2 — Respuesta ARP (unicast):            │ "¡Soy yo! Mi MAC es AA:BB:CC:DD:EE:FF"
  ┌──────┐ ◀────────────────────────────────── ┘
  │Nodo A│  (solo responde el dueño de esa IP)
  └──────┘

  Paso 3 — Nodo A guarda en su Tabla ARP:
  ┌─────────────────┬───────────────────┐
  │ IP              │ MAC               │
  ├─────────────────┼───────────────────┤
  │ 192.168.1.5     │ AA:BB:CC:DD:EE:FF │
  └─────────────────┴───────────────────┘
```

> 📝 **Lectura:** La petición ARP va en **difusión** (todos la oyen). La respuesta ARP va en **unicast** (solo al que preguntó). La tabla ARP es la caché local de traducciones IP↔MAC.
> 

> ⚠️ ARP actúa solo dentro de la **misma subred**. Es diferente de DNS en que DNS resuelve nombres a IPs globalmente, mientras ARP resuelve IPs a MACs localmente.
> 

> ⚠️ ARP es técnicamente un protocolo que está entre la capa de enlace y la capa de red; no sigue estrictamente la separación por capas del modelo OSI, pero es inevitable.
> 

---

### Seguridad: ARP Poisoning

El **envenenamiento ARP** (*ARP Poisoning*) es un ataque en que un nodo malicioso responde falsamente a peticiones ARP, asociando su MAC a la IP de otro nodo. Esto permite redirigir el tráfico de la víctima hacia el atacante (ataque *man in the middle*).

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Trama / Frame** | Unidad de datos de la capa de enlace; encapsula un datagrama IP con cabeceras MAC y bits de control de errores |
| **CRC** | *Cyclic Redundancy Check*; código de redundancia cíclica usado para detectar errores en tramas |
| **Canal de difusión** | Canal compartido por múltiples nodos donde todos reciben lo que cualquiera transmite |
| **Colisión** | Interferencia producida cuando dos nodos transmiten simultáneamente en un canal compartido |
| **CSMA/CD** | Protocolo de acceso múltiple con detección de portadora y de colisión; usado en Ethernet |
| **Jamming** | Señal que un nodo emite al detectar una colisión para avisar a todos los demás de que paren |
| **TDM** | *Time Division Multiplexing*; multiplexación por división en el tiempo |
| **FDM** | *Frequency Division Multiplexing*; multiplexación por división en frecuencia |
| **CDMA** | *Code Division Multiple Access*; acceso múltiple por división de códigos |
| **Token / Testigo** | Trama especial que circula por la red; solo el nodo que lo posee puede transmitir |
| **Dirección MAC** | Dirección física de 6 bytes asignada a cada adaptador de red; única y permanente de fábrica |
| **ARP** | *Address Resolution Protocol*; protocolo que traduce direcciones IP a direcciones MAC dentro de una subred |
| **Tabla ARP** | Caché local en cada nodo con las correspondencias IP↔MAC conocidas |
| **ARQ** | *Automatic Repeat reQuest*; protocolo que convierte un enlace no fiable en fiable mediante retransmisiones |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **Los protocolos de enlace pueden ser distintos en cada tramo** de una ruta — no tienen que ser el mismo en toda la comunicación.
- ❗ **MAC ≠ IP**: la MAC identifica el adaptador en el enlace local (cambia en cada salto); la IP identifica el nodo en la red global (permanece igual de extremo a extremo).
- ✅ **CSMA/CD es el protocolo de Ethernet** (IEEE 802.3). Es acceso aleatorio con detección de colisión.
- ❗ **ARP solo funciona dentro de la misma subred**. Para comunicarse con un nodo de otra subred, la MAC destino será la del router (gateway), no la del nodo final.
- ✅ La **dirección de difusión MAC** `FF:FF:FF:FF:FF:FF` hace que todos los adaptadores del canal procesen la trama.
- ❗ Aunque las MACs se diseñaron como únicas y permanentes, **hoy se pueden modificar por software** — esto tiene implicaciones de seguridad (ARP Poisoning, MAC Spoofing).
- 🔁 CRC solo **detecta** errores; no los corrige. Para corrección se necesitan códigos más sofisticados (como los códigos de Hamming).
- ✅ La **petición ARP es broadcast**; la **respuesta ARP es unicast** — dato clave para el test.

---

## 🔗 Conexiones con otros temas

- **Tema 1 (Modelo OSI):** La capa de enlace es el Nivel 2 del OSI, entre la capa física (Nivel 1) y la capa de red (Nivel 3).
- **Tema 5 (Capa de red - IP):** IP entrega datagramas a la capa de enlace para su transmisión en cada tramo. ARP conecta ambas capas traduciendo IPs a MACs.
- **IEEE 802.3 (Ethernet) / 802.11 (WiFi):** Los estándares vistos en el Tema 1 definen los protocolos concretos de la capa de enlace para LAN cableada e inalámbrica.

---

## 📝 Preguntas de repaso

1. ¿Qué función tiene la capa de enlace? ¿Cuál es su PDU (unidad de datos)?
2. ¿Pueden usarse protocolos de enlace distintos en los diferentes tramos de una ruta? ¿Por qué?
3. ¿Cuál es la diferencia entre detección y corrección de errores? ¿Qué técnica se usa para cada una?
4. ¿Qué son los canales de difusión y por qué necesitan protocolos de acceso múltiple?
5. Describe los tres grupos de protocolos de acceso múltiple con un ejemplo de cada uno.
6. ¿Qué diferencia hay entre CSMA y CSMA/CD? ¿Para qué sirve la señal de jamming?
7. ¿Qué protocolo de acceso múltiple usa Ethernet?
8. ¿Cuántos bytes tiene una dirección MAC? Da un ejemplo en formato hexadecimal.
9. ¿Cuál es la diferencia entre una dirección MAC y una dirección IP?
10. ¿Cuál es la dirección MAC de difusión y cuándo se usa?
11. ¿Qué hace el protocolo ARP? ¿La petición ARP va en unicast o en difusión?
12. ¿En qué se diferencia ARP de DNS?

---

## 📌 Resumen en una página

> La **capa de enlace** (Nivel 2 OSI) transmite **tramas** (*frames*) entre nodos adyacentes en cada enlace de una ruta. Los distintos tramos de una misma comunicación pueden usar protocolos de enlace diferentes (Ethernet, WiFi, fibra). Para proteger la integridad de los datos, la capa de enlace usa **detección de errores** (bits adicionales como CRC que permiten detectar bits alterados) y **corrección de errores** (mecanismos más complejos que localizan y corrigen los bits erróneos). En los **canales de difusión** (compartidos por varios nodos), es necesario coordinar el acceso mediante protocolos MAC agrupados en tres familias: **particionamiento del canal** (TDM, FDM, CDMA — dividen el canal en porciones), **acceso aleatorio** (ALOHA, CSMA, **CSMA/CD** — el de Ethernet — que transmiten libremente y gestionan colisiones) y **turnos** (polling, paso de testigo — coordinación explícita). A nivel de direccionamiento, cada adaptador de red tiene una **dirección MAC** única de 6 bytes. El protocolo **ARP** traduce direcciones IP a MACs dentro de una subred mediante peticiones en difusión (`FF:FF:FF:FF:FF:FF`) y respuestas unicast, guardando las correspondencias en una **tabla ARP** local.
>