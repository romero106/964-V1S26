---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - lógica-de-transferencia-entre-registros
  - registros
  - microoperaciones
  - bus-común
  - memoria
---

# Lógica de transferencia entre registros

## 1. Descripción de sistemas digitales grandes

Las tablas de verdad y los diagramas de compuertas son apropiados para circuitos pequeños. Sin embargo, un sistema digital como una computadora contiene demasiados componentes y conexiones para describirlo de manera útil únicamente a ese nivel.

Para estudiar sistemas de mayor tamaño se adopta un nivel de abstracción superior: se consideran los **registros** como componentes básicos y se expresa simbólicamente qué información se transfiere, qué operación se ejecuta y bajo qué condición ocurre.

La **lógica de transferencia entre registros** (LTR) es el método de descripción que representa:

- Los registros que forman el sistema.
- La información binaria almacenada en ellos.
- Las operaciones efectuadas sobre esa información.
- Las funciones de control que determinan cuándo se realizan las operaciones.

> [!note]
> La LTR no elimina los circuitos de compuertas y flip-flops. Los agrupa en bloques funcionales para que sea posible razonar sobre la organización y el funcionamiento del sistema completo.

## 2. Componentes de una descripción LTR

Una descripción completa debe permitir identificar cuatro elementos.

### 2.1 Conjunto de registros

Se especifican los registros disponibles y la función de cada uno. En este contexto, la palabra **registro** tiene un sentido amplio y puede representar:

- Un registro paralelo.
- Un registro de desplazamiento.
- Un contador.
- Una memoria.
- Un flip-flop individual.

Todos ellos almacenan información binaria y pueden participar en operaciones de transferencia o transformación de datos.

### 2.2 Información almacenada

La interpretación de una palabra binaria depende del uso del registro. El mismo patrón puede representar un número, una instrucción, una dirección, un carácter o un conjunto de señales de control.

Si un registro de ocho bits llamado $A$ contiene:

$$
A=10110110
$$

la LTR se ocupa principalmente de dónde está esa información y qué operación se realiza con ella. Su significado depende del sistema descrito.

### 2.3 Operaciones sobre los datos

Las operaciones elementales ejecutadas sobre la información almacenada se llaman **microoperaciones**. El libro las clasifica en cuatro grupos:

| Clase          | Propósito general                                              |
| -------------- | -------------------------------------------------------------- |
| Transferencia  | Copiar información entre registros                             |
| Aritmética     | Sumar, restar, incrementar y realizar operaciones relacionadas |
| Lógica         | Aplicar operaciones booleanas bit a bit                        |
| Desplazamiento | Mover los bits hacia la izquierda o la derecha                 |

Este tema se concentra en las microoperaciones de transferencia. Las demás clases se desarrollan posteriormente.

### 2.4 Funciones de control

Una microoperación se ejecuta solamente cuando su función de control toma el valor requerido. La función puede depender de señales externas, del contador de tiempo, del código de operación o del estado actual del sistema.

## 3. Notación de registros

Los registros se identifican con letras mayúsculas o abreviaturas que sugieren su función. Por ejemplo:

- $A$, $B$, $C$: registros de propósito general.
- $PC$: contador de programa.
- $AR$: registro de direcciones.
- $MBR$: registro separador de memoria.

### 3.1 Bits y partes de un registro

Los bits de un registro pueden numerarse para seleccionar una celda particular. Si $A$ posee ocho bits:

$$
A=A_7A_6A_5A_4A_3A_2A_1A_0
$$

$A_0$ designa el bit menos significativo y $A_7$ el más significativo.

También es posible referirse a una parte del registro. El libro utiliza, por ejemplo:

- $PC(H)$: mitad de orden alto del contador de programa.
- $PC(L)$: mitad de orden bajo del contador de programa.

La notación permite describir transferencias entre registros completos, bits individuales o grupos de bits sin dibujar cada conductor.

## 4. Microoperación de transferencia

La expresión fundamental es:

$$
A\leftarrow B
$$

Se lee: **transferir el contenido del registro $B$ al registro $A$**.

Después de la operación:

$$
A_{nuevo}=B_{anterior}
$$

El contenido de $B$ no cambia. La flecha indica la dirección del flujo de información: el registro de la izquierda es el destino y el de la derecha es la fuente.

