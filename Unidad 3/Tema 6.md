---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - cpu
  - modelo-de-von-neumann
  - programa-almacenado
  - ciclo-de-instrucción
  - unidad-de-control
---

# Concepto básico de CPU con modelo de von Neumann

## 1. Computador de propósito general

La propiedad principal de un computador digital de propósito general es su capacidad para seguir un **programa**, es decir, una secuencia de instrucciones que opera sobre datos.

El usuario puede cambiar:

- Las instrucciones del programa.
- Los datos procesados.

De esta manera, el mismo material digital puede resolver tareas diferentes sin reconstruirse físicamente.

| Sistema de propósito especial                      | Computador de propósito general                      |
| -------------------------------------------------- | ---------------------------------------------------- |
| Ejecuta una secuencia prefijada                    | Ejecuta la secuencia indicada por un programa        |
| Su función principal está incorporada en el diseño | Su función cambia al modificar instrucciones y datos |
| Puede ser muy eficiente para una tarea             | Ofrece flexibilidad para distintas tareas            |

## 2. Bloques funcionales

El diagrama básico del libro divide el computador en:

1. **Unidad de memoria.** Almacena instrucciones, datos de entrada, resultados intermedios y resultados finales.
2. **Unidad procesadora.** Realiza operaciones aritméticas, lógicas, de desplazamiento y transferencia.
3. **Unidad de control.** Obtiene e interpreta instrucciones y dirige la secuencia de microoperaciones.
4. **Entrada.** Introduce programas y datos.
5. **Salida.** Presenta los resultados al exterior.

La memoria y la CPU intercambian palabras binarias; la entrada y la salida permiten comunicar el sistema con dispositivos externos.

## 3. Unidad procesadora y unidad de control

La **unidad procesadora** contiene registros y funciones digitales capaces de ejecutar microoperaciones. Puede incluir:

- Registros de propósito general.
- Acumuladores.
- Sumadores y unidades lógicas.
- Desplazadores.
- Buses y multiplexores.

La **unidad de control** supervisa el orden de las operaciones. Decide:

- Qué registro entrega información.
- Qué operación se realiza.
- Qué destino se carga.
- Cuándo se lee o escribe la memoria.
- Cuándo cambia la dirección de la próxima instrucción.

## 4. Definición de CPU

El libro define la unidad central de proceso como la combinación de:

$$
\boxed{\text{CPU}=\text{unidad procesadora}+\text{unidad de control}}
$$

La CPU no es el computador completo. La memoria principal y los dispositivos de entrada/salida son unidades externas a ella, aunque trabajan coordinadamente.

> [!note]
> Una CPU construida en una sola pastilla integrada se denomina microprocesador. Un microcomputador incorpora una CPU, memoria e interconexiones de entrada/salida.

# Programa almacenado y modelo de von Neumann

## 5. Concepto de programa almacenado

Las instrucciones se codifican como palabras binarias y se almacenan en memoria junto con los datos. Durante la ejecución, la unidad de control:

1. Lee una instrucción de memoria.
2. La coloca en un registro de instrucción.
3. Decodifica su código de operación.
4. Genera las señales de control.
5. Ejecuta las microoperaciones requeridas.
6. Continúa con la instrucción siguiente o con una dirección de bifurcación.

La memoria no distingue físicamente una instrucción de un dato. La interpretación depende del momento y del registro que recibe la palabra.

## 6. Correspondencia con el modelo de von Neumann

La expresión **modelo de von Neumann** no aparece literalmente en las secciones consultadas del libro. No obstante, el sistema descrito presenta sus propiedades básicas:

- Instrucciones y datos comparten la memoria.
- Las instrucciones se representan en código binario.
- Una CPU contiene procesamiento y control.
- El contador de programa establece normalmente una secuencia lineal.
- Las bifurcaciones pueden modificar esa secuencia.
- Cada instrucción se descompone en microoperaciones temporizadas.

El nombre utilizado por el programa del curso puede relacionarse con estas características sin atribuir al libro una denominación que no emplea expresamente.

## 7. Secuencia normal y bifurcaciones

El contador de programa, $PC$, almacena la dirección de la siguiente instrucción. Durante una búsqueda normal:

$$
PC\leftarrow PC+1
$$

Por ello, las instrucciones se ejecutan en direcciones consecutivas. Una bifurcación reemplaza el contenido de $PC$:

$$
PC\leftarrow\text{dirección de destino}
$$

El siguiente ciclo de búsqueda continúa entonces desde una parte distinta del programa.

# Computador elemental del libro

## 8. Configuración general

El capítulo 11 presenta un computador didáctico compuesto por:

- Una CPU.
- Una memoria principal.
- Una unidad de entrada/salida.

La memoria contiene:

$$
4096\text{ palabras}\times16\text{ bits}
$$

Como:

$$
4096=2^{12}
$$

se necesitan doce bits para seleccionar una palabra.

## 9. Registros y flip-flops

| Símbolo | Bits  | Función                               |
| :-----: | :---: | ------------------------------------- |
|   $A$   |  16   | Acumulador y registro procesador      |
|   $B$   |  16   | Registro separador de memoria         |
|  $PC$   |  12   | Dirección de la siguiente instrucción |
|  $MAR$  |  12   | Dirección aplicada a la memoria       |
|   $I$   |   4   | Código de operación actual            |
|   $E$   |   1   | Extensión del acumulador y acarreo    |
|   $F$   |   1   | Distingue búsqueda de ejecución       |
|   $S$   |   1   | Comienza o detiene el computador      |
|   $G$   |   2   | Genera la secuencia de tiempo         |
|   $N$   |   9   | Datos e indicador de entrada          |
|   $U$   |   9   | Datos e indicador de salida           |

### 9.1 $MAR$ y $B$

$MAR$ almacena la dirección de memoria. $B$ recibe o entrega una palabra completa de 16 bits.

Cuando se busca una instrucción, $MAR$ recibe de $PC$. Cuando se obtiene un operando, $MAR$ recibe los doce bits de dirección conservados en $B$.

### 9.2 Contador de programa

$PC$ contiene la dirección de la siguiente instrucción, no la instrucción misma. Normalmente se incrementa durante la lectura de la instrucción actual.

### 9.3 Acumulador y extensión

$A$ participa en la mayoría de las operaciones aritméticas y lógicas. $E$ conserva el acarreo de una suma y sirve como extensión durante desplazamientos.

### 9.4 Registro de instrucción

$I$ contiene solamente los cuatro bits del opcode. La dirección permanece en los doce bits inferiores de $B$:

$$
B=B(OP)\,B(AD)
$$

Separar el opcode es necesario porque una lectura posterior de memoria reemplazará el contenido de $B$ con el operando.

### 9.5 Control de ciclos

- $F=0$: ciclo de búsqueda; la palabra leída se interpreta como instrucción.
- $F=1$: ciclo de ejecución; la palabra leída se interpreta como operando.
- $S=1$: el computador funciona.
- $S=0$: el computador permanece detenido.

## 10. Entrada y salida

Los registros $N$ y $U$ contienen ocho bits de carácter y un bit indicador.

- El indicador de entrada vale $1$ cuando hay un carácter disponible.
- El indicador de salida vale $1$ cuando el dispositivo puede aceptar otro carácter.

Estos indicadores sincronizan la CPU, que es rápida, con dispositivos externos considerablemente más lentos.

> **Ejemplo 1:** Espera de un dispositivo
> 
> Si el indicador de entrada es $0$, la CPU todavía no debe leer $N$. Un programa puede comprobar repetidamente el indicador mediante una instrucción de omisión y una bifurcación hacia atrás.
>
> Cuando el indicador se vuelve $1$, se omite la bifurcación y se ejecuta la instrucción que transfiere el carácter hacia $A$.

# Datos e instrucciones

## 11. Interpretación de una palabra

Una palabra de 16 bits puede representar:

- Un número con signo en complemento de 2.
- Una palabra lógica cuyos bits se tratan de manera independiente.
- Dos caracteres de ocho bits.
- Una instrucción.

El patrón binario por sí solo no determina el significado; este depende de cómo se utilice.

## 12. Formatos de instrucción

Los cuatro bits más significativos forman inicialmente el campo de operación. Los doce bits restantes se interpretan según el tipo de instrucción.

### 12.1 Referencia de memoria

| Opcode: 4 bits | Dirección: 12 bits |
| :------------: | :----------------: |

La dirección selecciona una palabra de memoria utilizada como operando o destino.

### 12.2 Referencia de registro

