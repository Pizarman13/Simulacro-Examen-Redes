# Simulacro-Examen-Redes
Diego Pizarro Garrido
https://github.com/Pizarman13/Simulacro-Examen-Redes.git

## Parte I: Capa de Red

### Pregunta 1: Cálculo de Ruta Más Corta

#### a) Algoritmo de Dijkstra  
**Descripción general**  
Dijkstra es un método determinista para hallar la ruta de coste mínimo desde un nodo origen a todos los demás nodos de un grafo dirigido o no dirigido con pesos no negativos.

**Pasos básicos**  
1. **Inicialización**  
   - A cada nodo se le asigna una distancia inicial ∞, excepto el origen que vale 0.  
   - Se mantiene un conjunto **S** de nodos “resueltos” (distancia definitiva) inicialmente vacío.

2. **Iteración**  
   - Mientras haya nodos fuera de **S**:  
     1. Seleccionar el nodo *u* fuera de **S** con la menor distancia provisional.  
     2. Agregar *u* a **S**.  
     3. Para cada vecino *v* de *u*, actualizar:  
        ```
        dist[v] = min(dist[v], dist[u] + w(u,v))
        ```
        donde *w(u,v)* es el peso de la arista *(u,v)*.

3. **Resultado**  
   - Al finalizar, **dist** contiene la distancia mínima desde el origen a cada nodo.  
   - Para reconstruir caminos, se almacena un array **prev** que indica el predecesor en la ruta óptima.

#### b) Enrutamiento por Inundación (Flooding)  
**Descripción**  
- Cada nodo que recibe un paquete lo reenvía a **todos** sus vecinos (excepto por donde llegó).  
- El paquete lleva un identificador único para evitar loops: si un nodo ve el mismo ID dos veces, descarta réplicas.

**Ventajas**  
- **Simplicidad**: no requiere conocer la topología completa ni calcular rutas.  
- **Robustez**: siempre encuentra un camino si existe, aun con fallos parciales.

**Desventajas**  
- **Explosión de tráfico**: genera muchas réplicas innecesarias → congestión.  
- **Escalabilidad pobre**: en redes grandes, el número de mensajes crece exponencialmente.  
- **Sin garantía de óptimo**: no minimiza coste ni distancia.

| Característica        | Dijkstra                | Flooding                  |
|-----------------------|-------------------------|---------------------------|
| Complejidad comput.   | O(V²) o O(E log V)      | Muy baja por nodo         |
| Mensajes totales      | Proporcional a rutas    | Exponencial en nodos      |
| Conocimiento topología| Necesario               | No necesario              |
| Óptimo                | Sí                      | No                        |

---

### Pregunta 2: Cálculo de Direcciones de Broadcast y Subredes

#### a) Subred 172.29.152.0 /21 (máscara 255.255.248.0)  
1. **Máscara en binario**  
255.255.248.0 → 11111111.11111111.11111000.00000000
21 bits de red, 11 bits de host
2. **Host bits**  
- Hay 11 bits de host: 32 − 21 = 11.
3. **Broadcast**  
- Invertir la máscara en la parte host → 0.0.7.255  
- OR con la dirección de red:
  ```
  172.29.152.0
  OR 0.0.7.255
  = 172.29.159.255
  ```
**Broadcast:** 172.29.159.255

#### b) Bloque 172.18.26.0 /23 (máscara 255.255.254.0)  
1. **Máscara en binario**  
255.255.254.0 → 11111111.11111111.11111110.00000000
23 bits de red, 9 bits de host
2. **Tamaño del bloque**  
- Incremento en tercer octeto: 256 − 254 = 2  
- Subredes empiezan en múltiplos de 2: …, 24.0; 26.0; 28.0; …
3. **Broadcast**  
- Tercero = 26 + (2 − 1) = 27  
- Cuarto = 255  
**Broadcast:** 172.18.27.255

---

### Pregunta 3: Última Dirección Válida y Rango de Hosts