> [!warning]
> La transferencia no mueve físicamente los bits dejando vacío el registro fuente. Es una operación de copia: el destino recibe el valor de la fuente y la fuente conserva su contenido.

> **Ejemplo 1:** Transferencia entre dos registros
> 
> Antes del pulso de reloj:
>
> $$
> A=0011,\qquad B=1101
> $$
>
> Al ejecutar:
>
> $$
> A\leftarrow B
> $$
>
> se obtiene:
>
> $$
> A=1101,\qquad B=1101
> $$
>
> El valor anterior de $A$ se reemplaza; $B$ permanece sin modificación.

## 5. Transferencia condicional

Una función de control se escribe a la izquierda de dos puntos:

$$
x'T_1:A\leftarrow B
$$

La transferencia se realiza cuando la función de control $x'T_1$ vale $1$; es decir, cuando $x=0$ y $T_1=1$.

En forma verbal:

> Si $x'T_1=1$, transferir el contenido de $B$ hacia $A$ durante el pulso de reloj correspondiente.

La implementación requiere dos acciones coordinadas:

1. Establecer un camino de datos desde las salidas de $B$ hasta las entradas de $A$.
2. Activar la señal de carga de $A$ cuando la función de control sea verdadera.

Por tanto, la función de carga puede expresarse como:

$$
L_A=x'T_1
$$

Si la función de control es cero, $A$ conserva su contenido.

## 6. Operaciones simultáneas

Una coma separa microoperaciones que deben ocurrir simultáneamente:

$$
xT_3:A\leftarrow B,\quad B\leftarrow A
$$

Cuando $xT_3=1$, ambos registros cargan durante el mismo borde activo del reloj. Cada destino recibe el valor que la fuente tenía **antes** del pulso.

> **Ejemplo 2:** Intercambio simultáneo
> 
> Sean los valores iniciales:
>
> $$
> A=1010,\qquad B=0111
> $$
>
> Al ejecutar:
>
> $$
> A\leftarrow B,\quad B\leftarrow A
> $$
>
> el resultado es:
>
> $$
> A=0111,\qquad B=1010
> $$
>
> No se ejecuta primero una asignación y luego la otra. Los dos registros observan los valores anteriores y cargan al mismo tiempo.

> [!warning]
> Interpretar las microoperaciones simultáneas como instrucciones secuenciales conduce a un resultado incorrecto. En un sistema síncrono, las entradas se preparan antes del borde y todos los registros habilitados cambian juntos en dicho borde.

## 7. Símbolos frecuentes de transferencia

| Símbolo             | Significado                                       |
| ------------------- | ------------------------------------------------- |
| $A\leftarrow B$     | Transferir el contenido de $B$ a $A$              |
| $A\leftarrow A$     | Conservar o volver a cargar el mismo contenido    |
| $A_i\leftarrow B_j$ | Transferir un bit específico                      |
| $PC(H)\leftarrow A$ | Transferir hacia la mitad alta de $PC$            |
| $M[AR]$             | Palabra de memoria cuya dirección está en $AR$    |
| Coma                | Microoperaciones simultáneas                      |
| Dos puntos          | Separa la función de control de la microoperación |

## 8. Un destino con varias fuentes

Un registro puede recibir información desde más de una fuente. Como sus entradas no deben ser accionadas simultáneamente por varias salidas, se necesita un circuito que seleccione una sola fuente.

Para dos registros fuente, un multiplexor por cada bit permite elegir el dato que llegará al destino. Si $A$, $B$ y $C$ tienen $n$ bits y $A$ puede recibir de $B$ o de $C$, las microoperaciones pueden ser:

$$
x:A\leftarrow B
$$

$$
x':A\leftarrow C
$$

En cada posición $i$, un multiplexor selecciona entre $B_i$ y $C_i$; su salida alimenta $A_i$. La señal $x$ selecciona la fuente y la carga de $A$ habilita la transferencia.

> [!note]
> La notación LTR especifica el comportamiento deseado. El camino de datos —multiplexores, buses y conexiones— indica cómo se realiza físicamente.

## 9. Transferencias mediante conexiones directas

La forma más sencilla de interconectar registros consiste en utilizar un conjunto independiente de conductores para cada transferencia posible.

Para implementar:

$$
A\leftarrow B
$$

