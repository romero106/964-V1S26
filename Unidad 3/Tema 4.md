---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - herramientas-ltr
  - funciones-de-control
  - proposiciones-condicionales
  - buses
  - caminos-de-datos
---

# Herramientas utilizadas en LTR

## 1. Propósito de las herramientas de LTR

Un sistema digital grande no puede describirse de manera práctica enumerando cada compuerta, flip-flop y estado. La lógica de transferencia entre registros proporciona un nivel de descripción donde se trabaja directamente con registros, operaciones, caminos de datos y señales de control.

El libro no reúne estas herramientas bajo un único apartado con el mismo nombre utilizado por el programa. Sin embargo, las presenta a lo largo de las secciones dedicadas a la notación de transferencia, las interconexiones y las proposiciones condicionales de control.

Las herramientas pueden agruparse en tres categorías:

| Categoría   | Pregunta que permite responder               |
| ----------- | -------------------------------------------- |
| Descriptiva | ¿Qué información se transfiere o transforma? |
| Estructural | ¿Por qué camino físico circulan los datos?   |
| De control  | ¿Cuándo se ejecuta cada microoperación?      |

> [!note]
> Una proposición LTR no es únicamente una abreviatura matemática. Debe poder relacionarse con registros, circuitos combinacionales, interconexiones y señales de control reales.

## 2. Cuatro componentes del método

Toda descripción LTR debe permitir reconocer:

1. **Los registros y funciones digitales** que forman el sistema.
2. **La información binaria** almacenada en ellos.
3. **Las operaciones** realizadas sobre esa información.
4. **Las funciones de control** que determinan cuándo se ejecutan.

Estos componentes están relacionados. Una operación requiere fuentes y destino; el camino de datos debe hacerla posible; una función de control debe habilitarla en el momento correcto.

# Herramientas descriptivas

## 3. Nombres de registros y selección de partes

Los registros se representan mediante letras o abreviaturas:

- $A$, $B$, $C$: registros generales.
- $PC$: contador de programa.
- $AR$: registro de direcciones.
- $MBR$: registro separador de memoria.

Es posible seleccionar un bit individual:

$$
A_i
$$

o una parte del registro:

$$
PC(H),\qquad PC(L)
$$

Esta herramienta permite describir operaciones con palabras completas, campos o bits sin dibujar cada conexión.

## 4. Símbolos fundamentales

| Símbolo             | Uso                                                                        |
| ------------------- | -------------------------------------------------------------------------- |
| $A\leftarrow B$     | Transferencia desde $B$ hacia $A$                                          |
| $P:$                | Función que controla las operaciones situadas después de los dos puntos    |
| Coma                | Microoperaciones ejecutadas simultáneamente                                |
| $M[AR]$             | Palabra de memoria cuya dirección está almacenada en $AR$                  |
| Barra superior      | Complemento lógico o negación                                              |
| $+$                 | Suma aritmética dentro de una microoperación; OR en una función de control |
| $\land,\lor,\oplus$ | Microoperaciones lógicas AND, OR y XOR                                     |
| $shl,shr$           | Desplazamiento a la izquierda o a la derecha                               |

### 4.1 Dirección de la flecha

En:

$$
A\leftarrow B
$$

$A$ es el destino y $B$ es la fuente. La fuente conserva su contenido.

### 4.2 Función de control

En:

$$
x'T_1:A\leftarrow B
$$

la transferencia ocurre únicamente cuando:

$$
x'T_1=1
$$

es decir, cuando $x=0$ y $T_1=1$.

### 4.3 Simultaneidad

La expresión:

$$
T_2:A\leftarrow B,\quad B\leftarrow A
$$

describe dos cargas durante el mismo borde del reloj. Cada registro recibe el valor anterior del otro.

