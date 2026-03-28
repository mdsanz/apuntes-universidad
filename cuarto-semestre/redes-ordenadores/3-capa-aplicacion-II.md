# La Capa de Aplicación (II): Web y HTTP

> **Unidad:** Tema 3
> 
> 
> **Palabras clave:** `HTTP`, `GET`, `POST`, `HEAD`, `PUT`, `DELETE`, `URI`, `cookies`, `código de estado`, `transacción HTTP`, `sin estado`, `TCP`
> 

---

## 🎯 Objetivo de la clase

Estudiar en profundidad el protocolo HTTP: su modelo de operación cliente-servidor, el formato exacto de los mensajes de solicitud y respuesta, los métodos de interacción disponibles (GET, POST, HEAD, PUT, DELETE) y los códigos de estado que devuelve el servidor.

---

## 🧠 Conceptos principales

### 3.2 Modelo de operación de HTTP

### ¿Qué es HTTP?

El **protocolo HTTP** (*HyperText Transfer Protocol*) define cómo se solicitan y transportan páginas web (documentos HTML, imágenes, vídeos, scripts, etc.) en Internet. Es un protocolo de **capa de aplicación** que usa **TCP** como transporte subyacente.

> 💡 **Intuición:** HTTP es como un sistema de pedidos en un restaurante: el cliente (navegador) hace un pedido (solicitud), el camarero (red TCP/IP) lo lleva a cocina (servidor), y regresa con el plato (respuesta con el documento HTML).
> 

---

### La transacción HTTP

El proceso completo de pedir y recibir una página se llama **transacción HTTP**:

```
  Usuario                Cliente web                   Servidor web
     │                       │                               │
     │  1. Escribe la URL     │                               │
     │──────────────────────▶│                               │
     │                       │  2. Establece conexión TCP    │
     │                       │──────────────────────────────▶│
     │                       │  3. Envía solicitud HTTP      │
     │                       │──────────────────────────────▶│
     │                       │  4. Responde con el objeto    │
     │                       │◀──────────────────────────────│
     │                       │  5. Cierra la conexión        │
     │  Renderiza la página   │                               │
     │◀──────────────────────│                               │
```

> 📝 **Lectura:** Cada objeto (HTML, imagen, CSS, script) implica su propia conexión independiente en HTTP/1.0. En HTTP/1.1 se pueden reutilizar conexiones (persistentes).
> 

**Pasos de una transacción HTTP:**

1. El usuario especifica la URL en el navegador.
2. El cliente establece una **conexión TCP** con el servidor (puerto 80 por defecto).
3. El cliente **solicita** la página u objeto.
4. El servidor **responde** con el objeto (o un código de error si no existe).
5. El cliente interpreta el HTML y **renderiza** visualmente la página.
6. Se **cierra** la conexión.

---

### HTTP es sin estado (*stateless*)

> ⚠️ HTTP **no conserva información de conexiones anteriores**. Cada solicitud es independiente; el servidor no recuerda si ese cliente ya accedió antes.
> 

```
  Solicitud 1: GET /index.html   → Servidor responde y olvida
  Solicitud 2: GET /imagen.jpg   → Servidor no sabe quién eres
  Solicitud 3: GET /estilo.css   → Servidor no sabe quién eres
```

**Consecuencia importante:** Una página con 1 HTML + 2 imágenes genera **3 conexiones independientes**.

> 💡 Para "recordar" al usuario entre sesiones se usan las **cookies**, que son datos que el servidor guarda en el cliente y el cliente reenvía en cada solicitud.
> 

---

### Cookies

Las **cookies** son la solución al problema del protocolo sin estado:

| Elemento | Descripción |
| --- | --- |
| **Quién las crea** | El servidor HTTP, en el mensaje de respuesta |
| **Cabecera de respuesta** | `Set-Cookie: valor` |
| **Cabecera de solicitud** | `Cookie: valor` (el cliente las reenvía automáticamente) |
| **Para qué sirven** | Seguimiento de sesión, preferencias, carritos de compra, login persistente |

> 💡 **Intuición:** La cookie es como una ficha de vestuario. El restaurante (servidor) te da una ficha numerada al llegar (primera visita). La próxima vez, presentas la ficha y el restaurante sabe quién eres y qué mesa prefieres.
> 

