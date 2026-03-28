# La Capa de Red

> **Unidad:** Tema 5
> 
> 
> **Palabras clave:** `capa de red`, `IP`, `IPv4`, `IPv6`, `conmutación`, `encaminamiento`, `datagramas`, `circuitos virtuales`, `routing`
> 

---

## 🎯 Objetivo de la clase

Estudiar los protocolos de comunicación a nivel de red, comprendiendo los mecanismos de conmutación de paquetes y circuitos, el funcionamiento del protocolo IP (v4 y v6), y los distintos algoritmos de encaminamiento según la fuente de decisión.

---

## 🧠 Conceptos principales

### 5.2 Conmutación de circuitos y paquetes

La **conmutación** define cómo se transporta la información (mediante reenvíos) a través de una red. Existen dos grandes técnicas:

---

### 1. Conmutación de circuitos

Se establece una **conexión dedicada** entre origen y destino **antes** de comenzar la transmisión.

- Siempre se usa la **misma ruta** durante toda la transferencia.
- Al finalizar, la conexión se **cierra** y los recursos reservados se liberan.
- Los recursos se usan **en exclusiva**: aunque no haya datos, están ocupados.

> 💡 **Intuición:** Es como una llamada telefónica tradicional — primero marcas el número, se establece el canal, hablas, y al colgar se libera la línea.
> 

---

### 2. Conmutación de paquetes

La información se **fragmenta en paquetes** que se retransmiten de nodo en nodo hasta llegar al destino. Existen dos variantes:

| Tipo | ¿Conexión previa? | Descripción |
| --- | --- | --- |
| **Mediante datagramas** | ❌ No | Cada paquete viaja de forma independiente y puede tomar rutas distintas |
| **Mediante circuitos virtuales** | ✅ Sí | Se establece una ruta lógica antes de transmitir, pero los recursos no son exclusivos |

> 💡 **Intuición:** Los datagramas son como cartas enviadas por correo — cada una puede tomar un camino diferente. Los circuitos virtuales son como reservar asientos en un tren sin que nadie más los ocupe.
> 

```
  EMISOR                                          RECEPTOR
    │                                                │
    │──[Paquete 1: R1→R2→R5]──────────────────────▶ │
    │──[Paquete 2: R1→R3→R6]──────────────────────▶ │
    │──[Paquete 3: R1→R4→R3→R6]──────────────────▶  │
    │                                                │
         Cada paquete puede tomar una ruta diferente
         (conmutación por datagramas)
```

> 📝 **Lectura del diagrama:** En una red de datagramas, los paquetes del mismo mensaje pueden llegar por caminos distintos y en distinto orden; el receptor los reensambla.
> 

---

### 5.3 El protocolo IP (v4 y v6)

El **protocolo Internet (IP)** opera en la capa de red y es el responsable del **encaminamiento** de los datos. Sus características principales son:

| Característica | Descripción |
| --- | --- |
| **No orientado a conexión** | No establece conexión previa; usa datagramas |
| **No fiable** | No implementa control de errores ni de flujo |
| **Best effort** | Hace el máximo esfuerzo para entregar los datos |
| **PDU** | Las unidades se llaman **datagramas IP** |

> ⚠️ El control de errores y de flujo se **delegan a capas superiores** (ej. TCP), lo que reduce la carga de trabajo en la red.
> 

---

### IPv4 vs IPv6

| Característica | IPv4 | IPv6 |
| --- | --- | --- |
| **Tamaño de dirección** | 32 bits | 128 bits |
| **Ejemplo de dirección** | `192.168.1.1` | `2001:0db8::1` |
| **Espacio de direcciones** | ~4.3 mil millones | ~3.4 × 10³⁸ |

---

### Formato del datagrama IPv4

```
 0        8       16       24      31
 ┌────────┬────────┬────────────────┐
 │Versión │  IHL   │Tipo de servicio│  Longitud total   │
 ├────────┴────────┴────────────────┤
 │     Identificación    │Flags│Desplazamiento         │
 ├───────────────────────┴─────┴───────────────────────┤
 │Tiempo de vida │ Protocolo  │  Suma de comprobación   │
 ├───────────────┴────────────┴────────────────────────┤
 │                Dirección de la fuente               │
 ├─────────────────────────────────────────────────────┤
 │                Dirección del destino                │
 ├─────────────────────────────────────────────────────┤
 │           Opciones              │      Relleno       │
 └─────────────────────────────────────────────────────┘
```

