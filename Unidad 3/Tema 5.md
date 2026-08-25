---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - diseño-con-ltr
  - camino-de-datos
  - unidad-de-control
  - ciclo-de-instrucción
  - computador-sencillo
---

# Diseño con LTR

## 1. De la especificación al circuito

La lógica de transferencia entre registros permite comenzar el diseño describiendo **qué debe hacer** el sistema antes de decidir **cómo construirlo**.

Una lista de proposiciones LTR especifica:

- Los registros que reciben información.
- Las fuentes de los datos.
- Las operaciones que deben ejecutarse.
- El orden temporal de las operaciones.
- Las condiciones que habilitan cada acción.

A partir de ella pueden deducirse:

- Los registros y sus funciones.
- Las conexiones y multiplexores del camino de datos.
- Las operaciones de memoria.
- Las entradas de carga, incremento y borrado.
- La lógica combinacional de la unidad de control.

El libro desarrolla este procedimiento mediante un computador deliberadamente pequeño. Su propósito no es representar una CPU moderna, sino mostrar con claridad cómo una especificación se transforma en material digital.

## 2. Etapas del diseño

El proceso seguido en el ejemplo puede resumirse así:

$$
\text{Instrucciones}
\longrightarrow
\text{Macrooperaciones}
\longrightarrow
\text{Microoperaciones temporizadas}
\longrightarrow
\text{Funciones de control}
\longrightarrow
\text{Camino de datos}
$$

Cada nivel añade detalles sin cambiar el comportamiento requerido.

> [!note]
> Comenzar por el diagrama final dificulta comprobar si el circuito ejecuta correctamente todas las instrucciones. La LTR permite obtener el diagrama de manera sistemática a partir de las operaciones.

# Especificación del computador sencillo

## 3. Organización general

El sistema de la Figura 8-14 contiene:

- Una memoria de $256$ palabras de $8$ bits.
- Siete registros.
- Un decodificador de operaciones.
- Un decodificador de tiempo.

Como existen $256=2^8$ palabras, una dirección necesita ocho bits. Tanto los datos como las instrucciones ocupan palabras de ocho bits.

## 4. Registros del sistema

La Tabla 8-4 define los registros:

| Símbolo | Bits  | Nombre                           | Función                                           |
| :-----: | :---: | -------------------------------- | ------------------------------------------------- |
|  $MAR$  |   8   | Registro de dirección de memoria | Almacena la dirección aplicada a la memoria       |
|  $MBR$  |   8   | Registro separador de memoria    | Almacena la palabra leída o escrita               |
|   $A$   |   8   | Registro $A$                     | Registro procesador                               |
|   $R$   |   8   | Registro $R$                     | Registro procesador                               |
|  $PC$   |   8   | Contador de programa             | Almacena la dirección de la siguiente instrucción |
|  $IR$   |   8   | Registro de instrucción          | Almacena el código de operación actual            |
|   $T$   |   3   | Contador de tiempo               | Genera la secuencia de control                    |

### 4.1 Registros de memoria

$MAR$ contiene la dirección y $MBR$ contiene la palabra transferida. En este diseño, solamente estos dos registros se comunican directamente con la memoria.

### 4.2 Registros procesadores

$A$ y $R$ almacenan operandos o resultados. Las instrucciones consideradas cargan datos en $A$ desde $R$, desde una constante inmediata o desde memoria.

### 4.3 Registros de control

- $PC$ señala la siguiente palabra de instrucción.
- $IR$ conserva el opcode mientras se ejecuta la instrucción.
- $T$ produce los estados temporales $t_0,t_1,\ldots,t_7$.

El decodificador de $IR$ genera una salida $q_i$ para cada opcode reconocido. En el ejemplo:

$$
q_1=1\text{ para el opcode }1
$$

$$
q_2=1\text{ para el opcode }2
$$

$$
q_3=1\text{ para el opcode }3
$$

## 5. Conjunto de instrucciones

La Tabla 8-5 utiliza tres instrucciones:

|   Opcode   | Mnemónico  | Descripción                                    | Macrooperación        |
| :--------: | :--------: | ---------------------------------------------- | --------------------- |
| $00000001$ |  `MOV R`   | Mover $R$ hacia $A$                            | $A\leftarrow R$       |
| $00000010$ | `LDI OPRD` | Cargar un operando inmediato en $A$            | $A\leftarrow OPRD$    |
| $00000011$ | `LDA ADRS` | Cargar en $A$ el dato situado en una dirección | $A\leftarrow M[ADRS]$ |

### 5.1 Longitud de las instrucciones

- `MOV R` ocupa una palabra: el opcode implica las fuentes y el destino.
- `LDI OPRD` ocupa dos palabras: opcode y operando.
- `LDA ADRS` ocupa dos palabras: opcode y dirección.

Aunque `LDI` y `LDA` ocupan dos palabras, su ejecución no requiere la misma cantidad de lecturas: `LDA` debe usar la dirección obtenida para leer una tercera palabra, que contiene el dato.

# Ciclo común de envío

## 6. Búsqueda del código de operación

Antes de ejecutar cualquier instrucción, el computador debe obtener su opcode. El libro denomina a esta secuencia **ciclo de envío de instrucciones**.

| Tiempo | Microoperaciones                      | Propósito                                       |
| :----: | ------------------------------------- | ----------------------------------------------- |
| $t_0$  | $MAR\leftarrow PC$                    | Colocar la dirección de la instrucción en $MAR$ |
| $t_1$  | $MBR\leftarrow M,\ PC\leftarrow PC+1$ | Leer el opcode e incrementar $PC$               |
| $t_2$  | $IR\leftarrow MBR$                    | Transferir el opcode a $IR$                     |

La lectura $MBR\leftarrow M$ utiliza la dirección que ya está en $MAR$.

### 6.1 Razón del incremento de $PC$

En $t_1$, la memoria ya utiliza la dirección almacenada en $MAR$. Por eso $PC$ puede incrementarse sin alterar la lectura en curso. Al finalizar, señala la palabra siguiente al opcode.

### 6.2 Macrooperación equivalente

El efecto global puede expresarse como:

$$
IR\leftarrow M[PC],\quad PC\leftarrow PC+1
$$

pero el material impone tres pasos porque $PC$ e $IR$ no se comunican directamente con la memoria.

> **Ejemplo 1:** Seguimiento del ciclo de envío
> 
> Si inicialmente:
>
> $$
> PC=25,\qquad M[25]=00000010
> $$
>
> entonces:
>
> |Tiempo|Resultado|
> |:---:|---|
> |$t_0$|$MAR=25$|
> |$t_1$|$MBR=00000010$ y $PC=26$|
> |$t_2$|$IR=00000010$|
>
> El decodificador reconoce el opcode 2 y activa $q_2$, correspondiente a `LDI OPRD`.

# Ejecución de las instrucciones

## 7. Ejecución de `MOV R`

Después del ciclo común, $q_1=1$. La ejecución requiere un solo intervalo:

$$
q_1t_3:A\leftarrow R,\quad T\leftarrow0
$$

El contenido de $R$ se copia en $A$ y el contador de tiempo vuelve a cero para comenzar la búsqueda de la instrucción siguiente.

## 8. Ejecución de `LDI OPRD`

Después de decodificar el opcode 2:

| Control  | Microoperaciones                      | Propósito                         |
| :------: | ------------------------------------- | --------------------------------- |
| $q_2t_3$ | $MAR\leftarrow PC$                    | Direccionar el operando inmediato |
| $q_2t_4$ | $MBR\leftarrow M,\ PC\leftarrow PC+1$ | Leer el operando y avanzar $PC$   |
| $q_2t_5$ | $A\leftarrow MBR,\ T\leftarrow0$      | Cargar $A$ y finalizar            |

El operando se encuentra inmediatamente después del opcode. $PC$ se incrementa por segunda vez y queda preparado para la siguiente instrucción.

> **Ejemplo 2:** Carga inmediata
> 
> Si el opcode `LDI` está en la localización 35 y:
>
> $$
> M[36]=00101100_2=44_{10}
> $$
>
> después del ciclo de envío se tiene $PC=36$. La ejecución produce:
>
> $$
> MAR=36
> $$
>
> $$
> MBR=M[36]=44,\qquad PC=37
> $$
>
> $$
> A=44
> $$