| Código `0110` | Operación o prueba: 12 bits |
| :-----------: | :-------------------------: |

No se necesita un operando de memoria. Los doce bits inferiores seleccionan una operación sobre $A$, $E$, $PC$ o $S$.

### 12.3 Entrada/salida

| Código `0111` | Operación o prueba de E/S: 12 bits |
| :-----------: | :--------------------------------: |

Los bits inferiores seleccionan una operación o condición relacionada con los registros de entrada y salida.

Aunque existen solo cuatro bits de opcode, los códigos `0110` y `0111` extienden la decodificación mediante los otros doce bits. El computador del libro dispone de 22 instrucciones.

## 13. Instrucciones de referencia de memoria

En la siguiente tabla, $m$ representa la dirección y $M$ la palabra almacenada en esa dirección.

| Instrucción | Código | Función                                               |
| :---------: | :----: | ----------------------------------------------------- |
|   `AND m`   |  $0m$  | $A\leftarrow A\land M$                                |
|   `ADD m`   |  $1m$  | $A\leftarrow A+M,\ E\leftarrow\text{acarreo}$         |
|   `STO m`   |  $2m$  | $M\leftarrow A$                                       |
|   `ISZ m`   |  $3m$  | $M\leftarrow M+1$; si queda cero, $PC\leftarrow PC+1$ |
|   `BSB m`   |  $4m$  | Guardar retorno y bifurcar a la subrutina             |
|   `BUN m`   |  $5m$  | $PC\leftarrow m$                                      |

> [!note]
> La descripción verbal de `STO` en la Tabla 11-2 dice “almacenar en A”, pero la macrooperación $M\leftarrow A$ y la explicación posterior muestran que se almacena el contenido de $A$ **en memoria**.

### 13.1 Operaciones sobre datos

- `AND` aplica una operación lógica bit a bit.
- `ADD` suma el operando al acumulador y conserva el acarreo en $E$.
- `STO` escribe el acumulador en memoria.

Para cargar una palabra de memoria en $A$, el conjunto mínimo usa:

1. `CLA` para borrar $A$.
2. `ADD m` para sumar el contenido de memoria a cero.

### 13.2 Control de programa

- `ISZ` permite construir contadores y ciclos: incrementa una palabra y omite la instrucción siguiente cuando el resultado es cero.
- `BUN` sustituye incondicionalmente el contenido de $PC$.
- `BSB` guarda una dirección de retorno en memoria y comienza una subrutina.

## 14. Instrucciones de referencia de registro

| Instrucción | Función                                    |
| :---------: | ------------------------------------------ |
|    `CLA`    | $A\leftarrow0$                             |
|    `CLE`    | $E\leftarrow0$                             |
|    `CMA`    | $A\leftarrow\overline{A}$                  |
|    `CME`    | $E\leftarrow\overline{E}$                  |
|    `SHR`    | Desplazar a la derecha el conjunto $E,A$   |
|    `SHL`    | Desplazar a la izquierda el conjunto $E,A$ |
|    `INC`    | $A\leftarrow A+1$                          |
|    `SPA`    | Omitir si $A$ es positivo                  |
|    `SNA`    | Omitir si $A$ es negativo                  |
|    `SZA`    | Omitir si $A$ es cero                      |
|    `SZE`    | Omitir si $E$ es cero                      |
|    `HLT`    | $S\leftarrow0$                             |

Una omisión se implementa incrementando $PC$ una vez adicional. Como $PC$ ya avanzó durante la búsqueda, la próxima instrucción ejecutada estará dos posiciones después de la instrucción de prueba.

## 15. Instrucciones de entrada/salida

| Instrucción | Función                                                       |
| :---------: | ------------------------------------------------------------- |
|    `SKI`    | Omitir si el indicador de entrada vale $1$                    |
|    `INP`    | Transferir el carácter de $N$ hacia $A$ y borrar el indicador |
|    `SKO`    | Omitir si el indicador de salida vale $1$                     |
|    `OUT`    | Transferir el carácter de $A$ hacia $U$ y borrar el indicador |

Las instrucciones `SKI` y `SKO` permiten implementar espera activa: el programa comprueba el indicador hasta que el dispositivo esté listo.

