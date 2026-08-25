---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - microoperaciones
  - microoperaciones-aritméticas
  - microoperaciones-lógicas
  - desplazamientos
  - registros
---

# Microoperaciones

## 1. De transferir información a transformarla

Una transferencia entre registros copia un patrón binario sin modificarlo:

$$
A\leftarrow B
$$

Las demás microoperaciones cambian la información mientras se transfiere hacia el registro de destino. Una **microoperación** es una operación elemental ejecutada sobre los datos almacenados en uno o más registros, normalmente durante un período de reloj.

El conjunto básico estudiado por el libro se organiza en tres clases:

| Clase          | Acción sobre la información                  |
| -------------- | -------------------------------------------- |
| Aritmética     | Realiza cálculos numéricos                   |
| Lógica         | Opera bit a bit mediante funciones booleanas |
| Desplazamiento | Cambia la posición de los bits               |

Estas operaciones sirven como bloques elementales para construir procedimientos más complejos. Por ejemplo, una multiplicación puede ejecutarse mediante una secuencia de sumas y desplazamientos.

> [!note]
> La expresión simbólica indica el resultado que debe recibir el destino. Su implementación exige registros, un circuito combinacional o funcional apropiado y señales de control que habiliten la carga.

## 2. Forma general

Una microoperación que transforma datos puede escribirse como:

$$
P:F\leftarrow f(A,B)
$$

donde:

- $P$ es la función de control.
- $A$ y $B$ son registros fuente.
- $f$ es la operación aplicada a sus contenidos.
- $F$ es el registro de destino.

Si $P=1$, el circuito calcula $f(A,B)$ y $F$ almacena el resultado en el borde activo del reloj. Los registros fuente conservan sus valores, salvo que alguno también sea el destino.

# Microoperaciones aritméticas

## 3. Suma

La proposición:

$$
F\leftarrow A+B
$$

establece que el contenido de $A$ se suma al contenido de $B$ y el resultado se transfiere a $F$.

Su implementación directa requiere:

- Los registros fuente $A$ y $B$.
- Un sumador paralelo de $n$ bits.
- El registro de destino $F$.
- Una señal de carga para $F$.

Para registros de $n$ bits, el resultado almacenado también posee $n$ bits. Un acarreo fuera de la posición más significativa debe conservarse por separado si el sistema lo necesita.

> **Ejemplo 1:** Suma de dos registros
> 
> Sean dos registros de cuatro bits:
>
> $$
> A=0110,\qquad B=0011
> $$
>
> Al ejecutar:
>
> $$
> F\leftarrow A+B
> $$
>
> se obtiene:
>
> $$
> \begin{array}{r}
> 0110\\
> +\,0011\\ \hline
> 1001
> \end{array}
> $$
>
> Por tanto:
>
> $$
> F=1001
> $$
>
> Los contenidos de $A$ y $B$ permanecen sin cambio.

## 4. Resta

La resta directa se representa mediante:

$$
F\leftarrow A-B
$$

Puede implementarse con un sustractor paralelo. Sin embargo, es común aprovechar un sumador y expresar la resta por medio del complemento de 2:

$$
A-B=A+\overline{B}+1
$$

Por tanto, la misma operación puede escribirse como:

$$
F\leftarrow A+\overline{B}+1
$$

El complemento de 2 de $B$ se obtiene complementando todos sus bits y agregando $1$.

> **Ejemplo 2:** Resta mediante complemento de 2
> 
> Para:
>
> $$
> A=0110,\qquad B=0011
> $$
>
> primero se forma el complemento de 2 de $B$:
>
> $$
> \overline{B}=1100
> $$
>
> $$
> \overline{B}+1=1101
> $$
>
> Luego:
>
> $$
> \begin{array}{r}
> 0110\\
> +\,1101\\ \hline
> 1\,0011
> \end{array}
> $$
>
> En cuatro bits se descarta el acarreo exterior y queda:
>
> $$
> \boxed{F=0011}
> $$
>
> que corresponde a $6-3=3$.

## 5. Complementos

### 5.1 Complemento de 1

La microoperación:

$$
B\leftarrow\overline{B}
$$

invierte cada bit del registro. Se implementa con un inversor por cada posición.

Si:

$$
B=00110110
$$

entonces:

$$
\overline{B}=11001001
$$

### 5.2 Complemento de 2

La proposición:

$$
B\leftarrow\overline{B}+1
$$