#### a) Subred 172.30.67.192 /26 (máscara 255.255.255.192)  
1. **Bits de host:** 32 − 26 = 6 → 2⁶ − 2 = 62 hosts.  
2. **Broadcast:**  
- Invertir 192 → 00111111 (63)  
- 192 OR 63 = 255 → 172.30.67.255  
3. **Última dirección válida:** 172.30.67.254

#### b) Host 172.22.53.199 /22 (máscara 255.255.252.0)  
1. **Bits de host:** 32 − 22 = 10.  
2. **Tamaño bloque tercer octeto:** 256 − 252 = 4.  
3. **Inicio de subred:** ⌊53/4⌋ × 4 = 13 × 4 = 52 → 172.22.52.0  
4. **Broadcast:**  
- Tercero = 52 + 3 = 55  
- Cuarto = 255 → 172.22.55.255  
5. **Rango de hosts válidos:**  
- Primera: 172.22.52.1  
- Última: 172.22.55.254

---

### Pregunta 4: Capacidad y Segmentación de Subredes

#### a) Nº de hosts en 172.26.0.0 /26 (255.255.255.192)  
- Bits de host = 32 − 26 = 6  
- Hosts útiles = 2⁶ − 2 = **62**

#### b) Subred de 172.18.171.190 /23  
1. /23 → bloques de 2 en tercer octeto.  
2. Inicio = ⌊171/2⌋ × 2 = 85 × 2 = 170  
3. Subred = **172.18.170.0 /23**  
- Rango: 172.18.170.1 – 172.18.171.254  
- Broadcast: 172.18.171.255

---

### Pregunta 5: Nº de Subredes Necesarias

La fórmula general es: Nº de subredes = 2^s
- **s** = bits prestados a host para subred.

Para **al menos 4 subredes**:
2^s ≥ 4 ⇒ s = 2
Por ejemplo, partiendo de /24 y tomando 2 bits → máscara /26:
- 2² = 4 subredes
- Cada una con 2^(32−26) − 2 = 62 hosts.
  
---

## Parte II: Capa de Transporte

### Pregunta 6: Comparación entre TCP y UDP

#### a) Comparativa TCP vs. UDP

| Característica                          | TCP                                             | UDP                                         |
|-----------------------------------------|-------------------------------------------------|---------------------------------------------|
| **Necesidad de conexión**               | Orientado a conexión (handshake inicial).       | Sin conexión (“connectionless”).            |
| **Fiabilidad y control de errores**     | Sí: ACKs, retransmisiones, checksum, secuencias.| Mínimo: checksum opcional, sin ACK ni retrans. |
| **Control de flujo y congestión**       | Sí: ventanas deslizantes, control de congestión.| No: el emisor no regula velocidad.          |
| **Velocidad de transmisión**            | Menor (overhead de control).                    | Mayor (overhead mínimo).                    |

#### b) Ejemplos de aplicaciones con UDP

1. **Streaming de vídeo en directo (VoD/Live Streaming)**  
   - Justificación: Prioriza baja latencia y tolera cierta pérdida de paquetes antes que retrasos por retransmisiones.

2. **DNS (Domain Name System)**  
   - Justificación: Consultas y respuestas breves; retransmisiones rápidas en caso de pérdida y bajo coste de control.

---

### Pregunta 7: Establecimiento y Terminación de Conexión en TCP

#### Establecimiento de conexión (Three-Way Handshake)

1. **SYN**  
   - El cliente envía un segmento con el flag SYN=1 y un número de secuencia inicial (ISN\_c).  
2. **SYN-ACK**  
   - El servidor responde con SYN=1, ACK=1, su propio ISN\_s y ACK = ISN\_c + 1.  
3. **ACK**  
   - El cliente envía ACK=1 con ACK = ISN\_s + 1.  
4. **Estado**  
   - Ambos pasan a estado “ESTABLISHED” y pueden intercambiar datos.