> **Ejemplo 2:** Bucle de espera para entrada
> 
> La secuencia:
>
> ```text
> 1: SKI
> 2: BUN 1
> 3: INP
> ```
>
> funciona así:
>
> - Si el indicador es $0$, `SKI` no omite y `BUN 1` repite la prueba.
> - Si el indicador es $1$, `SKI` omite `BUN 1` y se ejecuta `INP`.

# Sincronización y control

## 16. Reloj maestro y variables de tiempo

Todos los registros reciben pulsos de un reloj común. El registro de secuencia $G$ se decodifica para producir:

$$
t_0,t_1,t_2,t_3
$$

Cada variable habilita las microoperaciones correspondientes a un intervalo. Después de $t_3$, la secuencia regresa a $t_0$.

> [!note]
> El texto menciona inicialmente un pulso cada milisegundo para una frecuencia de 1 MHz. Esto es incompatible: $1\text{ MHz}$ corresponde a un período de $1\ \mu s$. La Figura 11-5 y el apartado posterior utilizan correctamente microsegundos.

## 17. Entradas de la lógica de control

La red combinacional de control recibe:

- Las variables de tiempo $t_0$ a $t_3$.
- Las salidas $q_0$ a $q_7$ del decodificador del opcode.
- El estado del flip-flop $F$.
- Bits de condición como el signo, cero e indicadores de E/S.
- El estado de comienzo-parada $S$.

Sus salidas habilitan las microoperaciones de registros, memoria y entrada/salida.

## 18. Papel de $F$

El flip-flop $F$ resuelve una diferencia esencial:

|  $F$  | Ciclo     | Interpretación de la palabra leída |
| :---: | --------- | ---------------------------------- |
|   0   | Búsqueda  | Instrucción                        |
|   1   | Ejecución | Operando                           |

Al terminar una instrucción de memoria:

$$
F\leftarrow0
$$

El computador regresa al ciclo de búsqueda.

# Ciclo de instrucción

## 19. Ciclo de búsqueda

Mientras $F=0$, todas las instrucciones comienzan con:

$$
F't_0:MAR\leftarrow PC
$$

$$
F't_1:B\leftarrow M,\quad PC\leftarrow PC+1
$$

$$
F't_2:I\leftarrow B(OP)
$$

Al finalizar:

- $I$ contiene el opcode.
- $B(AD)$ conserva la dirección de doce bits.
- $PC$ señala la instrucción siguiente.

## 20. Decisión durante $t_3$

El opcode se decodifica en $q_0,q_1,\ldots,q_7$.

- `AND`, `ADD`, `STO`, `ISZ` y `BSB` necesitan un ciclo de ejecución; por ello se establece $F\leftarrow1$.
- `BUN` puede transferir $B(AD)$ directamente a $PC$ durante $t_3$.
- Las instrucciones de registro e I/O se ejecutan durante $t_3$ porque no necesitan otro acceso de memoria.

La transición para las instrucciones que requieren operando es:

$$
F'(q_0+q_1+q_2+q_3+q_4)t_3:F\leftarrow1
$$

## 21. Ciclo de ejecución

Para una instrucción de referencia de memoria:

$$
Ft_0:MAR\leftarrow B(AD)
$$

Las instrucciones `AND`, `ADD` e `ISZ` deben leer el operando:

$$
F(q_0+q_1+q_3)t_1:B\leftarrow M
$$

Después se ejecuta la operación particular durante $t_2$ o $t_3$. Al terminar:

$$
Ft_3:F\leftarrow0
$$

El siguiente intervalo $t_0$ inicia la búsqueda de la instrucción indicada por $PC$.

## 22. Ejecución de instrucciones de memoria

### 22.1 `AND`

Después de leer el operando en $B$:

$$
Fq_0t_3:A\leftarrow A\land B
$$

### 22.2 `ADD`

$$
Fq_1t_3:A\leftarrow A+B,\quad E\leftarrow\text{acarreo}
$$

### 22.3 `STO`

No necesita leer un operando. Primero prepara la palabra y después escribe:

$$
Fq_2t_2:B\leftarrow A
$$

$$
Fq_2t_3:M\leftarrow B
$$

### 22.4 `ISZ`

Después de leer la palabra:

$$
Fq_3t_2:B\leftarrow B+1
$$

$$
Fq_3t_3:M\leftarrow B
$$

Si el valor incrementado es cero:

$$
Fq_3B_zt_3:PC\leftarrow PC+1
$$