se conectan las $n$ salidas de $B$ con las $n$ entradas correspondientes de $A$. Si existen muchas fuentes y destinos, el número de conexiones y multiplexores crece rápidamente.

Las conexiones directas son convenientes cuando:

- Hay pocos registros.
- Se requieren pocas transferencias.
- Algunas transferencias deben ocurrir al mismo tiempo por caminos diferentes.

Para sistemas con muchos registros se prefiere compartir líneas mediante un bus común.

## 10. Bus común

Un **bus** es un conjunto de líneas compartidas que transporta una palabra binaria entre componentes. Un bus de $n$ líneas puede transferir en paralelo el contenido de un registro de $n$ bits.

Una organización de bus debe resolver dos selecciones:

1. **Fuente:** qué registro coloca su contenido en el bus.
2. **Destino:** qué registro carga el contenido presente en el bus.

Solo una fuente debe alimentar el bus en un instante determinado, aunque uno o más destinos compatibles podrían cargar su valor si el diseño lo permite.

### 10.1 Bus común con multiplexores

Para cuatro registros $A$, $B$, $C$ y $D$, cada uno de $n$ bits, el libro presenta un bus construido con $n$ multiplexores de cuatro entradas. Cada multiplexor selecciona el mismo bit de uno de los cuatro registros.

Las dos líneas de selección $s_1s_0$ determinan la fuente:

| $s_1s_0$ | Registro colocado en el bus |
| :------: | :-------------------------: |
|    00    |             $A$             |
|    01    |             $B$             |
|    10    |             $C$             |
|    11    |             $D$             |

Las salidas del bus se conectan a las entradas de todos los registros. Un decodificador selecciona cuál destino recibe una señal de carga.

Con las líneas $d_1d_0$, la selección de destino es:

| $d_1d_0$ | Registro de destino |
| :------: | :-----------------: |
|    00    |         $A$         |
|    01    |         $B$         |
|    10    |         $C$         |
|    11    |         $D$         |

En la organización del libro, la entrada de habilitación $e$ del decodificador es activa en bajo:

- $e=0$: se habilita la carga del destino seleccionado.
- $e=1$: ninguna salida del decodificador se activa y no ocurre transferencia.

Por ejemplo, para realizar:

$$
C\leftarrow B
$$

se selecciona $B$ como fuente y $C$ como destino:

$$
s_1s_0=01,\qquad d_1d_0=10,\qquad e=0
$$

La palabra de control completa es:

$$
\boxed{01100}
$$

> **Ejemplo 3:** Interpretación de una palabra de control
> 
> Considérese la palabra:
>
> $$
> s_1s_0d_1d_0e=10010
> $$
>
> Se separan sus campos:
>
> $$
> s_1s_0=10,\qquad d_1d_0=01,\qquad e=0
> $$
>
> El código $10$ coloca $C$ en el bus, el código $01$ selecciona $B$ como destino y $e=0$ habilita el decodificador. Por tanto:
>
> $$
> \boxed{B\leftarrow C}
> $$

### 10.2 Ventajas y limitaciones del bus

El bus común reduce el número de conexiones porque varios registros comparten el mismo camino de datos. A cambio, solo puede transportar una palabra fuente a la vez.

Esto produce una diferencia importante:

- $A\leftarrow B$ y $C\leftarrow B$ podrían compartir la misma fuente si el circuito permite cargar ambos destinos.
- $A\leftarrow B$ y $C\leftarrow D$ requieren dos valores fuente diferentes al mismo tiempo, por lo que un único bus no basta para ejecutarlas simultáneamente.

## 11. Transferencias con memoria

Una memoria se considera una colección de registros. Para transferir una palabra es necesario indicar:

- La dirección de la palabra seleccionada.
- Si la operación es de lectura o escritura.
- El registro que entrega o recibe los datos.

### 11.1 Lectura de memoria

La lectura transfiere el contenido de la palabra seleccionada hacia un registro sin modificar la memoria. En forma general:

$$
R:MBR\leftarrow M
$$

Si la dirección se especifica dentro de corchetes:

$$
R:B_0\leftarrow M[A_3]
$$

significa que, cuando la función de lectura $R$ es verdadera, la palabra de memoria cuya dirección está en $A_3$ se copia al registro $B_0$.