> **Ejemplo 1:** Lectura completa de una proposición
> 
> Considérese:
>
> $$
> xy'T_3:A\leftarrow A+B,\quad C\leftarrow A
> $$
>
> - La función de control es $xy'T_3$.
> - Deben cumplirse $x=1$, $y=0$ y $T_3=1$.
> - $A$ recibe la suma de los valores anteriores de $A$ y $B$.
> - $C$ recibe el valor anterior de $A$.
> - Las dos microoperaciones ocurren simultáneamente.
>
> Aunque $A$ cambia, $C$ no recibe la suma recién calculada; recibe el contenido que $A$ tenía antes del borde.

## 5. Proposiciones declaratorias

Además de ordenar transferencias, la notación puede declarar características del sistema, como el tamaño y la función de un registro. Estas declaraciones ayudan a interpretar expresiones posteriores y a determinar la cantidad de material requerido.

Por ejemplo, si se declara que $A$ es un registro de ocho bits, entonces:

$$
A\leftarrow B
$$

presupone una transferencia paralela de ocho líneas, siempre que $B$ posea el mismo ancho.

# Herramientas estructurales

## 6. Caminos de datos

Un **camino de datos** es la conexión por la cual viaja la información desde una fuente hasta un destino. Una proposición solo es realizable si existe un camino apropiado.

Las conexiones directas son simples, pero crecen rápidamente cuando aumentan las fuentes y los destinos. Para administrar la interconexión se utilizan multiplexores, buses y decodificadores.

## 7. Multiplexores

Un multiplexor selecciona una de varias fuentes para alimentar un destino. Si $A$ puede recibir datos desde $B$ o $C$:

$$
x:A\leftarrow B
$$

$$
x':A\leftarrow C
$$

se requiere un multiplexor por cada bit de $A$. La señal $x$ elige la fuente y la señal de carga determina si $A$ almacena el valor seleccionado.

> [!warning]
> Seleccionar una fuente y cargar un destino son acciones diferentes. El multiplexor establece qué valor llega a las entradas; la habilitación decide si el registro cambia.

## 8. Bus común

Un bus comparte un conjunto de líneas entre varios registros. Su organización requiere dos decisiones:

1. Seleccionar una fuente para colocarla en el bus.
2. Seleccionar un destino para cargar el contenido del bus.

En el sistema de cuatro registros del libro, dos bits seleccionan la fuente y otros dos seleccionan el destino:

| Código | Registro |
| :----: | :------: |
|   00   |   $A$    |
|   01   |   $B$    |
|   10   |   $C$    |
|   11   |   $D$    |

Para ejecutar:

$$
D\leftarrow B
$$

se selecciona $B$ como fuente y $D$ como destino. Si la habilitación del decodificador es activa en bajo:

$$
s_1s_0=01,\qquad d_1d_0=11,\qquad e=0
$$

## 9. Decodificadores

El decodificador convierte el código de destino en una sola señal de carga activa. Evita habilitar accidentalmente varios registros cuando solo uno debe recibir el dato.

En general:

$$
\text{código de destino}
\longrightarrow
\text{decodificador}
\longrightarrow
L_A,L_B,L_C,L_D
$$

La polaridad de la entrada de habilitación debe verificarse: puede ser activa en alto o activa en bajo.

## 10. Memoria

Las llaves cuadradas indican que el contenido de un registro se utiliza como dirección:

$$
R:B\leftarrow M[A]
$$

Cuando $R=1$, la palabra de memoria seleccionada por la dirección almacenada en $A$ se transfiere hacia $B$.

Una escritura se expresa como:

$$
W:M[A]\leftarrow B
$$

Aquí $A$ proporciona la dirección y $B$ proporciona el dato.

La implementación requiere seleccionar:

- La fuente de dirección.
- La fuente o destino de datos.
- La operación de lectura o escritura.

# Herramientas de control

## 11. Variables de control y de tiempo

Una función de control es una expresión booleana que habilita una o más microoperaciones. Puede depender de:

- Entradas externas.
- Bits de estado.
- Contenido de registros.
- Salidas de decodificadores.
- Variables de tiempo como $T_0,T_1,T_2,\ldots$

Una variable de tiempo identifica un intervalo dentro de una secuencia. Por ejemplo:

$$
T_1:AR\leftarrow PC
$$

