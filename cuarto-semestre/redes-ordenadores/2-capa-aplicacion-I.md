# La Capa de Aplicación (I)

> **Unidad:** Tema 2
> 
> 
> **Palabras clave:** `capa de aplicación`, `FTP`, `SMTP`, `POP3`, `IMAP`, `DNS`, `socket`, `TCP`, `UDP`, `Cliente-Servidor`, `P2P`
> 

---

## 🎯 Objetivo de la clase

Estudiar la capa de aplicación del modelo OSI/TCP-IP: sus arquitecturas (Cliente-Servidor y P2P), el mecanismo de sockets como API de red, y los protocolos más importantes: FTP (transferencia de archivos), SMTP/POP3/IMAP (correo electrónico) y DNS (resolución de nombres de dominio).

---

## 🧠 Conceptos principales

### 2.2 Modelo y protocolos

### La capa de aplicación

Es la **capa más alta** del modelo OSI. Permite que aplicaciones (procesos) de distintos equipos se comuniquen directamente entre sí. Es la **única capa visible para el desarrollador** de aplicaciones con conectividad de red: las capas inferiores son transparentes para él.

---

### Arquitecturas de aplicaciones de red

Existen dos modelos principales:

```
  CLIENTE-SERVIDOR                        P2P (Peer to Peer)

  ┌────────┐   petición   ┌──────────┐    ┌────┐   ┌────┐
  │Cliente │ ────────────▶│ Servidor │    │Par │◀──│Par │
  └────────┘ ◀──────────  │(siempre  │    └────┘   └────┘
                          │ activo)  │      │  ╲ ╱  │
  ┌────────┐              └──────────┘      │   ╳   │
  │Cliente │ ────────────▶                  │  ╱ ╲  │
  └────────┘                              ┌────┐   ┌────┐
                                          │Par │──▶│Par │
                                          └────┘   └────┘
```

> 📝 **Lectura:** En Cliente-Servidor hay un punto central siempre disponible. En P2P cada nodo puede actuar como cliente y servidor a la vez; no hay un único punto de fallo.
> 

| Característica | Cliente-Servidor | P2P |
| --- | --- | --- |
| **Servidor centralizado** | ✅ Sí, siempre activo | ❌ No es necesario |
| **Escalabilidad** | Limitada (carga en el servidor) | ✅ Alta (cada par contribuye) |
| **Dependencia** | Alta (si el servidor cae, el servicio falla) | Baja (red mallada) |
| **Ejemplos** | Web (HTTP), correo (SMTP), FTP | BitTorrent, Skype (histórico) |

> 💡 **Intuición:** Cliente-Servidor es como un restaurante con un único chef — si el chef no está, no hay servicio. P2P es como una olla comunitaria donde todos traen y comen a la vez.
> 

---

### El socket como API de red

Un **socket** es una construcción del sistema operativo que representa un **extremo lógico de conexión** entre dos máquinas. Actúa como una "embajada" local de la máquina remota.

```
  Máquina A                              Máquina B
  ┌─────────────────────┐      red      ┌─────────────────────┐
  │  Proceso/Aplicación │              │  Proceso/Aplicación │
  │        │            │              │        ▲            │
  │        ▼            │              │        │            │
  │  ┌──────────┐       │◀────────────▶│  ┌──────────┐      │
  │  │  socket  │       │              │  │  socket  │      │
  │  └──────────┘       │              │  └──────────┘      │
  │  [abstracción de    │              │  [abstracción de   │
  │   la comunicación]  │              │   la comunicación] │
  └─────────────────────┘              └─────────────────────┘
```

> 📝 **Lectura:** El proceso simplemente escribe datos en el socket y los recibe de él; toda la complejidad de la red (errores, retransmisiones, control de flujo) queda oculta detrás del socket.
> 

> 💡 **Intuición:** El socket es como un buzón de correo. La aplicación solo mete o saca cartas del buzón; no necesita saber cómo funciona el sistema postal por dentro.
> 

---

### TCP vs UDP como transporte subyacente

Las aplicaciones de la capa de aplicación deben elegir qué protocolo de transporte usar:

| Característica | TCP | UDP |
| --- | --- | --- |
| **Orientado a conexión** | ✅ Sí (negociación previa) | ❌ No |
| **Fiable** | ✅ Sí (no pierde datos) | ❌ No (puede perder datos) |
| **Velocidad de inicio** | Más lento (handshake) | Inmediato |
| **Uso típico** | Correo, web, FTP, DNS* | Streaming, telefonía VoIP, videojuegos |