---

### GET condicional

El método `GET` puede incluir la cabecera `If-Modified-Since` para implementar **caché en el cliente**:

```
GET /pagina.html HTTP/1.1
Host: www.ejemplo.com
If-Modified-Since: Wed, 08 Nov 2014 23:11:55 GMT
```

- Si el objeto **no ha cambiado** desde esa fecha → servidor responde `304 Not Modified` (sin cuerpo, ahorra ancho de banda).
- Si **ha cambiado** → servidor responde `200 OK` con el nuevo contenido.

---

### 3.3 Formatos de mensajes HTTP

Todos los mensajes HTTP son **texto plano**, perfectamente legibles por humanos.

---

### Mensaje de solicitud (HTTP Request)

```
┌─────────────────────────────────────────────────────────┐
│  Línea de solicitud: MÉTODO  URI  VERSIÓN_HTTP  CRLF    │
├─────────────────────────────────────────────────────────┤
│  Cabeceras (opcionales):                                │
│    Cabecera-General: valor  CRLF                        │
│    Cabecera-Solicitud: valor  CRLF                      │
│    Cabecera-Entidad: valor  CRLF                        │
├─────────────────────────────────────────────────────────┤
│  CRLF  (línea en blanco obligatoria)                    │
├─────────────────────────────────────────────────────────┤
│  [Cuerpo de la entidad]  (opcional, raro en solicitudes)│
└─────────────────────────────────────────────────────────┘
```

**Ejemplo real de solicitud GET:**

```
GET /index.html HTTP/1.1
Host: www.unir.net
Accept: text/html
```

**Los tres elementos de la línea de solicitud:**

| # | Elemento | Descripción | Ejemplo |
| --- | --- | --- | --- |
| 1 | **Método** | Acción que se quiere realizar | `GET`, `POST`, `HEAD` |
| 2 | **URI** | Identificador del recurso solicitado | `/index.html` |
| 3 | **Versión HTTP** | Versión del protocolo | `HTTP/1.1` |

---

### Métodos HTTP

| Método | Función | ¿Envía cuerpo? | Notas clave |
| --- | --- | --- | --- |
| **GET** | Solicitar un recurso (HTML, imagen, vídeo, CSS…) | ❌ No | Los datos van en la URL; cantidad limitada |
| **POST** | Enviar datos de formulario al servidor | ✅ Sí | Sin límite de datos; más seguro que GET para datos sensibles |
| **HEAD** | Obtener solo las cabeceras, sin el cuerpo | ❌ No | Útil para depuración y verificar si un recurso existe |
| **PUT** | Actualizar/subir contenido al servidor | ✅ Sí | Reemplaza el recurso en la URI indicada |
| **DELETE** | Eliminar un recurso del servidor | ❌ No | Borra el recurso en la URI indicada |

> 💡 **Truco para el examen:** `HEAD` = `GET` sin el cuerpo de respuesta. Se usa para depuración o para implementar cachés que verifican si un documento cambió.
> 

> ⚠️ **GET en formularios:** Los datos enviados aparecen visibles en la URL (`?usuario=juan&pass=1234`). Para datos sensibles siempre se debe usar **POST**.
> 

---

### Mensaje de respuesta (HTTP Response)

```
┌─────────────────────────────────────────────────────────┐
│  Línea de estado: VERSIÓN_HTTP  CÓDIGO  RAZÓN  CRLF     │
├─────────────────────────────────────────────────────────┤
│  Cabeceras:                                             │
│    Date: ...                                            │
│    Server: ...                                          │
│    Content-Type: text/html                              │
│    Content-Length: 131                                  │
├─────────────────────────────────────────────────────────┤
│  CRLF  (línea en blanco obligatoria)                    │
├─────────────────────────────────────────────────────────┤
│  Cuerpo de la entidad  (el documento HTML solicitado)   │
└─────────────────────────────────────────────────────────┘
```

**Ejemplo real de respuesta:**