$$
T_2:MBR\leftarrow M[AR],\quad PC\leftarrow PC+1
$$

indica que la transferencia hacia $AR$ ocurre primero y que las dos operaciones controladas por $T_2$ ocurren juntas en el intervalo siguiente.

## 12. De la función booleana a las señales físicas

Considérese:

$$
xy'T_0+T_1+x'yT_2:A\leftarrow A+B
$$

La función de carga de $A$ es:

$$
L_A=xy'T_0+T_1+x'yT_2
$$

Para implementarla se necesitan:

- Inversores para obtener $x'$ y $y'$.
- Compuertas AND para los términos $xy'T_0$ y $x'yT_2$.
- Una compuerta OR para reunir esos términos con $T_1$.
- Un sumador paralelo que calcule $A+B$.
- Un registro $A$ con carga paralela.

La función de control no calcula la suma; únicamente decide cuándo debe almacenarse.

> **Ejemplo 2:** Evaluación de una función de control
> 
> Para:
>
> $$
> P=xy'T_0+T_1+x'yT_2
> $$
>
> si:
>
> $$
> x=1,\quad y=0,\quad T_0=1,\quad T_1=0,\quad T_2=0
> $$
>
> entonces:
>
> $$
> P=(1)(1)(1)+0+(0)(0)(0)=1
> $$
>
> Por tanto, $A$ carga la suma. Si todos los términos valen $0$, $A$ conserva su contenido.

## 13. Proposiciones condicionales de control

Una función booleana es precisa, pero una decisión compleja puede resultar más clara mediante una proposición condicional. El libro adopta la forma:

$$
P:\ \text{si }(\text{condición})\ \text{entonces }[\text{microoperación(es)}]
$$

$$
\text{por tanto }[\text{microoperación(es)}]
$$

Su significado es:

- Primero debe cumplirse la función exterior $P$.
- Si la condición es verdadera, se ejecuta la rama **entonces**.
- Si la condición es falsa, se ejecuta la rama **por tanto**.
- Si la rama **por tanto** no aparece y la condición es falsa, no ocurre ninguna microoperación.

> [!note]
> En esta notación del libro, la expresión “por tanto” desempeña la función que normalmente se escribiría como “si no” o `else` en un lenguaje de programación.

## 14. Ejemplo con registros de un bit

El ejemplo del libro es:

$$
T_2:\ \text{si }(C=0)\ \text{entonces }(F\leftarrow1)\ \text{por tanto }(F\leftarrow0)
$$

Si $C$ y $F$ son registros de un bit, la proposición equivale a:

$$
C'T_2:F\leftarrow1
$$

$$
CT_2:F\leftarrow0
$$

Durante $T_2$, exactamente una de las funciones $C'T_2$ o $CT_2$ puede valer $1$.

> **Ejemplo 3:** Ejecución de las dos ramas
> 
> Si $T_2=1$ y $C=0$:
>
> $$
> C'T_2=1,\qquad CT_2=0
> $$
>
> se ejecuta:
>
> $$
> F\leftarrow1
> $$
>
> Si $T_2=1$ y $C=1$:
>
> $$
> C'T_2=0,\qquad CT_2=1
> $$
>
> se ejecuta:
>
> $$
> F\leftarrow0
> $$
>
> Si $T_2=0$, ninguna rama modifica $F$, independientemente del valor de $C$.

## 15. Condición de cero en un registro de varios bits

Si $C$ posee varios bits, la expresión $C=0$ significa que **todos** sus bits son cero.

Para un registro de cuatro bits:

$$
C=C_4C_3C_2C_1
$$

se define:

$$
x=C_1'C_2'C_3'C_4'
$$

Por De Morgan:

$$
x=(C_1+C_2+C_3+C_4)'
$$

Esta función puede implementarse con una compuerta NOR de cuatro entradas:

- $x=1$ si $C=0000$.
- $x=0$ si al menos un bit de $C$ es $1$.

La proposición condicional se convierte entonces en:

$$
xT_2:F\leftarrow1
$$