### 11.2 Escritura en memoria

La escritura transfiere el contenido de un registro hacia la palabra seleccionada y reemplaza su valor anterior:

$$
W:M\leftarrow MBR
$$

Con dirección y fuente explícitas:

$$
W:M[A_1]\leftarrow B_2
$$

La dirección almacenada en $A_1$ selecciona la palabra de memoria, y el contenido de $B_2$ constituye el dato que se escribe.

> [!warning]
> En $M[A_1]\leftarrow B_2$, $A_1$ no es el destino de la transferencia. Su contenido se interpreta como una dirección; el destino real es la palabra de memoria seleccionada.

### 11.3 Organización con varios registros

En la configuración del libro aparecen dos grupos de registros:

- $A_0,A_1,A_2,A_3$: posibles fuentes de dirección.
- $B_0,B_1,B_2,B_3$: posibles fuentes o destinos de datos.

La organización emplea:

- Un multiplexor para seleccionar qué registro $A_i$ proporciona la dirección.
- Un multiplexor para seleccionar qué registro $B_i$ proporciona el dato de escritura.
- La salida de lectura de la memoria conectada a los registros $B_i$.
- Un decodificador para habilitar el registro $B_i$ que recibirá el dato leído.

Para una escritura se seleccionan una fuente de dirección y una fuente de datos. Para una lectura se seleccionan una fuente de dirección y un destino de datos.

## 12. Relación entre control, camino de datos y reloj

Una sentencia LTR reúne tres ideas que conviene distinguir:

| Elemento                   | Pregunta que responde             |
| -------------------------- | --------------------------------- |
| Función de control         | ¿Cuándo debe ejecutarse?          |
| Camino de datos            | ¿Por dónde viajan los bits?       |
| Señal de carga o escritura | ¿Qué componente cambia de estado? |

En un sistema síncrono, las señales de selección y control se establecen durante un intervalo de reloj. En el borde activo, el destino habilitado almacena el dato presente en su entrada.

Por ejemplo, en:

$$
xT_2:C\leftarrow A
$$

si $xT_2=1$:

1. $A$ se selecciona como fuente.
2. Su contenido aparece en el camino de datos.
3. Se habilita la carga de $C$.
4. $C$ recibe el valor en el borde activo del reloj.

Si $xT_2=0$, no se habilita la carga y $C$ conserva su estado.

## 13. Procedimiento para interpretar una sentencia LTR

Puede utilizarse el siguiente orden:

1. **Identificar la condición de control.** Determinar cuándo vale $1$.
2. **Localizar el destino.** Es el elemento situado a la izquierda de la flecha.
3. **Localizar la fuente.** Es la expresión situada a la derecha.
4. **Determinar el alcance.** Comprobar si se transfiere un registro completo, una parte, un bit o una palabra de memoria.
5. **Detectar simultaneidad.** Las operaciones separadas por comas ocurren durante el mismo pulso.
6. **Traducir al camino de datos.** Elegir fuente, destino, señales de selección y habilitación.
7. **Comprobar el efecto final.** El destino cambia; una fuente utilizada solo para lectura conserva su contenido.

## 14. Errores comunes

- Leer la flecha en sentido contrario. En $A\leftarrow B$, $A$ es el destino.
- Suponer que la fuente pierde su contenido después de transferirlo.
- Ejecutar secuencialmente microoperaciones separadas por comas.
- Seleccionar dos fuentes distintas sobre un mismo bus al mismo tiempo.
- Olvidar la polaridad de la habilitación del decodificador.
- Confundir el registro de dirección con la palabra de memoria seleccionada.
- Describir solo la transferencia y omitir la condición que la controla.

# Verificación del aprendizaje

Los siguientes problemas se basan en los ejercicios del capítulo dedicado a la lógica de transferencia entre registros.

## Problema 1

Muestre una configuración de bloques que permita ejecutar la siguiente sentencia:

$$
xT_3:A\leftarrow B,\quad B\leftarrow A
$$

Indique los caminos de datos y la función que controla la carga de cada registro.

## Problema 2

En un sistema de bus común con cuatro registros $A$, $B$, $C$ y $D$, la palabra de control tiene el formato:

$$
s_1s_0d_1d_0e
$$