```
HTTP/1.1 200 OK
Date: Mon, 24 Nov 2014 22:38:34 GMT
Server: Apache/1.3.3.7 (Unix)
Last-Modified: Wed, 08 Nov 2014 23:11:55 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 131
Connection: close

<html>
  <head><title>Ejemplo HTTP</title></head>
  <body>Esto es una página de pruebas.</body>
</html>
```

**Los tres elementos de la línea de estado:**

| # | Elemento | Descripción | Ejemplo |
| --- | --- | --- | --- |
| 1 | **Versión HTTP** | Versión disponible en el servidor | `HTTP/1.1` |
| 2 | **Código de estado** | Resultado numérico de la solicitud | `200` |
| 3 | **Razón** | Descripción textual del código | `OK` |

---

### Códigos de estado HTTP

| Rango | Categoría | Ejemplos importantes |
| --- | --- | --- |
| **1xx** | Informativos | `100 Continue` |
| **2xx** | Éxito | `200 OK` — solicitud resuelta correctamente |
| **3xx** | Redirección | `301 Moved Permanently`, `304 Not Modified` (caché válida) |
| **4xx** | Error del cliente | `400 Bad Request`, **`404 Not Found`** (recurso no existe), `403 Forbidden` |
| **5xx** | Error del servidor | `500 Internal Server Error`, `503 Service Unavailable` |

> 💡 **Regla fácil:** 2xx = todo bien · 3xx = busca en otro lado · 4xx = error tuyo (cliente) · 5xx = error del servidor.
> 

---

### Flujo completo: solicitud y respuesta

```
  CLIENTE                                       SERVIDOR
     │                                               │
     │  GET /index.html HTTP/1.1                     │
     │  Host: www.unir.net              ────────────▶│
     │                                               │
     │           HTTP/1.1 200 OK                     │
     │           Content-Type: text/html             │
     │           Content-Length: 131                 │
     │           [línea en blanco]      ◀────────────│
     │           <html>...</html>                    │
     │                                               │
     │  (si hay 2 imágenes en el HTML)               │
     │  GET /imagen1.jpg HTTP/1.1       ────────────▶│
     │           HTTP/1.1 200 OK        ◀────────────│
     │                                               │
     │  GET /imagen2.jpg HTTP/1.1       ────────────▶│
     │           HTTP/1.1 200 OK        ◀────────────│
```

> 📝 **Lectura:** Cada objeto requiere su propia solicitud/respuesta. Una página con 1 HTML + 2 imágenes = **3 transacciones HTTP**.
> 

---

## 📐 Definiciones formales

| Término | Definición |
| --- | --- |
| **HTTP** | *HyperText Transfer Protocol*; protocolo de capa de aplicación para la web; usa TCP |
| **Transacción HTTP** | Ciclo completo solicitud-respuesta: desde que el usuario pide una URL hasta que el navegador renderiza el contenido |
| **Sin estado (*stateless*)** | HTTP no recuerda conexiones anteriores; cada solicitud es independiente |
| **URI** | *Universal Resource Identifier*; identificador único del recurso dentro del servidor |
| **CRLF** | *Carriage Return + Line Feed*; secuencia que finaliza cada línea de un mensaje HTTP |
| **Línea de solicitud** | Primera línea del mensaje de solicitud: `MÉTODO URI VERSIÓN` |
| **Línea de estado** | Primera línea del mensaje de respuesta: `VERSIÓN CÓDIGO RAZÓN` |
| **Código de estado** | Número que indica el resultado de la solicitud (200 = éxito, 404 = no encontrado…) |
| **Cookie** | Pequeño fragmento de datos que el servidor almacena en el cliente para mantener estado |
| **GET condicional** | GET con cabecera `If-Modified-Since` para implementar caché; evita descargar contenido no modificado |
| **HTTPS** | HTTP con seguridad SSL/TLS; cifra el tráfico entre cliente y servidor |
| **AJAX** | Técnica que permite realizar solicitudes HTTP asíncronas desde JavaScript sin recargar la página |

---

## ⚠️ Puntos importantes / Errores comunes