forma el complemento de 2 del contenido original de $B$. Esta operación es esencial para representar números negativos y efectuar restas con un sumador.

> [!warning]
> El complemento de 1 solo invierte los bits. El complemento de 2 invierte los bits y después suma uno. Confundirlos cambia el resultado de la resta.

## 6. Incremento y decremento

Incrementar significa sumar uno al contenido de un registro:

$$
A\leftarrow A+1
$$

Decrementar significa restar uno:

$$
A\leftarrow A-1
$$

Estas microoperaciones pueden implementarse mediante un contador con entradas de cuenta creciente y decreciente.

En un registro de $n$ bits, las operaciones se realizan módulo $2^n$. Por ejemplo, con cuatro bits:

$$
1111+1=0000
$$

El acarreo exterior se pierde si no existe un flip-flop destinado a conservarlo.

## 7. Conjunto de microoperaciones aritméticas

La Tabla 8-2 del libro resume las operaciones fundamentales:

| Designación simbólica          | Descripción                                                    |
| ------------------------------ | -------------------------------------------------------------- |
| $F\leftarrow A+B$              | Transferir a $F$ la suma de $A$ y $B$                          |
| $F\leftarrow A-B$              | Transferir a $F$ la diferencia $A-B$                           |
| $B\leftarrow\overline{B}$      | Formar el complemento de 1 de $B$                              |
| $B\leftarrow\overline{B}+1$    | Formar el complemento de 2 de $B$                              |
| $F\leftarrow A+\overline{B}+1$ | Transferir a $F$ la diferencia $A-B$ mediante complemento de 2 |
| $A\leftarrow A+1$              | Incrementar $A$                                                |
| $A\leftarrow A-1$              | Decrementar $A$                                                |

## 8. Relación con el material digital

El libro ilustra esta relación mediante las proposiciones:

$$
T_2:A\leftarrow A+B
$$

$$
T_5:A\leftarrow A+1
$$

El registro $A$ debe poseer capacidad de carga paralela y de incremento:

- Un sumador paralelo recibe los contenidos de $A$ y $B$.
- La salida del sumador se conecta a las entradas paralelas de $A$.
- $T_2$ habilita la carga de la suma.
- $T_5$ habilita el incremento del contador.

La descripción LTR y el circuito deben guardar una relación directa: cada fuente, operación, destino y condición debe tener un camino físico y una señal de control correspondientes.

## 9. Multiplicación y división

La multiplicación y la división son operaciones aritméticas válidas, pero el libro no las incluye en el conjunto básico de microoperaciones.

Solo pueden considerarse microoperaciones individuales cuando existen circuitos combinacionales que producen el resultado dentro de un período de reloj. En la mayoría de los computadores se ejecutan como secuencias:

- **Multiplicación:** sumas y desplazamientos.
- **División:** restas y desplazamientos.

Por ello, su descripción suele requerir varias proposiciones LTR y varios ciclos de reloj.

# Microoperaciones lógicas

## 10. Operaciones bit a bit

Las microoperaciones lógicas tratan cada posición de los registros como una variable binaria independiente. Si los registros tienen $n$ bits, se aplican $n$ operaciones booleanas en paralelo:

$$
F_i=f(A_i,B_i),\qquad i=1,2,\ldots,n
$$

No existe acarreo entre posiciones. El bit $F_i$ depende únicamente de los bits de la misma posición en las fuentes.

Con dos variables binarias existen dieciséis funciones lógicas posibles. El libro adopta símbolos particulares para las operaciones más utilizadas:

| Designación simbólica     | Microoperación      |
| ------------------------- | ------------------- |
| $A\leftarrow\overline{A}$ | Complemento lógico  |
| $F\leftarrow A\lor B$     | OR lógica           |
| $F\leftarrow A\land B$    | AND lógica          |
| $F\leftarrow A\oplus B$   | OR exclusiva lógica |

### 10.1 Complemento lógico

$$
A\leftarrow\overline{A}
$$

Cada $0$ se convierte en $1$ y cada $1$ en $0$. Es la misma operación binaria que el complemento de 1.

### 10.2 OR lógica

$$
F\leftarrow A\lor B
$$

Cada bit de $F$ vale $1$ si al menos uno de los bits fuente correspondientes es $1$.

### 10.3 AND lógica

$$
F\leftarrow A\land B
$$

Cada bit de $F$ vale $1$ solamente si ambos bits fuente correspondientes son $1$.

### 10.4 OR exclusiva lógica

$$
F\leftarrow A\oplus B
$$