Los códigos $00$, $01$, $10$ y $11$ representan respectivamente $A$, $B$, $C$ y $D$, tanto para la fuente como para el destino. La habilitación $e$ del decodificador es activa en bajo.

### a)

Determine la transferencia que produce cada palabra:

1. $00010$
2. $01000$
3. $11100$
4. $01101$

### b)

Obtenga la palabra de control necesaria para cada transferencia:

1. $A\leftarrow B$
2. $B\leftarrow C$
3. $D\leftarrow A$

## Problema 3

Considere una memoria conectada a cuatro registros de dirección $A_0,A_1,A_2,A_3$ y cuatro registros de datos $B_0,B_1,B_2,B_3$. Los multiplexores y el decodificador usan los códigos:

| Código | Registro seleccionado |
| :----: | :-------------------: |
|   00   |      Subíndice 0      |
|   01   |      Subíndice 1      |
|   10   |      Subíndice 2      |
|   11   |      Subíndice 3      |

Indique las selecciones necesarias y el tipo de operación para ejecutar:

1. $M[A_2]\leftarrow B_3$
2. $B_2\leftarrow M[A_3]$

> **Soluciones**
>
> **Problema 1**
>
> Se requieren dos caminos de datos cruzados:
>
> - Las salidas de $B$ se conectan a las entradas de $A$.
> - Las salidas de $A$ se conectan a las entradas de $B$.
>
> La misma función de control habilita ambos registros:
>
> $$
> L_A=xT_3,\qquad L_B=xT_3
> $$
>
> Antes del borde activo, cada registro presenta su valor anterior a la entrada del otro. En el borde, ambos cargan simultáneamente, por lo que intercambian sus contenidos.
>
> **Problema 2**
>
> **a) Interpretación de las palabras**
>
> 1. $00010$:
>
> $$
> s_1s_0=00,\quad d_1d_0=01,\quad e=0
> $$
>
> La fuente es $A$, el destino es $B$ y el decodificador está habilitado:
>
> $$
> \boxed{B\leftarrow A}
> $$
>
> 2. $01000$:
>
> $$
> s_1s_0=01,\quad d_1d_0=00,\quad e=0
> $$
>
> Por tanto:
>
> $$
> \boxed{A\leftarrow B}
> $$
>
> 3. $11100$:
>
> $$
> s_1s_0=11,\quad d_1d_0=10,\quad e=0
> $$
>
> Por tanto:
>
> $$
> \boxed{C\leftarrow D}
> $$
>
> 4. $01101$:
>
> Aunque $s_1s_0=01$ selecciona $B$ y $d_1d_0=10$ corresponde a $C$, se tiene $e=1$. Como la habilitación es activa en bajo, el decodificador permanece deshabilitado:
>
> $$
> \boxed{\text{No ocurre transferencia}}
> $$
>
> **b) Construcción de las palabras**
>
> 1. Para $A\leftarrow B$:
>
> $$
> s_1s_0=01,\quad d_1d_0=00,\quad e=0
> $$
>
> $$
> \boxed{01000}
> $$
>
> 2. Para $B\leftarrow C$:
>
> $$
> s_1s_0=10,\quad d_1d_0=01,\quad e=0
> $$
>
> $$
> \boxed{10010}
> $$
>
> 3. Para $D\leftarrow A$:
>
> $$
> s_1s_0=00,\quad d_1d_0=11,\quad e=0
> $$
>
> $$
> \boxed{00110}
> $$
>
> **Problema 3**
>
> 1. En $M[A_2]\leftarrow B_3$ se realiza una **escritura**:
>
> - El multiplexor de dirección selecciona $A_2$: código $10$.
> - El multiplexor de datos selecciona $B_3$: código $11$.
> - El decodificador de destino no participa y debe permanecer deshabilitado.
> - Se activa la señal de escritura de la memoria.
>
> 2. En $B_2\leftarrow M[A_3]$ se realiza una **lectura**:
>
> - El multiplexor de dirección selecciona $A_3$: código $11$.
> - El multiplexor de datos de escritura no se utiliza: sus entradas de selección son indiferentes, $XX$.
> - El decodificador selecciona $B_2$: código $10$, y se habilita.
> - Se activa la señal de lectura de la memoria.

<p align="center">
  <a href="./Tema%202.md">Siguiente tema →</a>
</p>