> ⚠️ *DNS usa UDP por defecto (puerto 53) por su rapidez, aunque puede usar TCP para respuestas grandes.
> 

```
  Aplicaciones sobre TCP:          Aplicaciones sobre UDP:
  ┌──────┬──────┬──────────┐       ┌──────┬──────┐
  │ SMTP │ HTTP │   FTP    │       │ DNS  │ DHCP │
  │ p.25 │ p.80 │ p.20/21  │       │ p.53 │ p.67 │
  └──────┴──────┴──────────┘       └──────┴──────┘
  │         TCP             │       │      UDP     │
  └─────────────────────────┘       └──────────────┘
  │            Internet Protocol (IP)               │
  └─────────────────────────────────────────────────┘
```

> 📝 **Regla general:** Si no importa perder algún dato pero sí importa la fluidez → **UDP**. Si no se puede perder ningún dato → **TCP**.
> 

---

### 2.3 Los servicios FTP y SMTP

### FTP — File Transfer Protocol

Permite el **intercambio de archivos** entre una máquina local y una remota, en cualquier dirección, con autenticación de usuario opcional.

**Característica clave:** FTP usa **dos conexiones TCP paralelas** durante la transferencia:

```
  Cliente FTP                            Servidor FTP
  ┌────────────────────────┐             ┌───────────────────────┐
  │ Intérprete de protocolo│◀──────────▶│ Intérprete protocolo  │
  │        (usuario)       │  Control    │       (servidor)      │
  │                        │  TCP p.21   │                       │
  ├────────────────────────┤             ├───────────────────────┤
  │  Proceso transferencia │◀──────────▶│  Proceso transferencia│
  │      de datos DTP      │  Datos      │      de datos DTP     │
  │                        │  TCP p.20   │                       │
  └──────────┬─────────────┘             └───────────┬───────────┘
             │                                       │
        Sistema de                             Sistema de
         ficheros                               ficheros
```

> 📝 **Lectura:** La conexión de control (puerto 21) se mantiene abierta toda la sesión. Por cada archivo, se abre y cierra una conexión de datos independiente (puerto 20).
> 

| Conexión | Puerto | Función | ¿Persistente? |
| --- | --- | --- | --- |
| **Control** | TCP 21 | Comandos: usuario, contraseña, PUT, GET | ✅ Sí (toda la sesión) |
| **Datos** | TCP 20 | Transferencia real del archivo | ❌ No (una por archivo) |

**Comandos FTP principales:**

```
USER 'nombre'    → Identificar al usuario
PASS 'contraseña' → Autenticar
RETR 'archivo'   → Descargar (retrieve)
STOR 'archivo'   → Subir (store)
LIST             → Listar archivos
```

> 💡 **Ejemplo:** Si transferís 3 archivos en una sesión FTP, se abren **1 conexión de control + 3 conexiones de datos** (una por archivo, que se cierran en cuanto termina cada transferencia).
> 

---

### SMTP — Simple Mail Transfer Protocol

Protocolo principal para el **envío** de correo electrónico en Internet. Usa **TCP** (orientado a conexión y fiable).

**Flujo completo del correo electrónico:**

```
  Remitente                                         Destinatario
  ┌──────────┐  SMTP   ┌──────────────┐  SMTP  ┌──────────────┐
  │  Cliente │────────▶│ Servidor     │───────▶│ Servidor     │
  │  de      │         │ correo       │         │ correo       │
  │  correo  │         │ remitente    │         │ destinatario │
  │ (MUA)    │         │ (MTA)        │         │ (MTA)        │
  └──────────┘         └──────────────┘         └──────┬───────┘
                                                        │
                                                 POP3 / IMAP
                                                        │
                                                 ┌──────▼───────┐
                                                 │  Cliente de  │
                                                 │  correo del  │
                                                 │ destinatario │
                                                 └──────────────┘
```

> 📝 **Lectura:** SMTP solo sirve para **enviar** correo entre servidores. Para que el destinatario **descargue** su correo del servidor, se usan POP3 o IMAP.
> 

---

### POP3 vs IMAP — Protocolos de recepción de correo