Cada bit de $F$ vale $1$ cuando los bits fuente de esa posición son diferentes.

> **Ejemplo 3:** Operaciones lógicas en paralelo
> 
> El libro utiliza los registros:
>
> $$
> A=1010,\qquad B=1100
> $$
>
> Al operar posición por posición:
>
> |Operación|Resultado|
> |---|:---:|
> |$\overline{A}$|0101|
> |$A\lor B$|1110|
> |$A\land B$|1000|
> |$A\oplus B$|0110|
>
> El resultado de la OR exclusiva coincide con el ejemplo presentado en el libro:
>
> $$
> \boxed{F=0110}
> $$

## 11. Diferencia entre aritmética y lógica

Considérense:

$$
F\leftarrow A+B
$$

$$
G\leftarrow A\lor B
$$

La primera es una suma aritmética: puede producir acarreos de una posición hacia la siguiente. La segunda es una OR lógica: cada posición se resuelve de forma independiente.

Con:

$$
A=1010,\qquad B=1100
$$

se obtiene:

$$
A+B=1\,0110
$$

mientras que:

$$
A\lor B=1110
$$

Los símbolos y el contexto determinan qué operación debe realizarse.

## 12. El símbolo $+$ en diferentes contextos

El libro destaca que el símbolo $+$ puede tener dos significados. Considérese:

$$
T_1+T_2:A\leftarrow A+B,\quad C\leftarrow D\lor F
$$

| Aparición | Interpretación                           |
| --------- | ---------------------------------------- |
| $T_1+T_2$ | OR de Boole entre variables de control   |
| $A+B$     | Suma aritmética entre registros          |
| $D\lor F$ | Microoperación OR lógica entre registros |

La ubicación permite distinguirlos:

- Antes de los dos puntos, $+$ pertenece a una función de control.
- Después de la flecha, $+$ representa una suma aritmética.
- El símbolo $\lor$ identifica expresamente la OR lógica entre registros.

> [!warning]
> Una OR lógica no es una suma sin acarreo escrita con el mismo símbolo. La notación especial $\lor$ evita confundirla con el $+$ aritmético y con el $+$ booleano de una función de control.

## 13. Implementación de microoperaciones lógicas

Estas operaciones se realizan con grupos de compuertas en paralelo:

- El complemento de un registro de $n$ bits requiere $n$ inversores.
- La AND de dos registros requiere $n$ compuertas AND de dos entradas.
- La OR requiere $n$ compuertas OR.
- La OR exclusiva requiere $n$ compuertas XOR.

Cada compuerta recibe los bits de una misma posición. Sus salidas se conectan a las entradas correspondientes del registro de destino, cuya señal de carga determina cuándo se almacena el resultado.

# Microoperaciones de desplazamiento

## 14. Propósito del desplazamiento

Las microoperaciones de desplazamiento transfieren los bits de un registro hacia posiciones vecinas. Se utilizan para:

- Comunicación serial.
- Operaciones aritméticas.
- Procesamiento lógico.
- Multiplicaciones y divisiones por potencias de dos, cuando la representación lo permite.

El libro adopta los símbolos:

| Símbolo | Significado                             |
| ------- | --------------------------------------- |
| $shl$   | Desplazamiento de un bit a la izquierda |
| $shr$   | Desplazamiento de un bit a la derecha   |

Por ejemplo:

$$
A\leftarrow shl\ A
$$

$$
B\leftarrow shr\ B
$$

El mismo registro aparece a ambos lados de la flecha porque su contenido se transforma y vuelve a almacenarse en él.

## 15. Desplazamiento a la izquierda

Supóngase que un registro se representa como:

$$
A=A_nA_{n-1}\ldots A_2A_1
$$

donde $A_n$ es el bit del extremo izquierdo y $A_1$ el del extremo derecho.

Al ejecutar:

$$
A\leftarrow shl\ A
$$

cada bit se mueve una posición hacia la izquierda:

$$
A_i\leftarrow A_{i-1},\qquad i=2,3,\ldots,n
$$

El valor anterior de $A_n$ sale del registro y debe especificarse qué valor entra en $A_1$.

### 15.1 Desplazamiento circular a la izquierda

El libro completa el desplazamiento con:

$$
A\leftarrow shl\ A,\quad A_1\leftarrow A_n
$$

El bit que sale por el extremo izquierdo se introduce nuevamente por el extremo derecho. Esto forma una rotación o desplazamiento circular.