- ❗ **HTTP usa TCP**, no UDP. El transporte fiable es imprescindible para la integridad de los documentos web.
- ❗ **HTTP es sin estado** — el servidor no recuerda al cliente entre solicitudes. Las cookies son la solución a esto, no parte del protocolo base.
- ✅ **Una página con N objetos = N+1 conexiones en HTTP/1.0** (1 para el HTML + 1 por cada objeto embebido). En HTTP/1.1 con conexiones persistentes se reutiliza la misma conexión.
- ❗ **GET expone datos en la URL** — nunca usar para contraseñas o datos sensibles. POST los envía en el cuerpo del mensaje.
- ✅ **HEAD** es igual que GET pero el servidor NO devuelve el cuerpo. Se usa para depuración y para verificar si un recurso existe sin descargarlo.
- ❗ **404 es error del cliente** (pidió algo que no existe), no del servidor. Los errores del servidor son 5xx.
- 🔁 El puerto por defecto de HTTP es el **80**; el de HTTPS es el **443**. Aunque un servidor puede escuchar en cualquier puerto, estos son los estándar.
- ✅ Los mensajes HTTP son **texto plano legible** — se pueden leer directamente con herramientas como `telnet`, `curl` o las DevTools del navegador.

---

## 🔗 Conexiones con otros temas

- **Tema 2 (Capa de aplicación I):** HTTP es otro protocolo de la capa de aplicación, como FTP o SMTP. Todos usan TCP como transporte.
- **Tema 1 (Modelo OSI/TCP-IP):** HTTP opera en la capa de aplicación (capa 7 OSI / capa 4 TCP-IP) y delega el transporte en TCP (capa 4 OSI).
- **DNS (Tema 2):** Antes de establecer la conexión TCP con el servidor web, el navegador hace una consulta DNS para traducir el nombre de dominio (ej. `www.unir.net`) a una dirección IP.

---

## 📝 Preguntas de repaso

1. ¿Qué protocolo de transporte usa HTTP y en qué puerto escucha por defecto?
2. Describe los 5 pasos de una transacción HTTP.
3. ¿Qué significa que HTTP es un protocolo "sin estado"? ¿Qué mecanismo se usa para paliar esta limitación?
4. Si una página web contiene 1 HTML + 3 imágenes + 1 hoja CSS, ¿cuántas conexiones HTTP se abren en HTTP/1.0?
5. ¿Cuáles son los tres elementos de la línea de solicitud de un mensaje HTTP?
6. ¿Cuál es la diferencia entre los métodos GET y POST? ¿Cuándo usarías cada uno?
7. ¿Para qué sirve el método HEAD? ¿En qué se diferencia de GET?
8. ¿Cuáles son los tres elementos de la línea de estado de una respuesta HTTP?
9. ¿Qué significan los rangos de códigos de estado 2xx, 3xx, 4xx y 5xx?
10. ¿Qué es el GET condicional y qué cabecera usa? ¿Para qué sirve?
11. ¿Cómo funcionan las cookies y qué cabecera HTTP usa el servidor para crearlas?
12. ¿Qué diferencia hay entre HTTP y HTTPS?

---

## 📌 Resumen en una página

> El protocolo **HTTP** define cómo se solicitan y entregan recursos web (HTML, imágenes, CSS, etc.) en Internet. Opera en la **capa de aplicación** usando **TCP** como transporte (puerto 80). Su funcionamiento se basa en un ciclo de **solicitud-respuesta** llamado transacción HTTP: el cliente envía un mensaje de solicitud con una **línea de solicitud** (método + URI + versión), cabeceras opcionales y, a veces, un cuerpo; el servidor responde con una **línea de estado** (versión + código + razón), cabeceras y el cuerpo del documento. Los métodos principales son **GET** (pedir recurso, datos en URL), **POST** (enviar datos en el cuerpo, sin límite), **HEAD** (solo cabeceras, útil para depuración), **PUT** (actualizar) y **DELETE** (eliminar). Los **códigos de estado** indican el resultado: 200 (OK), 301 (redirección), 304 (no modificado / caché válida), 404 (no encontrado) y 500 (error del servidor). HTTP es un protocolo **sin estado**: no recuerda conexiones previas. Las **cookies** compensan esto permitiendo al servidor guardar información en el cliente. El **GET condicional** (`If-Modified-Since`) implementa caché en el cliente evitando descargar contenido que no ha cambiado.
>