$$
x'T_2:F\leftarrow0
$$

> **Ejemplo 4:** Detección de cero
> 
> Si:
>
> $$
> C=0100
> $$
>
> entonces:
>
> $$
> x=(0+0+1+0)'=1'=0
> $$
>
> La condición $C=0$ es falsa. En cambio, para $C=0000$:
>
> $$
> x=(0+0+0+0)'=1
> $$
>
> y se selecciona la rama correspondiente a cero.

## 16. Condición y microoperación son partes distintas

En:

$$
P:\ \text{si }(C=0)\ \text{entonces }(A\leftarrow B)
$$

la condición $C=0$ pertenece a la función de control. La transferencia $A\leftarrow B$ es la microoperación.

La condición debe:

- Producir un valor binario.
- Poder evaluarse con la información disponible.
- Ser realizable mediante un circuito combinacional.
- Estar definida sin ambigüedad.

No basta escribir una condición comprensible para una persona; el sistema debe poder obtenerla físicamente.

## 17. Conversión de una proposición condicional

Para convertir:

$$
P:\ \text{si }Q\ \text{entonces }(X)\ \text{por tanto }(Y)
$$

a funciones de control convencionales:

$$
PQ:X
$$

$$
PQ':Y
$$

Si no existe rama alternativa:

$$
P:\ \text{si }Q\ \text{entonces }(X)
$$

solo se necesita:

$$
PQ:X
$$

## 18. Selección de la herramienta apropiada

| Necesidad                               | Herramienta adecuada                                          |
| --------------------------------------- | ------------------------------------------------------------- |
| Copiar entre dos registros              | Flecha de transferencia                                       |
| Ejecutar solo bajo una condición simple | Función booleana antes de los dos puntos                      |
| Ejecutar varias acciones a la vez       | Lista separada por comas                                      |
| Elegir una de varias fuentes            | Multiplexor                                                   |
| Compartir conexiones                    | Bus común                                                     |
| Seleccionar un destino                  | Decodificador                                                 |
| Acceder a memoria                       | Dirección entre llaves cuadradas y señal de lectura/escritura |
| Expresar una decisión con dos ramas     | Proposición condicional                                       |
| Detectar que un registro es cero        | NOR de todos sus bits                                         |

## 19. Procedimiento para traducir una especificación a LTR

1. **Enumerar los registros.** Indicar su función y ancho.
2. **Identificar fuentes y destinos.** Establecer de dónde proviene cada dato y dónde se almacena.
3. **Definir las operaciones.** Transferencia, aritmética, lógica, desplazamiento, lectura o escritura.
4. **Reconocer simultaneidad y dependencia.** Determinar qué acciones pueden ocurrir en el mismo pulso.
5. **Formular las condiciones.** Expresarlas como funciones booleanas o proposiciones condicionales.
6. **Elegir caminos de datos.** Decidir entre conexiones directas, multiplexores o buses.
7. **Generar señales de destino.** Utilizar carga directa o un decodificador.
8. **Asignar variables de tiempo.** Ordenar las microoperaciones dependientes.
9. **Comprobar realizabilidad.** Verificar que no se seleccionen dos fuentes incompatibles y que cada operación tenga material disponible.

## 20. Procedimiento para leer una proposición

1. Localizar los dos puntos y separar el control de las operaciones.
2. Evaluar la función o condición de control.
3. Identificar los destinos a la izquierda de cada flecha.
4. Identificar fuentes y operaciones a la derecha.
5. Revisar las comas para detectar simultaneidad.
6. Interpretar las llaves cuadradas como una selección de memoria.
7. Traducir la expresión a selecciones, habilitaciones y cargas físicas.
8. Determinar el contenido final de cada registro afectado.

## 21. Errores comunes