| Característica | POP3 | IMAP |
| --- | --- | --- |
| **Complejidad** | Sencillo | Sofisticado |
| **Organización en carpetas** | ❌ No | ✅ Sí (en el servidor) |
| **Acceso desde múltiples dispositivos** | ❌ Descarga y borra del servidor | ✅ Vista sincronizada en todos |
| **Estado entre sesiones** | ❌ No conserva estado | ✅ Sí conserva estado |
| **Descarga parcial (solo adjuntos seleccionados)** | ❌ No | ✅ Sí |

> 💡 **Intuición:** POP3 es como recoger el correo de la oficina de correos y llevártelo a casa (ya no está en el buzón). IMAP es como acceder a una bandeja de entrada en la nube — lo que organizas en un dispositivo se refleja en todos.
> 

**Respuestas POP3:**

- `+OK` → Operación correcta
- `ERR` → Error

**Fase de autorización POP3:**

```
user 'nombreusuario'
pass 'contraseña'
```

---

### 2.4 El servicio DNS

### ¿Por qué existe DNS?

Los equipos en Internet se identifican por **direcciones IP** (ej. `187.45.85.96`), que son difíciles de memorizar. Los humanos preferimos usar **nombres de dominio** (ej. `contabilidad.miempresa.es`). El **DNS** hace la traducción.

> 💡 **Intuición:** DNS es como la agenda de contactos del teléfono: tú buscas "papá" y el teléfono sabe que tiene que marcar `+52 55 1234 5678`. Sin la agenda, habría que recordar todos los números.
> 

---

### ¿Qué es el sistema DNS?

DNS es **dos cosas a la vez**:

```
  DNS
  ├── Base de datos distribuida mundialmente
  │    └── Jerarquía de servidores con correspondencias
  │         nombre de dominio ↔ dirección IP
  │
  └── Protocolo de capa de aplicación
       └── Permite a las máquinas consultar esa base de datos
           (usa UDP puerto 53 por defecto)
```

**Jerarquía DNS:**

```
              . (raíz)
              │
     ┌────────┼────────┐
    .com     .org      .es      ← TLD (Top Level Domains)
     │                 │
  .google           .unir
     │
   www               ← Subdominio (nombre de host)
```

> 📝 **Lectura:** Cuando escribes `www.google.com`, la consulta DNS sube por la jerarquía: primero pregunta a un servidor raíz, luego al TLD `.com`, luego al servidor autoritativo de `google.com`, que devuelve la IP de `www`.
> 

---

### Servicios adicionales del DNS

Además de traducir nombres a IPs, DNS ofrece:

| Servicio | Descripción | Ejemplo |
| --- | --- | --- |
| **Alias de host** | Una máquina puede tener varios nombres | `servidor1.empresa.es` = `web.empresa.es` |
| **Alias de servidor de correo** | Simplifica el nombre de servidores SMTP | `mail.empresa.es` apunta al servidor real |
| **Distribución de carga** | Varios IPs asociadas a un nombre; se rotan entre consultas | `google.com` → varias IPs de distintos servidores |

---

### Herramienta nslookup

`nslookup` es un comando que permite **enviar consultas directamente a un servidor DNS** para obtener información de resolución de nombres.