> **Importancia**:  
> - Acordar números de secuencia para ordenar bytes.  
> - Evitar segmentos duplicados o replay.

#### Terminación de conexión (Four-Way Handshake)

1. **FIN (cliente→servidor)**  
   - Cliente envía FIN=1, solicita cerrar su lado de la conexión.  
2. **ACK (servidor→cliente)**  
   - Servidor confirma FIN, ACK = Seq_cliente + 1.  
3. **FIN (servidor→cliente)**  
   - Cuando el servidor quiere cerrar, envía FIN=1, Seq_servidor.  
4. **ACK (cliente→servidor)**  
   - Cliente confirma FIN del servidor, ACK = Seq_servidor + 1.  
5. **Cierre completo**  
   - Cada lado libera recursos tras el ACK final.

> **Importancia**:  
> - Cierre ordenado y bilateral, asegura entrega de datos pendientes antes de liberar.

---

### Pregunta 8: Multiplexación y Demultiplexación

- **Multiplexación descendente** (sender-side)  
  - La capa de transporte **combina** datos de varias aplicaciones (diferentes sockets/puertos) en un flujo de segmentos TCP/UDP para enviar al siguiente nivel (IP).  
  - **Ejemplo**: un servidor web (puerto 80) y un servidor SSH (puerto 22) en la misma máquina envían datos simultáneamente; TCP incorpora cada aplicación en su propio segmento, pero ambos flujos salen por la misma interfaz de red.

- **Demultiplexación ascendente** (receiver-side)  
  - La capa de transporte **separa** los segmentos recibidos según el número de puerto destino y los entrega al socket/aplicación correspondiente.  
  - **Ejemplo**: llegan dos segmentos, uno con destino puerto 80 y otro a puerto 22; la capa IP pasa ambos a TCP, que los entrega a la aplicación HTTP y a la aplicación SSH respectivamente.

---

### Pregunta 9: Cálculo del Tamaño de Ventana en TCP

**Datos**  
- RTT = 50 ms = 0,05 s  
- Ancho de banda = 100 Mbps = 100 × 10^6 bps  
- MSS = 1 500 bytes

1. **Ventana óptima (bits)**  
   W = BW × RTT
   = 100 × 10^6 bps × 0,05 s
   = 5,0 × 10^6 bits

2. **Ventana óptima (bytes)**  
    W_bytes = W_bits / 8
   = 5,0 × 10^6 bits / 8
   = 625 000 bytes

3. **Número de segmentos MSS**  
   N = W_bytes / MSS
   = 625 000 bytes / 1 500 bytes/segmento
   ≈ 416,67
   ≈ 417 segmentos

> **Resultado**:  
> - Ventana ≈ 5 000 000 bits (≈ 625 000 bytes)  
> - ∼ 417 segmentos MSS en vuelo

---

### Pregunta 10: Control de Congestión en TCP

1. **Algoritmo de Arranque Lento (Slow Start)**  
   - **Objetivo**: descubrir la capacidad de la red sin causar congestión.  
   - **Mecanismo**: cwnd comienza en 1 MSS y **se duplica** cada RTT (cwnd += cwnd) hasta alcanzar el umbral ssthresh.

2. **Algoritmo de Nagle**  
   - **Objetivo**: reducir el número de pequeños paquetes (“tinygrams”).  
   - **Mecanismo**: si hay datos sin ACK pendientes, retrasa el envío de segmentos de pequeño tamaño hasta que llegue el ACK.

3. **Algoritmo de Clark (Evitar muestras erróneas de RTT)**  
   - **Objetivo**: mejorar estimación de RTT y evitar usar muestras inválidas.  
   - **Mecanismo**: no actualizar estimación de RTT cuando el ACK corresponde a un segmento retransmitido.

> Cada uno de estos mecanismos equilibran el uso eficiente del enlace con la necesidad de evitar y recuperarse de la congestión, optimizando el rendimiento global de la transferencia TCP.  

---

## Parte III: Capa de Aplicación y Aplicaciones Multimedia