- Leer la flecha en sentido contrario.
- Creer que la fuente pierde el dato transferido.
- Confundir una función de control con información que debe almacenarse.
- Interpretar la coma como una secuencia temporal.
- Seleccionar una fuente sin habilitar el destino.
- Activar dos fuentes distintas sobre un mismo bus.
- Confundir el registro de dirección con el contenido de la memoria.
- Probar un solo bit cuando la condición indica que un registro completo es cero.
- Colocar la condición dentro de la microoperación en vez de usarla como control.
- Escribir una condición que no puede implementarse con la información y el material disponibles.

# Verificación del aprendizaje

Los siguientes ejercicios corresponden a los problemas 8-2, 8-3 y 8-13 del capítulo 8 del libro.

## Problema 1

Un valor constante puede transferirse a un registro conectando cada entrada a un nivel lógico fijo. Muestre la configuración necesaria para:

$$
T:A\leftarrow11010110
$$

Indique las conexiones de las entradas y la señal de carga.

## Problema 2

Un registro de ocho bits posee una entrada serial $x$. Sus celdas se numeran de derecha a izquierda, por lo que $A_1$ es la celda del extremo derecho y $A_8$ la del extremo izquierdo. Su operación es:

$$
P:A_8\leftarrow x,\quad A_i\leftarrow A_{i+1}\qquad i=1,2,\ldots,7
$$

Determine la función del registro y describa qué sucede con los bits durante un pulso controlado por $P$.

## Problema 3

Muestre los componentes que implementan:

$$
xy'T_0+T_1+x'yT_2:A\leftarrow A+B
$$

Incluya tanto el camino de datos como las compuertas necesarias para producir la función de control.

> **Soluciones**
>
> **Problema 1**
>
> El registro $A$ necesita ocho entradas paralelas. De $A_8$ a $A_1$ deben conectarse los niveles:
>
> |Entrada|$A_8$|$A_7$|$A_6$|$A_5$|$A_4$|$A_3$|$A_2$|$A_1$|
> |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
> |Nivel|1|1|0|1|0|1|1|0|
>
> Las entradas cuyo valor es $1$ se conectan al nivel lógico alto y las entradas cuyo valor es $0$ al nivel lógico bajo. La variable $T$ se conecta a la carga paralela de $A$.
>
> - Si $T=1$, en el borde activo se almacena $11010110$.
> - Si $T=0$, $A$ conserva su contenido.
>
> **Problema 2**
>
> Cada celda $A_i$ recibe el contenido anterior de la celda situada inmediatamente a su izquierda:
>
> $$
> A_1\leftarrow A_2,\quad A_2\leftarrow A_3,\quad\ldots,\quad A_7\leftarrow A_8
> $$
>
> El extremo izquierdo recibe:
>
> $$
> A_8\leftarrow x
> $$
>
> Por tanto, se trata de un:
>
> $$
> \boxed{\text{registro de desplazamiento a la derecha con entrada serial }x}
> $$
>
> El valor anterior de $A_1$ sale del registro. Todas las transferencias ocurren simultáneamente cuando $P=1$.
>
> **Problema 3**
>
> El camino de datos requiere:
>
> - Los registros fuente $A$ y $B$.
> - Un sumador paralelo de $n$ bits con entradas conectadas a $A$ y $B$.
> - Las salidas del sumador conectadas a las entradas paralelas de $A$.
> - Un registro $A$ con capacidad de carga paralela.
>
> La señal de carga debe ser:
>
> $$
> L_A=xy'T_0+T_1+x'yT_2
> $$
>
> La lógica de control puede construirse con:
>
> - Dos inversores para producir $x'$ y $y'$.
> - Una compuerta AND de tres entradas para $xy'T_0$.
> - Una compuerta AND de tres entradas para $x'yT_2$.
> - Una compuerta OR de tres entradas para combinar ambos productos con $T_1$.
>
> Cuando cualquiera de los tres términos vale $1$, el registro $A$ carga:
>
> $$
> \boxed{A_{nuevo}=A_{anterior}+B_{anterior}}
> $$
>
> Si la función completa vale $0$, $A$ no se carga y conserva su contenido.

<p align="center">
  <a href="./Tema%203.md">← Tema anterior</a> | <a href="./Tema%205.md">Siguiente tema →</a>
</p>