## 16. Desplazamiento a la derecha

En:

$$
B\leftarrow shr\ B
$$

cada bit se mueve una posición hacia la derecha:

$$
B_i\leftarrow B_{i+1},\qquad i=1,2,\ldots,n-1
$$

El bit $B_1$ sale del registro y la entrada serial debe proporcionar el nuevo valor de $B_n$.

Si un registro de un bit $E$ suministra la entrada serial:

$$
B\leftarrow shr\ B,\quad B_n\leftarrow E
$$

> [!note]
> En el ejemplo impreso de esta sección aparece $A_n\leftarrow E$ aunque el registro desplazado es $B$. Para que la operación sea coherente, la entrada del extremo izquierdo debe interpretarse como $B_n\leftarrow E$.

## 17. La entrada serial debe especificarse

Los símbolos $shl$ y $shr$ indican la dirección del desplazamiento, pero no establecen por sí solos qué entra en el flip-flop libre. Según la operación, la entrada puede ser:

- Un $0$.
- Un $1$.
- El bit que salió del extremo opuesto.
- El bit de signo.
- La salida de otro registro.

Por eso una descripción completa debe acompañar el desplazamiento con la microoperación que fija el bit de entrada.

> **Ejemplo 4:** Desplazamientos con diferentes entradas
> 
> Sea:
>
> $$
> A=10110010
> $$
>
> Un desplazamiento a la izquierda con entrada $0$ produce:
>
> $$
> A\leftarrow shl\ A,\quad A_1\leftarrow0
> $$
>
> $$
> \boxed{A=01100100}
> $$
>
> Si el desplazamiento es circular, el $1$ que salió del extremo izquierdo vuelve a entrar por la derecha:
>
> $$
> A\leftarrow shl\ A,\quad A_1\leftarrow A_8
> $$
>
> $$
> \boxed{A=01100101}
> $$
>
> En ambos casos se utiliza el contenido anterior de $A_8$ para determinar el resultado.

## 18. Conjunto lógico y de desplazamiento

La Tabla 8-3 del libro reúne las designaciones:

| Designación simbólica     | Descripción                        |
| ------------------------- | ---------------------------------- |
| $A\leftarrow\overline{A}$ | Complementar todos los bits de $A$ |
| $F\leftarrow A\lor B$     | Microoperación OR lógica           |
| $F\leftarrow A\land B$    | Microoperación AND lógica          |
| $F\leftarrow A\oplus B$   | Microoperación OR exclusiva lógica |
| $A\leftarrow shl\ A$      | Desplazar $A$ a la izquierda       |
| $A\leftarrow shr\ A$      | Desplazar $A$ a la derecha         |

## 19. Comparación de las tres clases

| Característica           | Aritmética                     | Lógica                 | Desplazamiento                 |
| ------------------------ | ------------------------------ | ---------------------- | ------------------------------ |
| Unidad de interpretación | Palabra numérica               | Bits independientes    | Posiciones dentro del registro |
| Acarreo entre bits       | Puede existir                  | No existe              | No aplica                      |
| Material típico          | Sumador, sustractor o contador | Compuertas en paralelo | Registro de desplazamiento     |
| Ejemplo                  | $F\leftarrow A+B$              | $F\leftarrow A\land B$ | $A\leftarrow shl\ A$           |

## 20. Procedimiento para resolver una microoperación

1. **Identificar la condición de control.** La operación solo se ejecuta cuando la función situada antes de los dos puntos vale $1$.
2. **Reconocer el destino.** Es el registro situado a la izquierda de la flecha.
3. **Identificar las fuentes.** Son los registros o valores utilizados en el lado derecho.
4. **Clasificar la operación.** Determinar si es aritmética, lógica o de desplazamiento.
5. **Conservar el ancho.** Realizar el cálculo con el número de bits de los registros y tratar explícitamente cualquier acarreo exterior.
6. **Respetar el paralelismo.** En las operaciones lógicas, resolver cada columna sin propagar acarreo.
7. **Completar el desplazamiento.** Indicar qué bit sale y qué valor entra por el extremo libre.
8. **Actualizar el destino.** Las fuentes permanecen iguales, excepto cuando una de ellas también es el destino.

## 21. Errores comunes