```bash
nslookup www.google.com
# Devuelve la dirección IP asociada al nombre
```

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **Capa de aplicación** | Capa más alta del modelo OSI; gestiona la comunicación entre procesos de distintas máquinas |
| **Arquitectura Cliente-Servidor** | Un servidor centralizado y siempre activo atiende las solicitudes de múltiples clientes |
| **Arquitectura P2P** | Comunicación entre pares sin servidores centralizados fijos; cada nodo puede ser cliente y servidor |
| **Socket** | Extremo lógico de conexión; API del SO que abstrae la comunicación de red para las aplicaciones |
| **FTP** | *File Transfer Protocol*; protocolo para transferencia de archivos usando dos conexiones TCP |
| **SMTP** | *Simple Mail Transfer Protocol*; protocolo de **envío** de correo electrónico; usa TCP |
| **POP3** | *Post Office Protocol v3*; protocolo sencillo de **recepción** de correo; descarga y opcionalmente borra del servidor |
| **IMAP** | *Internet Message Access Protocol*; protocolo avanzado de **recepción** de correo; sincroniza carpetas en servidor |
| **DNS** | *Domain Name System*; base de datos distribuida y protocolo que traduce nombres de dominio a IPs |
| **TLD** | *Top Level Domain*; dominio de primer nivel (`.com`, `.es`, `.org`, etc.) |
| **Aplicación elástica** | Aplicación que se adapta a la tasa de transferencia disponible en todo momento |
| **nslookup** | Herramienta de línea de comandos para consultar servidores DNS directamente |
| **Pharming** | Ataque que redirige el tráfico de un nombre DNS legítimo hacia un servidor falso |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **SMTP ≠ POP3/IMAP**: SMTP es para **enviar** correo; POP3 e IMAP son para **recibirlo/acceder** a él desde el servidor.
- ❗ **FTP usa DOS conexiones TCP**, no una: control (persistente) y datos (una por archivo, no persistente).
- ✅ **UDP no siempre es "malo"**: para streaming de vídeo/audio es preferible porque la fluidez importa más que la pérdida de algún frame.
- ✅ **IMAP es superior a POP3** para usuarios multidispositivo: el estado y las carpetas se mantienen en el servidor y se sincronizan.
- ❗ **DNS no es solo traducción de nombres**: también ofrece alias de host, alias de servidor de correo y distribución de carga.
- 🔁 El **socket** conecta la capa de aplicación con la capa de transporte (TCP o UDP); es la frontera entre lo que controla el programador y lo que controla el SO.
- ❗ En P2P, los **peers** son dispositivos de los propios usuarios, **no** del proveedor del servicio.

---

## 🔗 Conexiones con otros temas

- **Tema 1 (Modelo OSI/TCP-IP):** La capa de aplicación es la capa 7 del OSI o la capa 4 (aplicación) del modelo TCP-IP.
- **Tema 5 (Capa de red - IP):** DNS usa IP para enviar sus consultas. FTP y SMTP también dependen de IP para el encaminamiento de sus paquetes.
- **TCP/UDP:** Los protocolos de este tema dependen directamente del transporte elegido: FTP, SMTP, HTTP → TCP; DNS (por defecto), DHCP, streaming → UDP.

---

## 📝 Preguntas de repaso

1. ¿Cuáles son las dos grandes arquitecturas de aplicaciones de red y en qué se diferencian?
2. ¿Qué es un socket y qué ventaja ofrece al programador de aplicaciones?
3. ¿Cuándo conviene usar TCP y cuándo UDP como transporte subyacente? Da ejemplos.
4. ¿Cuántas conexiones TCP usa FTP y para qué sirve cada una? ¿Son persistentes?
5. ¿Qué protocolo se usa para enviar correo electrónico? ¿Y para recibirlo?
6. ¿Cuál es la principal diferencia entre POP3 e IMAP? ¿Cuándo usarías cada uno?
7. ¿Qué es el sistema DNS y qué dos cosas es simultáneamente?
8. Además de traducir nombres a IPs, ¿qué otros tres servicios ofrece DNS?
9. ¿Por qué DNS usa UDP en lugar de TCP por defecto?
10. Si en una sesión FTP transfieres 5 archivos, ¿cuántas conexiones TCP se habrán abierto en total?

---

## 📌 Resumen en una página

> La **capa de aplicación** es la más alta del modelo OSI y la única de la que debe preocuparse el desarrollador. Las aplicaciones de red pueden seguir dos arquitecturas: **Cliente-Servidor** (un servidor centralizado siempre activo atiende a los clientes) o **P2P** (los propios dispositivos de los usuarios actúan como clientes y servidores simultáneamente). La interfaz que usa una aplicación para acceder a la red es el **socket**, que abstrae toda la complejidad del transporte. Las aplicaciones eligen entre **TCP** (fiable, orientado a conexión: correo, web, FTP) y **UDP** (no fiable, inmediato: streaming, VoIP). El protocolo **FTP** transfiere archivos mediante dos conexiones TCP paralelas: una de control (persistente, puerto 21) y una de datos por cada archivo (no persistente, puerto 20). El **correo electrónico** usa **SMTP** para el envío y **POP3** (sencillo, descarga local) o **IMAP** (sofisticado, sincronización en servidor) para la recepción. El sistema **DNS** es una base de datos distribuida jerárquicamente que traduce nombres de dominio legibles a direcciones IP, y además proporciona alias de host, alias de servidor de correo y distribución de carga entre servidores replicados.
>