> 📝 **Lectura del diagrama:** Cada fila representa 32 bits. Los campos más importantes para el encaminamiento son la **dirección de la fuente** y la **dirección del destino**.
> 

---

### Fragmentación de datagramas IP

Cuando un datagrama debe atravesar redes con distintas **MTU** (*Maximum Transfer Unit* — Unidad Máxima de Transferencia), se **fragmenta** para poder encapsularse y enviarse a través de cada red física.

> 💡 **Intuición:** Es como dividir una caja grande en paquetes más pequeños para que quepan en un camión de menor capacidad. Al llegar al destino, se reensamblan.
> 

---

### 5.4 Algoritmos de encaminamiento

El **encaminamiento (routing)** es la función que determina la **ruta óptima** que deben seguir los datos entre un nodo origen y un nodo destino.

---

### Clasificación según la fuente de decisión

```
                  Algoritmos de Encaminamiento
                           │
         ┌─────────────────┼──────────────────┐
         │                 │                  │
  Centralizados        Aislados          Distribuidos
  (un nodo central    (cada nodo,       (cada nodo +
   calcula todo)     solo info local)    sus vecinos)
                                              │
                                         Jerárquicos
                                      (variante distribuida
                                       con regiones)
```

> 📝 **Lectura del diagrama:** La clasificación va de mayor centralización (izquierda) a mayor descentralización (derecha).
> 

---

### Descripción de cada tipo

| Tipo | ¿Quién decide? | Información usada | Ejemplo |
| --- | --- | --- | --- |
| **Centralizado** | Un único nodo de la red | Global | Nodo maestro con tabla de toda la red |
| **Aislado** | Cada nodo individualmente | Solo local | **Flooding** (inundación) |
| **Distribuido** | Cada nodo, coordinando con vecinos | Local + vecinos | RIP, OSPF |
| **Jerárquico** | Nodos dentro de regiones + entre regiones | Regional + inter-regional | BGP (entre regiones/AS) |

---

### Algoritmo de inundación (Flooding)

Es el ejemplo más conocido de encaminamiento **aislado**:

**Idea general:** Cada nodo reenvía el paquete por **todos** sus enlaces (excepto por el que llegó).

**Pasos:**

1. El nodo recibe un paquete.
2. Lo retransmite por **todos** los demás enlaces.
3. Para evitar bucles infinitos, se usa un contador de saltos (TTL) o un número de secuencia.

**Ventaja:** Garantiza que el paquete llegue si existe alguna ruta.

**Desventaja:** Genera mucho tráfico redundante en la red.

---

### Encaminamiento jerárquico

Resuelve la **escalabilidad** en redes grandes mediante dos niveles:

```
  ┌──────────────────────────────────────────────────┐
  │                   Red Global                     │
  │   ┌───────────┐          ┌───────────┐           │
  │   │ Región A  │◀────────▶│ Región B  │           │
  │   │  (nodos   │          │  (nodos   │           │
  │   │ internos) │          │ internos) │           │
  │   └───────────┘          └───────────┘           │
  │          ▲                     ▲                  │
  │   Routing distribuido    Routing distribuido      │
  │   dentro de la región   dentro de la región       │
  └──────────────────────────────────────────────────┘
```