### Pregunta 11: Funcionamiento de DNS

1. **Entrada del dominio**  
   - El usuario teclea `www.ejemplo.com` en el navegador.

2. **Consulta al resolver local**  
   - El sistema operativo revisa la caché local (OS/DNS cache) y, de no hallarlo, envía una consulta al _Recursive Resolver_ (servidor DNS configurado).

3. **Consulta iterativa al Root Server**  
   - El _Recursive Resolver_ pregunta a uno de los 13 root servers: “¿Quién conoce `.com`?”  
   - Recibe la dirección de los TLD servers para `.com`.

4. **Consulta al TLD Server**  
   - El resolver consulta al TLD server de `.com`: “¿Quién es autoritativo para `ejemplo.com`?”  
   - Obtiene la dirección del _Authoritative Name Server_ para `ejemplo.com`.

5. **Consulta al Authoritative Name Server**  
   - El resolver pregunta al servidor autoritativo: “¿Cuál es la IP de `www.ejemplo.com`?”  
   - Recibe la respuesta (por ejemplo, `93.184.216.34`).

6. **Respuesta al cliente**  
   - El _Recursive Resolver_ almacena en caché la respuesta (TTL) y se la devuelve al cliente (navegador).

7. **Conexión al servidor web**  
   - El navegador abre la conexión TCP a `93.184.216.34` (puerto 80/443) y envía la petición HTTP.

---

### Pregunta 12: Protocolos de Correo Electrónico

| Protocolo | Función principal                            | Uso / Acceso                  | Almacenamiento                         | Ejemplo de uso                                |
|-----------|-----------------------------------------------|-------------------------------|----------------------------------------|-----------------------------------------------|
| **SMTP**  | Envío de correo desde cliente a servidor o entre servidores | **Push:** cliente→servidor → otros servidores | No almacena; entrega directa al MTA destino | Envío de un email desde Outlook a Gmail        |
| **POP3**  | Recuperación de correo desde servidor         | **Pull:** descarga y opcionalmente borra del servidor | Local (cliente suele borrar del servidor) | Usuarios con conexión intermitente que quieren leer offline |
| **IMAP**  | Gestión y sincronización de buzón remoto      | **Pull / Push:** mantiene mensajes en servidor y sincroniza carpetas | Permanente en servidor, múltiples carpetas | Acceso concurrente desde móvil, web y PC       |

---

### Pregunta 13: Funcionamiento de HTTP y FTP

#### a) HTTP

- Modelo **cliente-servidor** sin estado (stateless).
- El cliente abre conexión TCP (puerto 80/443), envía un **request** con:
  - **Método**:  
    - `GET` (recuperar recurso)  
    - `POST` (enviar datos)  
    - `PUT` (actualizar/reemplazar)  
    - `DELETE` (eliminar)  
  - URI, cabeceras y opcionalmente cuerpo.
- El servidor procesa, devuelve **response** con código de estado (200, 404, 500…).
- Cierre o reutilización de la conexión (keep-alive).

#### b) FTP

- Modelo **cliente-servidor** con DOS conexiones TCP:
  1. **Control** (puerto 21): intercambia comandos y respuestas (USER, PASS, LIST, RETR, STOR…).
  2. **Datos** (puerto dinámico o 20): transmite archivos.
- **Modos**:  
  - **Active**: servidor inicia conexión de datos al cliente.  
  - **Passive**: cliente inicia ambas conexiones (más firewall-friendly).
- **Diferencias clave**:  
  - FTP usa siempre dos canales (control + datos); HTTP un único canal para ambos.  
  - FTP mantiene estado (sesión), HTTP es stateless.

---

### Pregunta 14: Streaming y VoIP

#### a) Tipos de streaming

