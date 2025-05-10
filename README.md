# Simulacro-Examen-Redes
Redes
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
- Cada una con 2^(32−26) − 2 = 62 hosts.```