> 📝 **Lectura:** Dentro de cada región los nodos intercambian info entre sí (distribuido). Entre regiones, los nodos frontera actúan como representantes.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Conmutación de circuitos** | Técnica donde se reserva un canal exclusivo entre origen y destino antes de transmitir |
| **Conmutación de paquetes** | Técnica donde la información se fragmenta en paquetes independientes |
| **Datagramas** | Paquetes que se enrutan de forma independiente sin conexión previa |
| **Circuito virtual** | Ruta lógica preestablecida para la transmisión de paquetes |
| **IP (Internet Protocol)** | Protocolo de capa de red, no orientado a conexión, no fiable, best effort |
| **PDU (Protocol Data Unit)** | Unidad de datos del protocolo; en IP se llaman datagramas IP |
| **MTU** | Maximum Transfer Unit — tamaño máximo de paquete que soporta una red física |
| **Fragmentación IP** | División de un datagrama en partes más pequeñas para adaptarse a la MTU |
| **Routing / Encaminamiento** | Proceso de determinar la ruta óptima entre origen y destino |
| **Flooding (Inundación)** | Algoritmo aislado que reenvía paquetes por todos los enlaces disponibles |
| **TTL (Time To Live)** | Campo en el datagrama IP que limita el número de saltos para evitar bucles |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **IP no es fiable** — no confundir con TCP. IP no garantiza entrega, orden ni ausencia de duplicados. Eso lo hace TCP (capa de transporte).
- ❗ En conmutación de circuitos, los recursos están **reservados aunque no se usen** — esto puede ser ineficiente.
- ✅ La conmutación por datagramas es más **resiliente a fallos**: si un nodo cae, los paquetes pueden tomar otra ruta.
- ✅ IPv6 resuelve el agotamiento de direcciones IPv4 con un espacio de 128 bits.
- 🔁 El protocolo TCP (capa de transporte) complementa a IP añadiendo fiabilidad, control de flujo y control de congestión.
- ❗ En **flooding**, sin mecanismo de control (TTL o número de secuencia), los paquetes circulan indefinidamente.

---

## 🔗 Conexiones con otros temas

- **Capa de transporte (TCP/UDP):** TCP añade fiabilidad sobre IP no fiable. UDP también usa IP pero mantiene la no fiabilidad.
- **Capa de enlace:** Define la MTU de cada red física, lo que determina cuándo IP necesita fragmentar.
- **Modelo OSI / TCP-IP:** La capa de red (Nivel 3) se ubica entre la capa de enlace (Nivel 2) y la capa de transporte (Nivel 4).
- **Protocolos de routing (RIP, OSPF, BGP):** Son implementaciones concretas de los algoritmos de encaminamiento distribuido y jerárquico.

---

## 📝 Preguntas de repaso

1. ¿Cuál es la diferencia principal entre conmutación de circuitos y conmutación de paquetes?
2. ¿En qué se diferencia la conmutación por datagramas de la conmutación por circuitos virtuales?
3. ¿Por qué se dice que IP es un protocolo *best effort* y no fiable?
4. ¿Qué diferencia hay entre IPv4 e IPv6 en cuanto a direccionamiento?
5. ¿Para qué sirve la fragmentación en IP y qué papel juega la MTU?
6. ¿Cuál es la diferencia entre un algoritmo de encaminamiento centralizado y uno distribuido?
7. ¿Qué es el algoritmo de inundación (*flooding*) y cuál es su principal desventaja?
8. ¿Por qué se usan algoritmos de encaminamiento jerárquicos en redes grandes?
9. ¿Qué campo del datagrama IPv4 evita que los paquetes circulen indefinidamente?
10. En la técnica de conmutación de circuitos, ¿qué ocurre con los recursos cuando no se están transmitiendo datos?

---

## 📌 Resumen en una página

> La **capa de red** es responsable del encaminamiento de datos entre nodos origen y destino. Existen dos técnicas de conmutación: la de **circuitos** (canal dedicado, misma ruta, recursos reservados) y la de **paquetes** (datos fragmentados que viajan por la red), que a su vez se divide en datagramas (sin conexión previa) y circuitos virtuales (con ruta lógica preestablecida). El **protocolo IP**, en sus versiones IPv4 (32 bits) y IPv6 (128 bits), opera con conmutación por datagramas, es no orientado a conexión, no fiable y *best effort*; delega la fiabilidad a capas superiores. IP contempla la **fragmentación** cuando los datagramas superan la MTU de alguna red intermedia. Para el **encaminamiento**, los algoritmos se clasifican según dónde se toma la decisión: centralizados (un nodo central), aislados (cada nodo con info local, ej. flooding), distribuidos (cada nodo intercambia info con vecinos) y jerárquicos (variante distribuida en regiones para escalar en redes grandes).
>