## 9. Ejecución de `LDA ADRS`

Después de decodificar el opcode 3:

| Control  | Microoperaciones                      | Propósito                                  |
| :------: | ------------------------------------- | ------------------------------------------ |
| $q_3t_3$ | $MAR\leftarrow PC$                    | Direccionar la palabra que contiene $ADRS$ |
| $q_3t_4$ | $MBR\leftarrow M,\ PC\leftarrow PC+1$ | Leer $ADRS$ y avanzar $PC$                 |
| $q_3t_5$ | $MAR\leftarrow MBR$                   | Usar $ADRS$ como nueva dirección           |
| $q_3t_6$ | $MBR\leftarrow M$                     | Leer el operando                           |
| $q_3t_7$ | $A\leftarrow MBR,\ T\leftarrow0$      | Cargar $A$ y finalizar                     |

Esta instrucción realiza dos lecturas después de obtener el opcode:

1. Lee la dirección del operando.
2. Lee el operando almacenado en esa dirección.

> [!warning]
> $PC$ se incrementa al leer el opcode y al leer la palabra $ADRS$, porque ambas forman parte de la instrucción. No se incrementa al leer el operando desde $M[ADRS]$, ya que esa palabra es un dato y no la siguiente parte del programa.

> **Ejemplo 3:** Carga directa
> 
> Si el opcode `LDA` está en 45, la dirección está en 46 y:
>
> $$
> M[46]=70,\qquad M[70]=28
> $$
>
> el ciclo de envío deja $PC=46$. La ejecución realiza:
>
> $$
> MAR\leftarrow46
> $$
>
> $$
> MBR\leftarrow70,\quad PC\leftarrow47
> $$
>
> $$
> MAR\leftarrow70
> $$
>
> $$
> MBR\leftarrow28
> $$
>
> $$
> A\leftarrow28
> $$
>
> Al finalizar, $PC=47$, la dirección de la siguiente instrucción.

# De las secuencias a las funciones de control

## 10. Tabla completa de proposiciones

La Tabla 8-6 reúne el ciclo común y las tres ejecuciones:

| Etapa           | Control  | Microoperaciones                      |
| --------------- | :------: | ------------------------------------- |
| Enviar          |  $t_0$   | $MAR\leftarrow PC$                    |
| Enviar          |  $t_1$   | $MBR\leftarrow M,\ PC\leftarrow PC+1$ |
| Enviar          |  $t_2$   | $IR\leftarrow MBR$                    |
| Mover           | $q_1t_3$ | $A\leftarrow R,\ T\leftarrow0$        |
| Carga inmediata | $q_2t_3$ | $MAR\leftarrow PC$                    |
| Carga inmediata | $q_2t_4$ | $MBR\leftarrow M,\ PC\leftarrow PC+1$ |
| Carga inmediata | $q_2t_5$ | $A\leftarrow MBR,\ T\leftarrow0$      |
| Carga directa   | $q_3t_3$ | $MAR\leftarrow PC$                    |
| Carga directa   | $q_3t_4$ | $MBR\leftarrow M,\ PC\leftarrow PC+1$ |
| Carga directa   | $q_3t_5$ | $MAR\leftarrow MBR$                   |
| Carga directa   | $q_3t_6$ | $MBR\leftarrow M$                     |
| Carga directa   | $q_3t_7$ | $A\leftarrow MBR,\ T\leftarrow0$      |

Durante $t_0,t_1,t_2$, todavía no se necesita saber qué instrucción se ejecutará. A partir de $t_3$, el opcode almacenado en $IR$ selecciona una sola salida $q_i$ y, por tanto, una secuencia particular.

## 11. Agrupación de microoperaciones iguales

Una misma microoperación puede aparecer bajo varias condiciones. En vez de construir un camino distinto para cada línea, se combinan sus funciones de control con OR.

La transferencia:

$$
MAR\leftarrow PC
$$

aparece bajo $t_0$, $q_2t_3$ y $q_3t_3$. Su señal combinada es:

$$
x_1=t_0+q_2t_3+q_3t_3
$$

Factorizando:

$$
\boxed{x_1=t_0+(q_2+q_3)t_3}
$$

Cuando $x_1=1$, se selecciona $PC$ como fuente y se carga $MAR$.

> **Ejemplo 4:** Agrupar una operación repetida
> 
> La microoperación:
>
> $$
> PC\leftarrow PC+1
> $$
>
> aparece durante la lectura del opcode y durante la lectura de la segunda palabra de `LDI` o `LDA`:
>
> $$
> x_3=t_1+q_2t_4+q_3t_4
> $$
>
> Por tanto:
>
> $$
> \boxed{x_3=t_1+(q_2+q_3)t_4}
> $$
>
> Una sola entrada de incremento de $PC$ atiende las tres condiciones.

## 12. Funciones de control resultantes

La Tabla 8-7 presenta ocho operaciones distintas:

| Función de control      | Microoperación      |
| ----------------------- | ------------------- |
| $x_1=t_0+q_2t_3+q_3t_3$ | $MAR\leftarrow PC$  |
| $x_2=q_3t_5$            | $MAR\leftarrow MBR$ |
| $x_3=t_1+q_2t_4+q_3t_4$ | $PC\leftarrow PC+1$ |
| $x_4=x_3+q_3t_6$        | $MBR\leftarrow M$   |
| $x_5=q_2t_5+q_3t_7$     | $A\leftarrow MBR$   |
| $x_6=q_1t_3$            | $A\leftarrow R$     |
| $x_7=x_5+x_6$           | $T\leftarrow0$      |
| $x_8=t_2$               | $IR\leftarrow MBR$  |

### 12.1 Por qué $x_4=x_3+q_3t_6$

Cada condición incluida en $x_3$ corresponde a una lectura de memoria acompañada por un incremento de $PC$:

- Lectura del opcode en $t_1$.
- Lectura del operando inmediato en $q_2t_4$.
- Lectura de la dirección en $q_3t_4$.

La condición adicional $q_3t_6$ lee el operando directo, pero no incrementa $PC$. Por ello:

$$
x_4=x_3+q_3t_6
$$

### 12.2 Finalización de la instrucción

La secuencia termina cuando $A$ recibe el resultado:

$$
x_7=x_5+x_6
$$

- $x_6$ termina `MOV R`.
- $x_5$ termina `LDI OPRD` o `LDA ADRS`.

Al cumplirse $x_7$, se borra $T$ y el siguiente intervalo vuelve a ser $t_0$.

# Deducción del camino de datos

## 13. Entradas de $MAR$

$MAR$ recibe información desde dos fuentes:

$$
MAR\leftarrow PC
$$

$$
MAR\leftarrow MBR
$$

Por tanto, necesita:

- Un multiplexor de ocho bits entre $PC$ y $MBR$.
- Una señal de selección.
- Una entrada de carga activada por $x_1+x_2$.

En la configuración del libro, $x_1$ selecciona $PC$. Cuando $x_2=1$, se selecciona $MBR$ y se carga $MAR$.

## 14. Entradas de $A$

$A$ también posee dos fuentes:

$$
A\leftarrow MBR
$$

$$
A\leftarrow R
$$

Necesita otro multiplexor de ocho bits. Su carga se habilita mediante:

$$
L_A=x_5+x_6
$$

La misma expresión sirve para borrar $T$ porque cualquiera de esas transferencias completa una instrucción.

## 15. Conexiones restantes

- $PC$ necesita una entrada de incremento controlada por $x_3$.
- $IR$ recibe directamente de $MBR$ y carga con $x_8$.
- $MBR$ recibe la salida de la memoria y carga con $x_4$.
- La memoria recibe la dirección desde $MAR$ y lee cuando $x_4=1$.
- $T$ se incrementa mientras continúa la secuencia y se borra con $x_7$.
- El decodificador de $IR$ produce $q_1,q_2,q_3$.
- El decodificador de $T$ produce $t_0,t_1,\ldots,t_7$.

## 16. Unidad de control

El circuito combinacional de control recibe:

- Las salidas decodificadas del opcode: $q_1,q_2,q_3$.
- Las variables de tiempo: $t_0,t_1,\ldots,t_7$.

Y produce:

$$
x_1,x_2,\ldots,x_8
$$

Estas salidas controlan multiplexores, cargas, incrementos, borrados y lecturas de memoria. La unidad de control y el camino de datos cumplen funciones diferentes:

- El camino de datos transporta y transforma información.
- La unidad de control decide qué camino y qué destino se habilitan en cada intervalo.

## 17. Comprobación estructural

Cada proposición debe corresponder a material concreto:

| Proposición         | Material necesario                       |
| ------------------- | ---------------------------------------- |
| $MAR\leftarrow PC$  | Conexión $PC$-MUX-$MAR$ y carga de $MAR$ |
| $MAR\leftarrow MBR$ | Conexión $MBR$-MUX-$MAR$                 |
| $PC\leftarrow PC+1$ | Contador con incremento                  |
| $MBR\leftarrow M$   | Salida de memoria hacia $MBR$ y lectura  |
| $A\leftarrow MBR$   | Conexión $MBR$-MUX-$A$                   |
| $A\leftarrow R$     | Conexión $R$-MUX-$A$                     |
| $T\leftarrow0$      | Entrada de borrado de $T$                |
| $IR\leftarrow MBR$  | Conexión $MBR$-$IR$ y carga de $IR$      |

Si alguna proposición carece de un camino o señal correspondiente, el diseño está incompleto.

## 18. Procedimiento general de diseño con LTR

1. **Definir las operaciones externas.** Expresar cada instrucción como macrooperación.
2. **Elegir registros y memoria.** Determinar función y ancho de cada elemento.
3. **Establecer el ciclo común.** Definir la búsqueda y decodificación compartidas.
4. **Descomponer cada instrucción.** Escribir microoperaciones temporizadas y realizables.
5. **Construir la tabla de proposiciones.** Reunir todas las secuencias.
6. **Agrupar microoperaciones iguales.** Combinar sus condiciones mediante OR.
7. **Obtener las funciones de control.** Simplificarlas cuando sea posible.
8. **Deducir el camino de datos.** Añadir conexiones y multiplexores según las fuentes de cada destino.
9. **Diseñar la unidad de control.** Generar cargas, incrementos, lecturas y borrados.
10. **Integrar y comprobar.** Seguir cada instrucción pulso por pulso.

## 19. Criterios de verificación

- Todas las instrucciones pasan por el ciclo común.
- Cada microoperación posee una función de control.
- Cada transferencia dispone de un camino físico.
- Ningún multiplexor selecciona dos fuentes a la vez.
- $PC$ avanza únicamente al consumir palabras de instrucción.
- Las operaciones simultáneas no compiten por el mismo recurso.
- Toda secuencia termina borrando $T$.
- Al terminar, los registros contienen el resultado definido por la macrooperación.

## 20. Errores comunes

- Diseñar directamente el diagrama sin elaborar las secuencias LTR.
- Omitir el ciclo común de búsqueda.
- Confundir el operando inmediato con una dirección.
- Incrementar $PC$ al leer un dato indirectamente direccionado.
- Olvidar transferir $ADRS$ desde $MBR$ hacia $MAR$.
- Crear un circuito distinto para cada aparición de la misma microoperación.
- No añadir un multiplexor cuando un destino posee varias fuentes.
- Confundir una función $x_i$ con un dato del camino de datos.
- Terminar una instrucción sin borrar el contador de tiempo.

# Verificación del aprendizaje

Los problemas 1 y 2 utilizan directamente las Tablas 8-6 y 8-7. El problema 3 corresponde al ejercicio 8-31 del libro.

## Problema 1

El opcode `LDA ADRS` se encuentra en la localización 40. Además:

$$
M[41]=11001000_2=200_{10}
$$

$$
M[200]=00110111_2=55_{10}
$$

Si inicialmente $PC=40$, siga el contenido de $MAR$, $MBR$, $IR$, $PC$ y $A$ durante el ciclo de envío y la ejecución de la instrucción.

## Problema 2