donde $B_z=1$ indica que todos los bits de $B$ son cero.

> **Ejemplo 3:** Ciclo completo de `ADD`
> 
> Supóngase:
>
> $$
> PC=100,\qquad A=5
> $$
>
> La palabra $M[100]$ contiene `ADD 250` y:
>
> $$
> M[250]=7
> $$
>
> **Búsqueda:**
>
> |Control|Resultado|
> |:---:|---|
> |$F't_0$|$MAR=100$|
> |$F't_1$|$B=M[100]$ y $PC=101$|
> |$F't_2$|$I=0001$ y $B(AD)=250$|
> |$F'q_1t_3$|$F=1$|
>
> **Ejecución:**
>
> |Control|Resultado|
> |:---:|---|
> |$Ft_0$|$MAR=250$|
> |$Fq_1t_1$|$B=M[250]=7$|
> |$Fq_1t_3$|$A=5+7=12$ y $E$ recibe el acarreo|
> |$Ft_3$|$F=0$|
>
> El siguiente ciclo comienza con $PC=101$.

## 23. Búsqueda y ejecución no son lo mismo

| Ciclo de búsqueda           | Ciclo de ejecución                    |
| --------------------------- | ------------------------------------- |
| Obtiene una instrucción     | Cumple la operación indicada          |
| Usa la dirección de $PC$    | Puede usar la dirección $B(AD)$       |
| Incrementa normalmente $PC$ | Puede modificar datos, memoria o $PC$ |
| Coloca el opcode en $I$     | Usa el opcode ya decodificado         |

No toda instrucción necesita un ciclo de ejecución separado. `BUN`, las instrucciones de registro y las de E/S terminan durante $t_3$ del ciclo de búsqueda.

# Propiedades del modelo estudiado

## 24. Separación funcional

- La memoria conserva palabras.
- La unidad procesadora transforma datos.
- La unidad de control genera secuencias.
- La entrada/salida comunica el computador con el exterior.

La cooperación entre bloques se realiza mediante transferencias entre registros.

## 25. Secuencialidad controlada

El orden normal surge del incremento de $PC$. Las instrucciones de bifurcación, omisión y subrutina permiten modificarlo para crear:

- Decisiones.
- Ciclos.
- Saltos.
- Llamadas a subrutinas.

## 26. Instrucciones como macrooperaciones

Una instrucción expresa un efecto global, pero la CPU lo refina en una secuencia de microoperaciones.

Por ejemplo:

$$
ADD\ m:\quad A\leftarrow A+M[m]
$$

requiere buscar la instrucción, extraer la dirección, leer el operando y efectuar la suma. La notación LTR conecta el nivel del programa con los registros y señales del material.

## 27. Procedimiento para seguir una instrucción

1. Anotar los valores iniciales de $PC$, registros y memoria relevante.
2. Ejecutar $t_0,t_1,t_2$ del ciclo de búsqueda.
3. Separar $B(OP)$ y $B(AD)$.
4. Determinar la salida $q_i$ activa.
5. Decidir si la instrucción termina en $t_3$ o establece $F=1$.
6. Si necesita operando, transferir $B(AD)$ a $MAR$ y leer memoria.
7. Ejecutar las microoperaciones específicas.
8. Actualizar indicadores, $PC$ y memoria cuando corresponda.
9. Confirmar que $F$ regrese a cero.

## 28. Errores comunes

- Confundir la CPU con el computador completo.
- Pensar que $PC$ contiene la instrucción en vez de su dirección.
- Suponer que $I$ almacena los 16 bits; solo conserva el opcode.
- Confundir $B(AD)$ con la palabra de memoria ubicada en esa dirección.
- Interpretar toda palabra leída como dato o toda palabra como instrucción.
- Omitir el incremento normal de $PC$ durante la búsqueda.
- Suponer que todas las instrucciones necesitan otro acceso de memoria.
- Olvidar que una bifurcación modifica la próxima dirección.
- Tratar las microoperaciones como si ocurrieran sin una secuencia temporal.

# Verificación del aprendizaje

Los problemas 1 y 2 se basan en los ejercicios 11-1 y 11-5 del libro. El problema 3 aplica las Tablas 11-5 a 11-7 al seguimiento de una instrucción.

## Problema 1

Clasifique las instrucciones del computador elemental que resultan útiles para:

### a)

Transferencias entre memoria y acumulador.