| Tipo                         | Descripción                                                                                                     | Ejemplo                           |
|------------------------------|-----------------------------------------------------------------------------------------------------------------|-----------------------------------|
| **UDP Streaming**            | Envío de paquetes por UDP, sin control de pérdida ni orden; baja latencia pero posible pérdida de datos.       | IPTV multicast en redes locales   |
| **HTTP Streaming**           | Descarga progresiva por HTTP; el cliente solicita segmentos fijos secuencialmente.                               | Vídeo on-demand en YouTube (no DASH) |
| **Adaptive HTTP Streaming**<br>(DASH) | El servidor ofrece varios calidades, el cliente selecciona segmento por segmento según ancho de banda actual. | Netflix, YouTube (DASH/HLS)       |

#### b) VoIP (Voz sobre IP)

1. **Captura y codificación**  
   - Micrófono → códec (G.711, G.729…) → paquetes RTP.
2. **Transporte**  
   - RTP sobre UDP, con RTCP para estadísticas y control.
3. **Recepción y reproducción**  
   - Jitter buffer (almacén temporal) → decodificación → altavoz.

**Problemas comunes y soluciones**  
- **Retardo (delay)**: usar QoS, priorizar paquetes en routers.  
- **Pérdida de paquetes**: FEC (Forward Error Correction), retransmisión selectiva mínima.  
- **Eco**: cancelación de eco en la aplicación.

---

### Pregunta 15: Control de Congestión en Redes Multimedia

1. **Buffering en el cliente**  
   - El reproductor almacena un pequeño búfer de datos antes de reproducir.  
   - Compensa variaciones de ritmo y pérdidas temporales, evita interrupciones.

2. **Marcado de paquetes (DiffServ)**  
   - Clasifica y marca paquetes de multimedia con prioridad alta (EF, AF).  
   - Los routers aplican colas y políticas diferenciadas, garantizando ancho de banda y menor retardo.

---

### Pregunta 16: Best-Effort vs Servicios Multiclase

| Aspecto                             | Best-Effort                                    | Servicios Multiclase                           |
|-------------------------------------|-------------------------------------------------|-------------------------------------------------|
| **Manejo de tráfico**               | Trato igualitario; sin diferenciación           | Clases (p. ej. EF, AF, BE) con colas separadas  |
| **Garantía de QoS**                 | Ninguna, entrega “mejor esfuerzo”               | Garantías de retardo, jitter, tasa de pérdidas  |
| **Ejemplos de uso**                 | Web browsing, email                             | VoIP (EF), streaming critico (AF), datos web (BE) |


---

## Parte IV: Seguridad en Redes

### Pregunta 17: Problemas de Seguridad en Redes

| Área            | Descripción                                              | Técnica de mitigación                        |
|-----------------|----------------------------------------------------------|-----------------------------------------------|
| **Confidencialidad** | Garantizar que solo las partes autorizadas pueden leer la información. | Cifrado de datos (AES, TLS)                  |
| **Autenticación**    | Verificar la identidad de usuarios o dispositivos.     | Autenticación multifactor (MFA), certificados digitales |
| **No repudio**       | Impedir que una parte niegue haber enviado o recibido un mensaje. | Firmas digitales (RSA, ECDSA)                |
| **Integridad**       | Asegurar que los datos no hayan sido alterados en tránsito o almacenamiento. | Hashes criptográficos (SHA-256) y HMAC       |

---

### Pregunta 18: Cifrado Simétrico vs Asimétrico

| Característica       | Cifrado Simétrico                                 | Cifrado Asimétrico                                   |
|----------------------|---------------------------------------------------|------------------------------------------------------|
| **Número de claves** | Una sola clave compartida entre emisor y receptor | Par de claves: pública y privada                     |
| **Velocidad/eficiencia** | Muy rápido, bajo coste computacional          | Más lento, alto coste computacional                  |
| **Algoritmos típicos**  | AES, 3DES, ChaCha20                             | RSA, ECC (ECDSA, ECDH), ElGamal                      |
| **Aplicaciones**        | Cifrado de discos (BitLocker), VPN (IPsec ESP) | Intercambio de claves, firmas digitales, TLS handshake |

---