A partir de la Tabla 8-6:

### a)

Obtenga y simplifique la función que controla:

$$
MAR\leftarrow PC
$$

### b)

Obtenga la función que controla:

$$
A\leftarrow MBR
$$

### c)

Indique las expresiones de carga de $MAR$ y de $A$ si cada registro posee dos fuentes.

## Problema 3

Se agrega al computador sencillo la instrucción inmediata del problema 8-31:

|   Opcode   | Mnemónico  | Función            |
| :--------: | :--------: | ------------------ |
| $00000100$ | `LRI OPRD` | $R\leftarrow OPRD$ |

Liste la secuencia completa de microoperaciones necesarias para buscar y ejecutar la instrucción. Utilice $q_4$ como salida del decodificador correspondiente al nuevo opcode.

> **Soluciones**
>
> **Problema 1**
>
> El ciclo de envío es común:
>
> |Control|Resultado relevante|
> |:---:|---|
> |$t_0$|$MAR\leftarrow40$|
> |$t_1$|$MBR\leftarrow M[40]=00000011$ y $PC\leftarrow41$|
> |$t_2$|$IR\leftarrow00000011$; se activa $q_3$|
>
> La ejecución de `LDA ADRS` continúa:
>
> |Control|Resultado relevante|
> |:---:|---|
> |$q_3t_3$|$MAR\leftarrow PC=41$|
> |$q_3t_4$|$MBR\leftarrow M[41]=11001000_2=200$ y $PC\leftarrow42$|
> |$q_3t_5$|$MAR\leftarrow MBR=200$|
> |$q_3t_6$|$MBR\leftarrow M[200]=00110111_2=55$|
> |$q_3t_7$|$A\leftarrow55$ y $T\leftarrow0$|
>
> Los valores finales principales son:
>
> $$
> \boxed{PC=42,\qquad A=55}
> $$
>
> $PC$ no se incrementa al leer $M[200]$ porque esa localización contiene el dato, no una palabra de la instrucción.
>
> **Problema 2**
>
> **a)** $MAR\leftarrow PC$ aparece bajo tres condiciones:
>
> $$
> x_1=t_0+q_2t_3+q_3t_3
> $$
>
> Factorizando $t_3$:
>
> $$
> \boxed{x_1=t_0+(q_2+q_3)t_3}
> $$
>
> **b)** $A\leftarrow MBR$ ocurre al terminar `LDI` o `LDA`:
>
> $$
> \boxed{x_5=q_2t_5+q_3t_7}
> $$
>
> **c)** $MAR$ recibe de $PC$ cuando $x_1=1$ y de $MBR$ cuando $x_2=1$:
>
> $$
> \boxed{L_{MAR}=x_1+x_2}
> $$
>
> $A$ recibe de $MBR$ cuando $x_5=1$ y de $R$ cuando $x_6=1$:
>
> $$
> \boxed{L_A=x_5+x_6}
> $$
>
> Además de las cargas, cada multiplexor debe usar una señal de selección coherente con la fuente activa.
>
> **Problema 3**
>
> Primero se ejecuta el ciclo de envío:
>
> $$
> t_0:MAR\leftarrow PC
> $$
>
> $$
> t_1:MBR\leftarrow M,\quad PC\leftarrow PC+1
> $$
>
> $$
> t_2:IR\leftarrow MBR
> $$
>
> El opcode $00000100$ activa $q_4$. Como `LRI OPRD` es inmediata, su ejecución es análoga a `LDI`, pero el destino es $R$:
>
> $$
> q_4t_3:MAR\leftarrow PC
> $$
>
> $$
> q_4t_4:MBR\leftarrow M,\quad PC\leftarrow PC+1
> $$
>
> $$
> q_4t_5:R\leftarrow MBR,\quad T\leftarrow0
> $$
>
> La nueva implementación necesita un camino desde $MBR$ hacia $R$ y una señal de carga:
>
> $$
> \boxed{L_R=q_4t_5}
> $$

<p align="center">
  <a href="./Tema%204.md">← Tema anterior</a> | <a href="./Tema%206.md">Siguiente tema →</a>
</p>