### b)

Transferencias entre entrada/salida y acumulador.

### c)

Operaciones aritméticas y lógicas.

### d)

Desplazamientos.

### e)

Decisiones de control basadas en condiciones de estado.

### f)

Bifurcación a una subrutina y regreso.

## Problema 2

Escriba dos secuencias de tres instrucciones:

1. En las localizaciones 1, 2 y 3, esperar un carácter de entrada y transferirlo al acumulador.
2. En las localizaciones 5, 6 y 7, esperar que el dispositivo de salida esté disponible y transferirle un carácter desde el acumulador.

## Problema 3

Suponga que:

$$
PC=20,\qquad A=0003_{16}
$$

La localización 20 contiene la instrucción `ADD 100` y:

$$
M[100]=0005_{16}
$$

Siga los ciclos de búsqueda y ejecución. Determine los valores finales de $PC$, $A$, $F$ y $E$, suponiendo que la suma no produce acarreo.

> **Soluciones**
>
> **Problema 1**
>
> **a) Memoria y acumulador**
>
> - Para cargar $M[m]$ en $A$: `CLA` seguida de `ADD m`.
> - Para almacenar $A$ en memoria: `STO m`.
> - `AND m` y `ADD m` también transfieren información desde memoria para operar con $A$.
>
> **b) Entrada/salida y acumulador**
>
> - `INP`: transfiere el carácter de entrada hacia $A$.
> - `OUT`: transfiere el carácter de $A$ hacia la salida.
>
> **c) Aritmética y lógica**
>
> - Aritmética: `ADD`, `INC` y las combinaciones con `CMA` para formar complementos y restas.
> - Lógica: `AND` y `CMA`; conjuntamente permiten derivar otras funciones lógicas.
>
> **d) Desplazamientos**
>
> - `SHR`: desplazamiento a la derecha.
> - `SHL`: desplazamiento a la izquierda.
>
> **e) Decisiones por estado**
>
> - `ISZ`, `SPA`, `SNA`, `SZA`, `SZE`, `SKI` y `SKO`.
> - Normalmente se combinan con `BUN` para continuar o cambiar el flujo.
>
> **f) Subrutina y regreso**
>
> - `BSB m` guarda la dirección de retorno y bifurca al inicio de la subrutina.
> - `BSB m` deja en la localización $m$ una instrucción `BUN` con la dirección de retorno y comienza la subrutina en $m+1$.
> - Una instrucción `BUN m` al final de la subrutina conduce a esa instrucción almacenada, cuya ejecución devuelve el control al programa llamador.
>
> **Problema 2**
>
> La secuencia de entrada es:
>
> ```text
> 1: SKI
> 2: BUN 1
> 3: INP
> ```
>
> Si el indicador está en cero, se ejecuta `BUN 1` y se repite la prueba. Cuando vale uno, `SKI` omite la bifurcación y permite ejecutar `INP`.
>
> La secuencia de salida es:
>
> ```text
> 5: SKO
> 6: BUN 5
> 7: OUT
> ```
>
> Mientras el dispositivo está ocupado, `BUN 5` repite la prueba. Cuando el indicador de salida vale uno, `SKO` omite la bifurcación y se ejecuta `OUT`.
>
> **Problema 3**
>
> **Búsqueda:**
>
> |Control|Resultado|
> |:---:|---|
> |$F't_0$|$MAR\leftarrow20$|
> |$F't_1$|$B\leftarrow M[20]=\text{ADD }100$ y $PC\leftarrow21$|
> |$F't_2$|$I\leftarrow0001$; $B(AD)=100$|
> |$F'q_1t_3$|$F\leftarrow1$|
>
> **Ejecución:**
>
> |Control|Resultado|
> |:---:|---|
> |$Ft_0$|$MAR\leftarrow100$|
> |$Fq_1t_1$|$B\leftarrow M[100]=0005_{16}$|
> |$Fq_1t_3$|$A\leftarrow0003_{16}+0005_{16}=0008_{16}$ y $E\leftarrow0$|
> |$Ft_3$|$F\leftarrow0$|
>
> Por tanto:
>
> $$
> \boxed{PC=21,\qquad A=0008_{16},\qquad F=0,\qquad E=0}
> $$

<p align="center">
  <a href="./Tema%205.md">← Tema anterior</a>
</p>
