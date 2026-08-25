---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - lógica-secuencial
  - bloques-secuenciales
  - registros
  - registros-de-desplazamiento
  - contadores
  - memoria
---

# Bloques digitales secuenciales de mediana y alta escala de integración

## 1. Bloques secuenciales integrados

Un circuito secuencial temporizado contiene elementos de almacenamiento y, normalmente, lógica combinacional conectada con realimentación. Los flip-flops conservan el estado; las compuertas determinan cómo debe cambiar.

Cuando varias celdas de almacenamiento y su lógica de control se integran en un solo circuito, resulta más útil identificar el componente por la función que realiza. El libro agrupa estos bloques en tres categorías:

| Bloque            | Función principal                                     |
| ----------------- | ----------------------------------------------------- |
| Registro          | Almacenar y transferir una palabra binaria.           |
| Contador          | Recorrer una secuencia predeterminada de estados.     |
| Unidad de memoria | Almacenar numerosas palabras y permitir su selección. |

Los registros y contadores suelen presentarse como circuitos MSI. Una memoria reúne un número mucho mayor de celdas y constituye un ejemplo natural de integración a gran escala.

> [!note]
> Un bloque integrado no deja de ser secuencial por ocultar sus flip-flops internos. Si puede conservar información después de retirar las entradas que la produjeron, contiene estado.

## 2. Registros

Un **registro** es un grupo de celdas binarias capaz de retener información. Cada flip-flop almacena un bit, por lo que un registro de $n$ bits contiene $n$ flip-flops y almacena una palabra de $n$ bits.

~~~text
Entradas:   I3  I2  I1  I0
             │   │   │   │
           ┌─┴─┐┌┴──┐┌┴──┐┌┴──┐
CP ───────►│ D ││ D ││ D ││ D │
           └─┬─┘└─┬─┘└─┬─┘└─┬─┘
             │    │    │    │
Salidas:    A3   A2   A1   A0
~~~

Todos los flip-flops reciben el mismo reloj. En el flanco activo, los bits de entrada se almacenan simultáneamente:

$$
A_3A_2A_1A_0\leftarrow I_3I_2I_1I_0
$$

### 2.1 Retenedor y registro

El libro distingue entre dos comportamientos:

| Unidad              | Respuesta temporal                                       |
| ------------------- | -------------------------------------------------------- |
| Retenedor o *latch* | Permanece habilitado durante todo el nivel activo.       |
| Registro            | Captura la información durante una transición del reloj. |

Los retenedores sirven para almacenamiento temporal, pero los circuitos con realimentación requieren especial cuidado. Para registros temporizados se prefieren flip-flops disparados por flanco o maestro-esclavo, porque limitan el cambio a un acontecimiento bien definido.

## 3. Carga en paralelo

La transferencia de información nueva hacia un registro se denomina **carga**. Si todos los bits se reciben con un mismo pulso, se realiza una **carga en paralelo**.

Una entrada de control $C$ puede seleccionar entre:

- $C=0$: conservar la palabra presente.
- $C=1$: cargar la palabra de entrada.

Para un registro construido con flip-flops D, cada etapa cumple:

$$
\boxed{D_i=CI_i+C'A_i}
$$

donde $I_i$ es el bit de entrada y $A_i$ el bit almacenado.

> **Ejemplo**
> 
> El registro contiene $A_3A_2A_1A_0=1010$ y las entradas son $I_3I_2I_1I_0=0111$.
>
> - Si $C=0$, el siguiente pulso conserva $1010$.
> - Si $C=1$, el siguiente pulso carga $0111$.

### 3.1 Controlar datos o controlar reloj

Una forma de habilitar un registro consiste en colocar una compuerta en el camino del reloj. Sin embargo, los retardos introducidos en diferentes ramas pueden desincronizar los flip-flops.

El libro recomienda aplicar el reloj común directamente y controlar la operación mediante las entradas de datos o excitación. Así, todos los elementos reciben el mismo flanco y la variable de control decide si cargan o conservan.

> [!warning]
> Insertar lógica combinacional diferente en varias ramas de reloj puede hacer que los flancos lleguen en instantes distintos. La coordinación temporal del bloque debe preservarse.

## 4. Registro y lógica combinacional

Un circuito secuencial puede representarse con dos bloques:

~~~mermaid
flowchart LR
    I["Entradas"] --> C["Circuito combinacional"]
    Q["Estado presente<br/>registro"] --> C
    C --> N["Estado siguiente"]
    C --> Y["Salidas"]
    N --> R["Registro"]
    CP["Reloj"] --> R
    R --> Q
~~~

La lógica combinacional puede realizarse con compuertas o sustituirse por una ROM que almacene la tabla de transición.

### 4.1 Realización con ROM

En el ejemplo 7-2 del libro, el estado presente ocupa dos bits y existe una entrada externa $x$. Por tanto, la ROM necesita tres líneas de dirección:

$$
A_1,A_2,x
$$

La ROM produce los dos bits del estado siguiente y una salida $y$. Se requieren tres líneas de salida y la organización es:

$$
2^3\times3=8\times3
$$

Las dos salidas de estado alimentan los flip-flops y la tercera constituye la salida externa.

> [!note]
> Una ROM puede implementar la parte combinacional porque cada combinación de estado presente y entrada funciona como una dirección que selecciona los valores de estado siguiente y salida.

## 5. Registros de desplazamiento

Un **registro de desplazamiento** mueve la información almacenada una posición por cada pulso activo.

En un desplazamiento a la derecha:

$$
A_3^+=SI,\qquad A_2^+=A_3,\qquad A_1^+=A_2,\qquad A_0^+=A_1
$$

- $SI$ es la entrada serial.
- $A_0$ proporciona la salida serial $SO$.
- El bit que abandona $SO$ se pierde si no se conecta a otro destino.

~~~text
SI ──►[ A3 ]──►[ A2 ]──►[ A1 ]──►[ A0 ]──► SO
             desplazamiento a la derecha
~~~

Según sus conexiones, un registro puede desplazar:

- Solo a la derecha.
- Solo a la izquierda.
- En ambas direcciones.
- Con entrada y salida serial.
- Con carga o lectura en paralelo.

## 6. Transferencia serial y paralela

En una transferencia en paralelo, los $n$ bits de una palabra viajan simultáneamente por $n$ líneas. En una transferencia serial, los bits recorren una sola línea, uno por pulso.

| Característica                | Paralela | Serial |
| ----------------------------- | -------- | ------ |
| Líneas de datos para $n$ bits | $n$      | 1      |
| Pulsos para una palabra       | 1        | $n$    |
| Hardware de conexión          | Mayor    | Menor  |
| Tiempo de transferencia       | Menor    | Mayor  |

Para transferir en serie una palabra de cuatro bits del registro $A$ al registro $B$:

1. Se conecta $SO_A$ con $SI_B$.
2. Ambos registros reciben los mismos cuatro pulsos.
3. Cada pulso mueve un bit de $A$ hacia $B$.
4. Después de cuatro pulsos, $B$ contiene la palabra original de $A$.

El contenido anterior de $B$ sale serialmente y debe enviarse a otro registro si se desea conservarlo.

## 7. Sumador en serie

Un sumador en serie procesa un par de bits por pulso. Utiliza dos registros de desplazamiento, un sumador completo, un flip-flop que almacena el acarreo y un control que habilita tantos pulsos como bits tenga la palabra.

Las salidas seriales de los registros suministran $x$ y $y$. El estado presente $Q$ es el acarreo anterior.

La suma es:

$$
\boxed{S=x\oplus y\oplus Q}
$$

El acarreo siguiente es:

$$
\boxed{Q^+=xy+xQ+yQ}
$$

Con un flip-flop D:

$$
D_Q=xy+xQ+yQ
$$

El ejemplo 7-3 del libro también obtiene una realización con JK:

$$
\boxed{J_Q=xy}
$$

$$
\boxed{K_Q=x'y'=(x+y)'}
$$

### 7.1 Operación

1. Se cargan los sumandos en los registros $A$ y $B$.
2. El flip-flop de acarreo se borra a $Q=0$.
3. Las salidas seriales presentan el primer par de bits.
4. El sumador calcula $S$ y el nuevo acarreo.
5. En el pulso, $S$ entra al registro de resultado y $Q$ almacena el acarreo.
6. El proceso se repite para los demás bits.

### 7.2 Comparación con el sumador paralelo

| Característica                    | Sumador paralelo             | Sumador en serie |
| --------------------------------- | ---------------------------- | ---------------- |
| Sumadores completos para $n$ bits | $n$                          | 1                |
| Tiempo aproximado                 | Una operación combinacional  | $n$ pulsos       |
| Almacenamiento del acarreo        | Se propaga entre etapas      | Un flip-flop     |
| Naturaleza                        | Principalmente combinacional | Secuencial       |

El sumador serial emplea menos hardware aritmético, pero necesita más tiempo.

## 8. Contadores integrados

Un contador es un registro cuyas conexiones internas producen una secuencia predeterminada después de cada pulso de cuenta.

Los contadores se clasifican en:

- **Contadores de rizado:** un flip-flop dispara al siguiente.
- **Contadores sincrónicos:** todos los flip-flops reciben el mismo reloj.

## 9. Contadores de rizado

En un **contador de rizado**, el bit menos significativo recibe los pulsos externos. Su salida sirve como reloj del siguiente flip-flop, y así sucesivamente.

~~~text
Pulsos ──►[ A0 ]──►[ A1 ]──►[ A2 ]──►[ A3 ]
              reloj de cada etapa viene de la anterior
~~~

Si los flip-flops JK tienen $J=K=1$, cada etapa se complementa cuando recibe su flanco activo.

### 9.1 División de frecuencia

Cada flip-flop divide entre dos la frecuencia recibida:

$$
f_{A_0}=\frac{f_{CP}}2,\qquad
f_{A_1}=\frac{f_{CP}}4,\qquad
f_{A_2}=\frac{f_{CP}}8
$$

Con $n$ etapas:

$$
f_{A_{n-1}}=\frac{f_{CP}}{2^n}
$$

### 9.2 Propagación del rizado

Las salidas no cambian al mismo tiempo. La transición se propaga etapa por etapa, por lo que un contador de $n$ bits puede presentar un retardo máximo aproximado de:

$$
t_{máx}=n\,t_{pd}
$$

donde $t_{pd}$ es el retardo de un flip-flop.

Durante ese intervalo pueden aparecer configuraciones transitorias que no pertenecen a la cuenta estable.

> [!warning]
> Las salidas de un contador de rizado no deben interpretarse como una palabra válida inmediatamente después del pulso. Debe esperarse a que termine la propagación.

## 10. Contador decimal de rizado

Un contador BCD decimal recorre diez estados:

$$
0000\rightarrow0001\rightarrow\cdots\rightarrow1001\rightarrow0000
$$

Los códigos 1010 a 1111 no pertenecen a la secuencia decimal. La lógica de realimentación modifica la operación del contador binario para regresar a cero después de nueve.

Varios contadores BCD pueden conectarse en cascada. El acarreo de una década incrementa la siguiente:

~~~text
Pulsos ──►[ unidades ]──►[ decenas ]──►[ centenas ]
              0–9             0–9             0–9
~~~

## 11. Contadores sincrónicos

En un **contador sincrónico**, todos los flip-flops reciben directamente el mismo reloj. Las entradas de excitación determinan cuáles deben complementarse en el próximo flanco.

Para un contador binario creciente de cuatro bits con flip-flops T:

$$
T_{A_0}=1
$$

$$
T_{A_1}=A_0
$$

$$
T_{A_2}=A_1A_0
$$

$$
T_{A_3}=A_2A_1A_0
$$

Cada bit cambia cuando todos los bits de menor orden son $1$.

### 11.1 Habilitación de cuenta

Si $E$ habilita la cuenta:

$$
T_{A_0}=E,\qquad
T_{A_1}=EA_0,\qquad
T_{A_2}=EA_1A_0,\qquad
T_{A_3}=EA_2A_1A_0
$$

Con $E=0$, todos los flip-flops conservan su estado. Con $E=1$, el contador avanza normalmente.

### 11.2 Creciente y decreciente

- Para contar hacia arriba, un bit se complementa cuando todos los inferiores son $1$.
- Para contar hacia abajo, se complementa cuando todos los inferiores son $0$.

Una entrada de dirección selecciona cuál de estas condiciones alimenta cada etapa.

### 11.3 Comparación

| Característica       | Rizado                                     | Sincrónico                   |
| -------------------- | ------------------------------------------ | ---------------------------- |
| Reloj                | Solo llega directamente a la primera etapa | Llega a todas las etapas     |
| Cambio de salidas    | Se propaga una tras otra                   | Se inicia simultáneamente    |
| Retardo acumulado    | Aumenta con el número de etapas            | No se acumula del mismo modo |
| Lógica de excitación | Más sencilla                               | Más compuertas               |
| Velocidad            | Menor                                      | Mayor                        |

## 12. Contador con carga en paralelo

Un contador integrado puede reunir las funciones de borrado, conservación, carga de una palabra inicial y cuenta binaria.

| Borrado | Flanco de reloj | Carga | Conteo | Operación                      |
| :-----: | :-------------: | :---: | :----: | ------------------------------ |
|    0    |       $X$       |  $X$  |  $X$   | Poner el contador a cero.      |
|    1    |     No hay      |  $X$  |  $X$   | Conservar.                     |
|    1    |   $\uparrow$    |   1   |  $X$   | Cargar las entradas paralelas. |
|    1    |   $\uparrow$    |   0   |   0    | Conservar.                     |
|    1    |   $\uparrow$    |   0   |   1    | Avanzar a la siguiente cuenta. |

### 12.1 Contador de módulo $N$

Un contador de $n$ bits posee $2^n$ estados. Para obtener una secuencia repetida de $N$ estados mediante carga paralela, puede cargarse:

$$
P=2^n-N
$$

y contar desde $P$ hasta $2^n-1$.

> **Ejemplo**
> 
> Para construir un contador módulo 6 con cuatro bits:
>
> $$
> P=2^4-6=16-6=10=1010_2
> $$
>
> La secuencia es:
>
> $$
> 1010\rightarrow1011\rightarrow1100\rightarrow1101
> \rightarrow1110\rightarrow1111\rightarrow1010
> $$
>
> Al terminar la sexta cuenta, la carga paralela restaura $1010$.

## 13. Generación de secuencias de tiempo

Las señales de tiempo activan operaciones en instantes determinados. Pueden obtenerse mediante contadores o registros de desplazamiento.

### 13.1 Tiempo de palabra

Si una operación serial procesa una palabra de ocho bits, debe permanecer habilitada durante ocho pulsos. Un contador de tres bits puede medir ese intervalo porque:

$$
2^3=8
$$

Una señal de comienzo habilita la cuenta y una señal de parada la deshabilita al completar el tiempo de palabra.

### 13.2 Contador de anillo

Un **contador de anillo** es un registro de desplazamiento circular que conserva un único $1$:

$$
1000\rightarrow0100\rightarrow0010\rightarrow0001\rightarrow1000
$$

Cada salida actúa directamente como una señal de tiempo. Para generar $k$ señales se necesitan $k$ flip-flops.

### 13.3 Contador binario con decodificador

Un contador binario de $n$ bits conectado a un decodificador de $n$ a $2^n$ líneas produce $2^n$ señales de tiempo. Utiliza menos flip-flops que el contador de anillo, aunque necesita el decodificador.

### 13.4 Contador Johnson

Un **contador Johnson** realimenta la salida complementada del último flip-flop hacia la entrada del primero. Con $k$ flip-flops genera $2k$ estados.

Para cuatro etapas, una secuencia posible es:

$$
0000\rightarrow1000\rightarrow1100\rightarrow1110
\rightarrow1111\rightarrow0111\rightarrow0011\rightarrow0001
\rightarrow0000
$$

Sus señales pueden decodificarse usando pares de salidas normales o complementadas. Como existen estados ajenos a la secuencia válida, debe comprobarse la inicialización o proporcionar un medio para corregirlos.

## 14. Unidad de memoria

Una **unidad de memoria** es una colección de celdas de almacenamiento junto con los circuitos necesarios para introducir y extraer información.

La información se organiza en palabras:

- Una palabra contiene $n$ bits.
- La memoria almacena $m$ palabras.
- Su capacidad se expresa como $m\times n$.

Por ejemplo:

$$
1024\times8
$$

representa 1024 palabras de 8 bits, con una capacidad total de:

$$
1024\cdot8=8192\text{ bits}
$$

## 15. Dirección, MAR y MBR

Para seleccionar una entre $m$ palabras se requieren $k$ bits de dirección tales que:

$$
2^k\geq m
$$

Si $m$ es una potencia de dos:

$$
\boxed{k=\log_2m}
$$

El libro utiliza dos registros asociados:

- **MAR**: registro de direcciones de memoria. Conserva la dirección seleccionada.
- **MBR**: registro separador de memoria. Conserva la palabra que entra o sale.

~~~mermaid
flowchart LR
    MAR["MAR<br/>dirección"] --> M["Unidad de memoria<br/>m palabras × n bits"]
    M <--> MBR["MBR<br/>palabra de n bits"]
    C["Lectura / escritura"] --> M
~~~

El MAR necesita $k$ flip-flops y el MBR necesita $n$ flip-flops.

## 16. Operaciones de lectura y escritura

### 16.1 Lectura

1. Transferir al MAR la dirección de la palabra seleccionada.
2. Activar la señal de lectura.
3. Transferir la palabra desde la memoria hacia el MBR.

Simbólicamente:

$$
MBR\leftarrow M[MAR]
$$

La memoria integrada permite normalmente una lectura no destructiva: la palabra permanece almacenada.

### 16.2 Escritura

1. Transferir la dirección al MAR.
2. Transferir la palabra nueva al MBR.
3. Activar la señal de escritura.

Simbólicamente:

$$
M[MAR]\leftarrow MBR
$$

La palabra nueva sustituye a la que ocupaba esa dirección.

> **Ejemplo**
> 
> Una memoria de $1024\times8$ utiliza un MAR de 10 bits y un MBR de 8 bits.
>
> Si $MAR=42$ y se ejecuta una lectura, la palabra almacenada en la dirección 42 pasa al MBR. Si se coloca una palabra nueva en el MBR y se ejecuta una escritura, esa palabra reemplaza el contenido de la dirección 42.

## 17. Características de una memoria

### 17.1 Acceso aleatorio y secuencial

- En una memoria de **acceso aleatorio**, el tiempo para acceder a una palabra es esencialmente independiente de su posición.
- En una memoria de **acceso secuencial**, deben recorrerse posiciones hasta alcanzar la información deseada.

### 17.2 Tiempo de acceso y tiempo de ciclo

- **Tiempo de acceso:** intervalo entre iniciar una operación y disponer de la información solicitada.
- **Tiempo de ciclo:** intervalo mínimo entre dos operaciones sucesivas de memoria.

### 17.3 Volatilidad

- Una memoria **volátil** pierde la información al retirar la energía.
- Una memoria **no volátil** conserva la información sin alimentación.

### 17.4 Lectura destructiva

Una lectura es destructiva si la operación elimina el dato almacenado. En ese caso, la unidad debe restaurar inmediatamente la palabra después de leerla.

## 18. Memoria de circuito integrado

Una memoria RAM integrada organiza celdas binarias en filas y columnas. Las líneas de dirección alimentan un decodificador que selecciona una palabra.

En una celda basada en flip-flop intervienen:

- Una línea de selección.
- Una entrada de datos.
- Una salida de datos.
- Una señal de lectura/escritura.

Cuando una palabra no está seleccionada, sus celdas conservan el contenido y no deben intervenir en las líneas de salida. Las salidas de las celdas seleccionadas forman la palabra leída.

Una memoria de $2^k$ palabras necesita un decodificador de $k$ a $2^k$ líneas. Cada salida del decodificador habilita una palabra completa.

## 19. Memoria de núcleos magnéticos

El libro también estudia la memoria de núcleos magnéticos. Cada núcleo puede magnetizarse en dos direcciones para representar $0$ o $1$.

Sus propiedades principales son:

- Conserva la información sin energía, por lo que es no volátil.
- La lectura es destructiva.
- Después de leer debe ejecutarse un ciclo de restauración.
- Utiliza líneas de selección, un alambre sensor y circuitos de escritura.

Aunque corresponde a una tecnología histórica, permite observar claramente la diferencia entre almacenamiento no volátil y lectura destructiva.

## 20. Procedimiento para interpretar un bloque secuencial

1. Identifique cuántos bits almacena.
2. Distinga entradas de datos, control y reloj.
3. Determine si la transferencia es serial o paralela.
4. Establezca la prioridad entre borrado, carga, conteo y conservación.
5. Identifique el flanco activo.
6. Calcule el estado después de cada pulso, no durante el intervalo entre pulsos.
7. En contadores, compruebe la transición de la última cuenta a la primera.
8. En memorias, separe dirección, dato y orden de lectura o escritura.
9. Verifique los estados o direcciones que no se utilizan.

### Errores frecuentes

| Error                                                     | Corrección                                                           |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| Confundir registro con retenedor                          | Determine si responde a un nivel o a un flanco.                      |
| Cambiar el registro cuando carga está inactiva            | Debe realimentarse y conservar la palabra.                           |
| Invertir el sentido del desplazamiento                    | Escriba explícitamente qué etapa recibe $SI$ y cuál entrega $SO$.    |
| Esperar una palabra serial completa en un pulso           | Una palabra de $n$ bits requiere $n$ pulsos.                         |
| Olvidar borrar el acarreo antes de una suma serial        | Inicialice el flip-flop de acarreo cuando no existe acarreo previo.  |
| Suponer que las salidas de rizado cambian simultáneamente | Considere el retardo acumulado.                                      |
| Omitir el retorno de la última cuenta                     | Toda secuencia repetida debe cerrar su ciclo.                        |
| Confundir carga con conteo                                | Respete la prioridad indicada por la tabla funcional.                |
| Creer que el reloj es un dato de la cuenta                | El reloj provoca la transición; el estado contiene el valor contado. |
| Confundir MAR y MBR                                       | MAR guarda una dirección; MBR guarda una palabra.                    |
| Leer sin seleccionar una dirección                        | Primero establezca el MAR y después active lectura.                  |
| Suponer que toda lectura conserva el dato                 | Compruebe si la tecnología tiene lectura destructiva.                |

# Verificación del aprendizaje

**Problema 1:** resuelva el problema 7-6 del libro. Un registro de desplazamiento de cuatro bits contiene inicialmente $1101$. Se desplaza seis veces a la derecha y la entrada serial recibe, en orden, la secuencia $101101$. Determine el contenido después de cada desplazamiento.

**Problema 2:** resuelva el problema 7-13 del libro. Cada flip-flop tiene una demora de $20\text{ ns}$ desde el flanco aplicado en $CP$ hasta la complementación de su salida. Determine la demora máxima de un contador binario de rizado de diez bits y su frecuencia máxima de operación.

**Problema 3:** resuelva el problema 7-32 del libro:

1. Una memoria tiene una capacidad de 8192 palabras de 32 bits. ¿Cuántos flip-flops necesitan el MAR y el MBR?
2. ¿Cuántas palabras contiene una memoria si su registro de dirección tiene 15 bits?

> **Soluciones**
>
> **Problema 1**
>
> En un desplazamiento a la derecha, el nuevo bit serial entra por la izquierda. Tomando primero el bit situado más a la izquierda de la secuencia $101101$:
>
>|Pulso|Entrada serial|Contenido anterior|Contenido nuevo|Bit que sale|
>|:---:|:---:|:---:|:---:|:---:|
>|0|-|1101|1101|-|
>|1|1|1101|1110|1|
>|2|0|1110|0111|0|
>|3|1|0111|1011|1|
>|4|1|1011|1101|1|
>|5|0|1101|0110|1|
>|6|1|0110|1011|0|
>
> Por tanto:
>
> $$
> \boxed{1110,\ 0111,\ 1011,\ 1101,\ 0110,\ 1011}
> $$
>
> **Problema 2**
>
> En el peor caso, la transición debe propagarse por los diez flip-flops:
>
> $$
> t_{máx}=10(20\text{ ns})=200\text{ ns}
> $$
>
> El período de entrada no debe ser menor que esa demora:
>
> $$
> f_{máx}=\frac{1}{200\times10^{-9}}
> $$
>
> $$
> \boxed{f_{máx}=5\text{ MHz}}
> $$
>
> **Problema 3**
>
> Para 8192 palabras:
>
> $$
> 8192=2^{13}
> $$
>
> El MAR necesita 13 flip-flops. Como cada palabra contiene 32 bits, el MBR necesita 32:
>
> $$
> \boxed{MAR=13\text{ flip-flops},\qquad MBR=32\text{ flip-flops}}
> $$
>
> Con un registro de dirección de 15 bits pueden seleccionarse:
>
> $$
> 2^{15}=32768
> $$
>
> palabras. Por tanto:
>
> $$
> \boxed{32768\text{ palabras}}
> $$

<p align="center">
  <a href="./Tema%205.md">← Tema anterior</a> | <a href="./Tema%207.md">Siguiente tema →</a>
</p>