### Pregunta 19: Funcionamiento del Algoritmo RSA

#### a) Generación de claves

1. **Elegir dos primos grandes** _p_ y _q_.  
2. Calcular \(n = p \times q\).  
3. Calcular \(\varphi(n) = (p-1)\,(q-1)\).  
4. Elegir un entero \(e\) tal que \(1 < e < \varphi(n)\) y \(\gcd(e,\varphi(n))=1\).  
5. Calcular \(d\) como inverso modular de \(e\) módulo \(\varphi(n)\):  
   \[
   d \equiv e^{-1} \pmod{\varphi(n)}
   \]
6. **Clave pública** = \((e, n)\).  
   **Clave privada** = \((d, n)\).

#### b) Ejemplo numérico

- Elegimos \(p = 3\), \(q = 11\).  
- \(n = 3 \times 11 = 33\).  
- \(\varphi(n) = (3-1)\,(11-1) = 2 \times 10 = 20\).  
- Seleccionamos \(e = 7\); \(\gcd(7,20)=1\).  
- Calculamos \(d\) tal que \(7d \equiv 1 \pmod{20}\).  
  \[
    7 \times 3 = 21 \equiv 1 \pmod{20} \;\Longrightarrow\; d = 3
  \]
- **Clave pública** \((e,n)=(7,33)\).  
  **Clave privada** \((d,n)=(3,33)\).

**Cifrado** de \(M=4\):  
\[
C = M^e \bmod n = 4^7 \bmod 33 = 16
\]

**Descifrado**:  
\[
M = C^d \bmod n = 16^3 \bmod 33 = 4
\]

---

### Pregunta 20: Firewalls, VPN e IPSec

#### a) Firewalls

- **Filtrado de paquetes**: inspecciona encabezados IP/puertos y aplica reglas de permit/deny.  
- **Firewall de estado (stateful)**: además de reglas básicas, mantiene tabla de estados de conexiones y permite dinámicamente respuestas a conexiones legítimas iniciadas desde dentro.

**Importancia**: control de acceso basado en políticas; defiende perímetro de la red frente a tráfico no autorizado.

#### b) VPN vs. IPSec

| Aspecto            | VPN (túnel general)                          | IPSec (suite de protocolo)                             |
|--------------------|----------------------------------------------|--------------------------------------------------------|
| **Propósito**      | Conectar usuarios remotos o sucursales a la red | Proveer cifrado y autenticación a nivel IP             |
| **Modo de operación** | Puede usar SSL/TLS (SSL VPN) o IPsec        | Transport mode (host-to-host) y Tunnel mode (gateway-to-gateway) |
| **Ejemplos de uso**| Acceso remoto de empleados via SSL VPN        | Enlace cifrado entre routers de dos oficinas (IPsec tunnel) |

---

### Pregunta 21: SSL/TLS y DNS Spoofing

#### a) SSL/TLS

1. **Handshake**:  
   - Cliente manda `ClientHello` con versiones y cifrados soportados.  
   - Servidor responde `ServerHello`, manda certificado X.509 y clave pública.  
   - Cliente verifica cert, genera clave pre-maestra y la cifra con la pública del servidor.  
2. **Sesión segura**:  
   - Ambos derivan claves simétricas de sesión.  
   - Todas las comunicaciones posteriores van cifradas y autenticadas con MAC.

**Importancia**: garantiza confidencialidad, integridad y autenticación del servidor (y opcionalmente del cliente) en HTTPS.

#### b) DNS Spoofing y DNSSEC

- **DNS Spoofing**: un atacante envía respuestas DNS falsas al resolver o cliente, redirigiendo tráfico a servidores maliciosos.  
- **DNSSEC**: añade firmas digitales (RRSIG) y claves públicas (DNSKEY) a las zonas DNS. Cada respuesta incluye una firma que el resolver verifica contra la clave pública de la zona, asegurando que la respuesta no ha sido alterada y proviene del servidor autoritativo legítimo.