- Confundir una transferencia con una transformación de datos.
- Interpretar $A+B$ como OR lógica cuando aparece dentro de una microoperación.
- Propagar acarreo durante una AND, OR o XOR lógica.
- Omitir el $+1$ al formar el complemento de 2.
- Olvidar el ancho del registro y el posible acarreo exterior.
- Suponer que multiplicación y división siempre se completan en un solo ciclo.
- Cambiar el sentido de $shl$ y $shr$.
- No especificar la entrada serial de un desplazamiento.
- Calcular secuencialmente microoperaciones separadas por comas, aunque ocurren simultáneamente.

# Verificación del aprendizaje

Los siguientes ejercicios corresponden a los problemas 8-10, 8-11 y 8-27 del capítulo 8 del libro.

## Problema 1

Muestre los componentes necesarios para implementar las siguientes microoperaciones lógicas. Suponga que todos los registros tienen $n$ bits:

### a)

$$
T_1:F\leftarrow A\land B
$$

### b)

$$
T_2:G\leftarrow C\lor D
$$

### c)

$$
T_3:E\leftarrow\overline{E}
$$

## Problema 2

Explique la diferencia entre las siguientes proposiciones:

$$
A+B:F\leftarrow C\lor D
$$

y

$$
C+D:F\leftarrow A+B
$$

Identifique la función de control y la microoperación en cada caso.

## Problema 3

Determine la microoperación lógica que borra selectivamente los bits del registro $A$ en las posiciones donde los bits correspondientes del registro $B$ son iguales a $1$.

Compruebe la operación con:

$$
A=11010110,\qquad B=00111100
$$

> **Soluciones**
>
> **Problema 1**
>
> **a)** Para $T_1:F\leftarrow A\land B$ se requieren:
>
> - $n$ compuertas AND de dos entradas.
> - Cada compuerta recibe $A_i$ y $B_i$.
> - Las $n$ salidas se conectan a las entradas paralelas de $F$.
> - $T_1$ se conecta a la habilitación de carga de $F$.
>
> Para cada posición:
>
> $$
> F_i=A_iB_i
> $$
>
> **b)** Para $T_2:G\leftarrow C\lor D$ se requieren:
>
> - $n$ compuertas OR de dos entradas.
> - Cada compuerta recibe $C_i$ y $D_i$.
> - Las salidas alimentan las entradas correspondientes de $G$.
> - $T_2$ habilita la carga de $G$.
>
> Para cada posición:
>
> $$
> G_i=C_i+D_i
> $$
>
> **c)** Para $T_3:E\leftarrow\overline{E}$ se requieren:
>
> - $n$ inversores conectados a las salidas de $E$.
> - Las salidas de los inversores conectadas a las entradas paralelas del mismo registro.
> - $T_3$ como señal de carga de $E$.
>
> En el borde activo, $E$ almacena el complemento de su contenido anterior.
>
> **Problema 2**
>
> En la primera proposición:
>
> $$
> A+B:F\leftarrow C\lor D
> $$
>
> - $A+B$ es una función de control booleana. La operación se habilita cuando $A=1$, $B=1$ o ambos valen $1$.
> - $C\lor D$ es una microoperación OR lógica bit a bit.
> - El resultado se almacena en $F$.
>
> En la segunda:
>
> $$
> C+D:F\leftarrow A+B
> $$
>
> - $C+D$ es la función de control booleana.
> - $A+B$ es una suma aritmética entre los contenidos de los registros $A$ y $B$.
> - La suma se almacena en $F$.
>
> La posición del símbolo $+$ determina su significado: antes de los dos puntos representa OR de control; después de la flecha representa suma aritmética.
>
> **Problema 3**
>
> Para conservar un bit de $A$ cuando $B_i=0$ y borrarlo cuando $B_i=1$, se necesita la función:
>
> $$
> A_i^{nuevo}=A_i\overline{B_i}
> $$
>
> La microoperación es:
>
> $$
> \boxed{A\leftarrow A\land\overline{B}}
> $$
>
> Con los valores dados:
>
> $$
> A=11010110
> $$
>
> $$
> B=00111100\quad\Rightarrow\quad\overline{B}=11000011
> $$
>
> Entonces:
>
> $$
> \begin{array}{r}
> 11010110\\
> \land\,11000011\\ \hline
> 11000010
> \end{array}
> $$
>
> Por tanto:
>
> $$
> \boxed{A=11000010}
> $$
>
> Las posiciones donde $B$ contenía $1$ quedaron en $0$; las demás conservaron el valor original de $A$.

<p align="center">
  <a href="./Tema%201.md">← Tema anterior</a> | <a href="./Tema%203.md">Siguiente tema →</a>
</p>
