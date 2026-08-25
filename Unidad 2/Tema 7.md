---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - familias-lógicas
  - circuitos-integrados
  - rtl
  - dtl
  - ttl
  - ecl
  - mos
  - cmos
---

# Familias lógicas

## 1. De la función lógica al circuito electrónico

Una función de Boole indica la relación entre variables binarias. Una **familia lógica** indica cómo se realiza físicamente esa relación mediante transistores, diodos, resistencias, corrientes y voltajes.

Dos compuertas de familias diferentes pueden implementar la misma función NAND y tener la misma tabla de verdad, pero diferir en:

- Velocidad.
- Consumo de potencia.
- Capacidad para accionar otras entradas.
- Tolerancia al ruido.
- Densidad de integración.
- Niveles eléctricos.

El libro estudia las siguientes familias:

| Abreviatura | Nombre                                        |
| :---------: | --------------------------------------------- |
|     RTL     | Lógica de transistor y resistencia            |
|     DTL     | Lógica de transistores y diodos               |
|     I²L     | Lógica de inyección integrada                 |
|     TTL     | Lógica de transistor y transistor             |
|     ECL     | Lógica de emisor acoplado                     |
|     MOS     | Semiconductor de óxido de metal               |
|    CMOS     | Semiconductor de óxido de metal complementado |

> [!note]
> RTL y DTL se incluyen principalmente para comprender la evolución de las familias bipolares. Las cifras de velocidad y potencia presentadas por el libro describen circuitos integrados de su época y deben interpretarse como referencias históricas, no como especificaciones de componentes actuales.

## 2. Niveles lógicos

Los circuitos electrónicos no reciben números abstractos, sino voltajes. Se distinguen dos intervalos:

- **H:** nivel alto.
- **L:** nivel bajo.

En lógica positiva:

$$
H\longleftrightarrow1
$$

$$
L\longleftrightarrow0
$$

### 2.1 NAND en términos eléctricos

| Entrada $A$ | Entrada $B$ | Salida $Y=(AB)'$ |
| :---------: | :---------: | :--------------: |
|      L      |      L      |        H         |
|      L      |      H      |        H         |
|      H      |      L      |        H         |
|      H      |      H      |        L         |

Una entrada baja basta para producir una salida alta. La salida solo es baja si todas las entradas son altas.

### 2.2 NOR en términos eléctricos

| Entrada $A$ | Entrada $B$ | Salida $Y=(A+B)'$ |
| :---------: | :---------: | :---------------: |
|      L      |      L      |         H         |
|      L      |      H      |         L         |
|      H      |      L      |         L         |
|      H      |      H      |         L         |

Una entrada alta basta para producir una salida baja. La salida solo es alta si todas las entradas son bajas.

> [!warning]
> Un nivel lógico no es necesariamente un voltaje único. Normalmente es un intervalo permitido. Un voltaje fuera de los intervalos garantizados puede interpretarse de manera impredecible.

## 3. Características de comparación

El libro compara las familias mediante cuatro propiedades principales.

### 3.1 Capacidad de carga o *fan-out*

El **fan-out** es el número máximo de entradas normales que una salida puede accionar sin dejar de cumplir sus niveles válidos.

Si todas las cargas son iguales, el límite se obtiene comparando la corriente que puede suministrar o absorber la salida con la corriente requerida por cada entrada:

$$
\text{fan-out}\approx
\min\left(
\frac{|I_{OH}|}{|I_{IH}|},
\frac{I_{OL}}{I_{IL}}
\right)
$$

El resultado práctico debe redondearse hacia abajo.

> **Ejemplo**
> 
> Si una salida puede absorber $8\text{ mA}$ en nivel bajo y cada entrada consume $0.4\text{ mA}$:
>
> $$
> \text{fan-out bajo}=\frac{8}{0.4}=20
> $$
>
> Todavía debe comprobarse la condición de nivel alto; el menor de ambos resultados establece la capacidad real.

### 3.2 Disipación de potencia

La **disipación de potencia** es la potencia consumida por una compuerta y suministrada por la fuente:

$$
P\approx V_{CC}I_{promedio}
$$

Una potencia pequeña reduce el calentamiento y permite integrar más compuertas.

### 3.3 Retardo de propagación

El cambio de salida ocurre después del cambio de entrada:

- $t_{PLH}$: retardo de salida baja a alta.
- $t_{PHL}$: retardo de salida alta a baja.

El retardo medio es:

$$
t_{pd}=\frac{t_{PLH}+t_{PHL}}{2}
$$

Un retardo menor permite una frecuencia de operación mayor.

### 3.4 Margen de ruido

El **margen de ruido** es la perturbación de voltaje que puede añadirse sin provocar una interpretación lógica incorrecta.

Para niveles especificados:

$$
NM_H=V_{OH(min)}-V_{IH(min)}
$$

$$
NM_L=V_{IL(max)}-V_{OL(max)}
$$

El margen efectivo es el menor de los dos.

### 3.5 Producto velocidad-potencia

El compromiso entre rapidez y consumo puede expresarse mediante:

$$
\boxed{PDP=P_Dt_{pd}}
$$

Su unidad es el joule; suele expresarse en picojoules. Un valor menor representa un mejor compromiso entre potencia y retardo.

## 4. Transistor bipolar como interruptor

RTL, DTL, I²L, TTL y ECL utilizan transistores bipolares. Para comprender sus compuertas, el libro estudia tres regiones del transistor NPN.

| Región     | Condición aproximada                                 | Comportamiento digital                           |
| ---------- | ---------------------------------------------------- | ------------------------------------------------ |
| Corte      | $V_{BE}<0.6\text{ V}$                                | Interruptor abierto                              |
| Activa     | $V_{BE}\approx0.7\text{ V}$ y $I_C\approx h_{FE}I_B$ | Amplificación                                    |
| Saturación | La corriente de base excede la necesaria             | Interruptor cerrado, $V_{CE}\approx0.2\text{ V}$ |

La condición aproximada para asegurar saturación es:

$$
I_B>\frac{I_{CS}}{h_{FE}}
$$

donde $I_{CS}$ es la corriente máxima impuesta por el circuito del colector.

### 4.1 Inversor bipolar

En un inversor con resistencia de colector:

- Entrada baja: el transistor está en corte y la resistencia eleva la salida.
- Entrada alta: el transistor conduce hasta saturarse y lleva la salida a un nivel cercano a tierra.

Por tanto:

$$
Y=A'
$$

> [!note]
> En RTL, DTL y TTL se aprovechan principalmente corte y saturación. ECL evita deliberadamente la saturación para eliminar el tiempo necesario para abandonar esa región.

## 5. Lógica RTL

La compuerta básica RTL del libro es una NOR. Cada entrada se conecta a la base de un transistor mediante una resistencia y los colectores comparten la salida.

### 5.1 Operación

- Si todas las entradas son bajas, todos los transistores quedan en corte y la resistencia de colector produce una salida alta.
- Si cualquier entrada es alta, su transistor se satura y lleva la salida a nivel bajo.

$$
\boxed{Y=(A+B+C)'}
$$

La familia resulta sencilla, pero su capacidad de carga está limitada y el nivel alto disminuye al conectar más entradas.

El ejemplo del libro describe una compuerta RTL con capacidad de carga cercana a cinco, disipación aproximada de $12\text{ mW}$ y retardo de $25\text{ ns}$.

## 6. Lógica DTL

La compuerta básica DTL es una NAND. Los diodos realizan la decisión de entrada y un transistor invierte la señal resultante.

### 6.1 Operación

- Si cualquier entrada es baja, su diodo conduce, impide activar el transistor y la salida permanece alta.
- Si todas las entradas son altas, los diodos de entrada quedan polarizados inversamente, el transistor se satura y la salida baja.

$$
\boxed{Y=(ABC)'}
$$

Separar la función de entrada mediante diodos mejora la capacidad de carga respecto de RTL. En el ejemplo del libro, la compuerta DTL presenta aproximadamente ocho cargas, $12\text{ mW}$ y $30\text{ ns}$.

### 6.2 Lógica de umbral alto

Una variante DTL eleva el voltaje requerido para activar el transistor. Al aumentar la separación entre el nivel bajo y el umbral, mejora la inmunidad al ruido, aunque necesita una fuente de mayor voltaje.

## 7. Lógica de inyección integrada

La familia I²L se diseñó para obtener alta densidad y bajo consumo en funciones LSI.

Su celda básica utiliza:

- Una fuente de corriente de inyección.
- Un transistor inversor.
- Varios colectores para suministrar múltiples salidas.

Los colectores pueden conectarse a otras etapas para producir funciones lógicas mediante interconexiones compactas. Su principal ventaja es integrar muchas compuertas en un área reducida; la velocidad no es su característica dominante.

## 8. Lógica TTL

TTL es una evolución de DTL. Sustituye los diodos de entrada por un transistor de emisores múltiples y utiliza otros transistores para amplificar y controlar la salida.

La función básica continúa siendo NAND:

$$
Y=(AB)'
$$

### 8.1 Versiones TTL del libro

| Versión                       | Retardo (ns) | Potencia (mW) | Producto velocidad-potencia (pJ) |
| ----------------------------- | :----------: | :-----------: | :------------------------------: |
| TTL normalizada               |      10      |      10       |               100                |
| TTL de baja potencia          |      33      |       1       |                33                |
| TTL de alta velocidad         |      6       |      22       |               132                |
| TTL Schottky                  |      3       |      19       |                57                |
| TTL Schottky de baja potencia |     9.5      |       2       |                19                |

Estas variantes realizan las mismas funciones. Cambian los valores de las resistencias y el tipo de transistor para modificar el compromiso entre retardo y potencia.

### 8.2 TTL Schottky

Un transistor bipolar saturado almacena carga y necesita tiempo para apagarse. TTL Schottky coloca una unión Schottky entre base y colector para impedir la saturación profunda.

La caída de un diodo Schottky conductor es menor que la de una unión convencional. Al evitar la saturación, disminuye el tiempo de almacenamiento y mejora la velocidad.

## 9. Salidas TTL

El libro distingue tres configuraciones.

### 9.1 Colector abierto

El transistor de salida puede llevar la línea a nivel bajo, pero no producir por sí solo el nivel alto. Se necesita una resistencia externa hacia $V_{CC}$:

~~~text
VCC
 │
[R]
 │
 ├──── Y
 │
[transistor de salida]
 │
GND
~~~

Aplicaciones indicadas por el libro:

- Accionar cargas como lámparas o relevos.
- Formar lógica alambrada.
- Compartir una línea de bus.

### 9.2 Lógica alambrada

Varias salidas de colector abierto pueden unirse con una sola resistencia de elevación. El nodo común es alto únicamente si todos los transistores están en corte:

$$
Y=Y_1Y_2\cdots Y_n
$$

En lógica positiva se obtiene una AND alambrada.

> [!warning]
> La lógica alambrada solo es válida con configuraciones preparadas para compartir el nodo, como colector abierto. No deben unirse directamente salidas TTL de poste totémico.

### 9.3 Poste totémico

La salida de poste totémico emplea un transistor para bajar activamente la salida y otro para elevarla. Su menor resistencia de salida carga y descarga más rápido la capacitancia conectada.

Ventajas:

- Transiciones más rápidas.
- Menor impedancia de salida.

Limitación:

- Dos salidas no pueden conectarse entre sí; una podría intentar producir alto mientras la otra fuerza bajo, causando una corriente excesiva.

### 9.4 Tres estados

Una salida de tres estados puede adoptar:

- Nivel bajo.
- Nivel alto.
- Alta impedancia, $Z$.

| Habilitación | Dato  | Salida no inversora |
| :----------: | :---: | :-----------------: |
|      0       |  $X$  |         $Z$         |
|      1       |   0   |          0          |
|      1       |   1   |          1          |

El estado $Z$ desconecta eléctricamente al dispositivo del bus. Varias salidas pueden compartir una línea si solo una se habilita en cada instante.

> **Ejemplo**
> 
> Cuatro registros comparten un bus. Las cuatro salidas permanecen en $Z$ excepto la del registro seleccionado. Este puede transmitir $0$ o $1$ sin que los demás intenten imponer otro nivel.

## 10. Lógica ECL

ECL utiliza un amplificador diferencial y evita que los transistores entren en saturación. Al eliminar el tiempo de almacenamiento, obtiene un retardo muy pequeño.

La compuerta básica produce simultáneamente:

$$
Y_{OR}=A+B
$$

$$
Y_{NOR}=(A+B)'
$$

El circuito dirige una corriente casi constante hacia una de dos ramas según el voltaje de entrada.

### 10.1 Niveles y características del libro

- Nivel alto aproximado: $-0.8\text{ V}$.
- Nivel bajo aproximado: $-1.8\text{ V}$.
- Umbral aproximado: $-1.3\text{ V}$.
- Retardo aproximado: $2\text{ ns}$.
- Disipación aproximada: $25\text{ mW}$.
- Producto velocidad-potencia: $50\text{ pJ}$.

ECL favorece la velocidad a cambio de mayor consumo y niveles negativos de alimentación.

## 11. Tecnología MOS

MOS utiliza transistores de efecto de campo. A diferencia del BJT, cuyo funcionamiento depende de electrones y huecos, el MOSFET es un dispositivo unipolar controlado por el campo eléctrico de su puerta.

Puede construirse con:

- Canal $n$, cuyos portadores mayoritarios son electrones.
- Canal $p$, cuyos portadores mayoritarios son huecos.

También puede operar en:

- Modo de enriquecimiento.
- Modo de empobrecimiento.

La puerta está aislada del canal, por lo que la corriente de entrada es muy pequeña.

### 11.1 Compuertas MOS de canal $n$

Un transistor MOS puede utilizarse como dispositivo de conmutación o como carga.

- En el inversor, un MOS actúa como carga y otro como transistor activo.
- En una NAND, los transistores activos se colocan en serie.
- En una NOR, se colocan en paralelo.

Para la NAND, todos los transistores en serie deben conducir para llevar la salida a nivel bajo. Para la NOR, basta con que uno de los transistores en paralelo conduzca.

La tecnología MOS permite alta densidad de integración y se presta a la construcción de funciones LSI.

## 12. Tecnología CMOS

CMOS combina transistores MOS de canal $p$ y canal $n$ en redes complementarias.

### 12.1 Inversor CMOS

~~~text
VDD
 │
[ pMOS ]
 │
 ├──── Y
 │
[ nMOS ]
 │
GND

Las dos puertas reciben A.
~~~

- Si $A=0$, el pMOS conduce y el nMOS se corta: $Y=1$.
- Si $A=1$, el pMOS se corta y el nMOS conduce: $Y=0$.

$$
\boxed{Y=A'}
$$

En estado estable, uno de los transistores está cortado. Por ello, la corriente directa entre alimentación y tierra es extremadamente pequeña; el mayor consumo aparece durante las transiciones.

### 12.2 NAND CMOS

Una NAND de dos entradas utiliza:

- Dos pMOS en paralelo hacia $V_{DD}$.
- Dos nMOS en serie hacia tierra.

Solo cuando todas las entradas son altas existe un camino completo hacia tierra:

$$
\boxed{Y=(AB)'}
$$

### 12.3 NOR CMOS

Una NOR de dos entradas utiliza:

- Dos pMOS en serie hacia $V_{DD}$.
- Dos nMOS en paralelo hacia tierra.

Cualquier entrada alta crea un camino hacia tierra:

$$
\boxed{Y=(A+B)'}
$$

### 12.4 Características indicadas por el libro

- Disipación estática muy baja.
- Buena inmunidad al ruido.
- Alta densidad de integración.
- Amplio intervalo de alimentación.
- Retardo del inversor cercano a $25\text{ ns}$ en los dispositivos considerados.
- Margen de ruido cercano al $40\%$ de la alimentación.

> [!note]
> El bajo consumo estático no significa consumo nulo. Al cambiar de estado deben cargarse y descargarse capacitancias, por lo que la potencia aumenta con la frecuencia de conmutación.

## 13. Comparación general

| Familia | Dispositivo                | Ventaja destacada                                 | Limitación destacada                   |
| ------- | -------------------------- | ------------------------------------------------- | -------------------------------------- |
| RTL     | BJT y resistencias         | Simplicidad                                       | Bajo fan-out e inmunidad limitada      |
| DTL     | Diodos y BJT               | Mejor separación entre entradas y salida          | Mayor retardo                          |
| I²L     | BJT con inyección          | Alta densidad y bajo consumo                      | Velocidad moderada                     |
| TTL     | BJT                        | Buen equilibrio y variedad de funciones           | Consumo mayor que CMOS                 |
| ECL     | BJT no saturado            | Muy alta velocidad                                | Mayor potencia                         |
| MOS     | MOSFET de un tipo de canal | Alta densidad                                     | Carga activa y transiciones más lentas |
| CMOS    | MOSFET $p$ y $n$           | Muy baja potencia estática y buen margen de ruido | Consumo dinámico durante conmutación   |

No existe una familia universalmente mejor. La elección depende de:

- Frecuencia requerida.
- Potencia disponible.
- Número de cargas.
- Niveles eléctricos.
- Ruido esperado.
- Densidad y costo.

## 14. Compatibilidad entre bloques

Antes de conectar dos circuitos deben comprobarse:

1. Que $V_{OH}$ del emisor sea aceptado como alto por la entrada receptora.
2. Que $V_{OL}$ sea aceptado como bajo.
3. Que la salida pueda suministrar y absorber las corrientes necesarias.
4. Que no se exceda el fan-out.
5. Que el retardo sea compatible con la frecuencia del sistema.
6. Que las masas y alimentaciones tengan referencias adecuadas.
7. Que la estructura de salida permita la conexión deseada.

### 14.1 Producto correcto, conexión incorrecta

Dos compuertas pueden realizar funciones booleanas compatibles y aun así no ser eléctricamente compatibles. Una tabla de verdad no informa:

- Voltajes garantizados.
- Corrientes.
- Capacitancia.
- Retardo.
- Tipo de salida.

La hoja de datos completa esas condiciones.

## 15. Procedimiento para analizar una compuerta

1. Identifique la familia y su dispositivo básico.
2. Determine qué transistores conducen para cada combinación de entrada.
3. Busque un camino desde la salida hacia alimentación o tierra.
4. Deduzca si la salida es H, L o $Z$.
5. Compare el resultado con la tabla NAND, NOR o inversora esperada.
6. Revise fan-out, potencia, retardo y margen de ruido.
7. Identifique si la salida es colector abierto, poste totémico o triestado.
8. Compruebe la compatibilidad con la carga.

### Errores frecuentes

| Error                                                      | Corrección                                                 |
| ---------------------------------------------------------- | ---------------------------------------------------------- |
| Confundir función lógica y familia                         | La función dice qué hace; la familia, cómo se implementa.  |
| Tratar H y L como voltajes exactos                         | Son intervalos eléctricos permitidos.                      |
| Calcular fan-out con una sola condición                    | Compruebe nivel alto y nivel bajo.                         |
| Suponer que menor potencia siempre implica mayor velocidad | Existe un compromiso entre consumo y retardo.              |
| Ignorar la saturación del BJT                              | La saturación introduce tiempo de almacenamiento.          |
| Interpretar una salida abierta como un alto confiable      | Un colector abierto necesita una resistencia de elevación. |
| Unir salidas de poste totémico                             | Puede producir corrientes excesivas y daño.                |
| Confundir $Z$ con un nivel lógico                          | Alta impedancia significa desconexión, no 0 ni 1.          |
| Habilitar dos controladores de un bus                      | Solo una salida triestado debe conducir a la vez.          |
| Suponer que MOS y CMOS son idénticos                       | CMOS utiliza redes complementarias de canal $p$ y $n$.     |
| Comparar cifras históricas como si fueran actuales         | Interprételas dentro del contexto tecnológico del libro.   |

# Verificación del aprendizaje

**Problema 1:** resuelva el problema 13-6 del libro:

1. Demuestre mediante una tabla de verdad que dos salidas TTL de colector abierto, conectadas juntas a una resistencia externa y a $V_{CC}$, producen una función AND.
2. Demuestre que dos inversores de colector abierto conectados juntos producen una función NOR.

**Problema 2:** resuelva el problema 13-10 del libro. Calcule el margen de ruido de la compuerta ECL estudiada, cuyos niveles nominales son $V_H=-0.8\text{ V}$, $V_L=-1.8\text{ V}$ y cuyo umbral está alrededor de $-1.3\text{ V}$.

**Problema 3:** resuelva el problema 13-13 del libro:

1. Muestre la estructura de una compuerta NAND CMOS de cuatro entradas.
2. Repita el procedimiento para una compuerta NOR CMOS de cuatro entradas.

> **Soluciones**
>
> **Problema 1**
>
> Sean $Y_1$ y $Y_2$ los estados que intentan producir las dos salidas. El nodo alambrado queda alto únicamente si ambas salidas están inactivas o altas:
>
>|$Y_1$|$Y_2$|Nodo común $Y$|
>|:---:|:---:|:---:|
>|0|0|0|
>|0|1|0|
>|1|0|0|
>|1|1|1|
>
> Por tanto:
>
> $$
> \boxed{Y=Y_1Y_2}
> $$
>
> La unión es una AND alambrada.
>
> Si se conectan dos inversores de colector abierto:
>
> $$
> Y_1=A',\qquad Y_2=B'
> $$
>
> entonces:
>
> $$
> Y=Y_1Y_2=A'B'
> $$
>
> Aplicando De Morgan:
>
> $$
> \boxed{Y=(A+B)'}
> $$
>
> Por ello, la conexión de los dos inversores realiza una NOR.
>
> **Problema 2**
>
> La separación entre el nivel alto y el umbral es:
>
> $$
> NM_H=(-0.8)-(-1.3)=0.5\text{ V}
> $$
>
> La separación entre el umbral y el nivel bajo es:
>
> $$
> NM_L=(-1.3)-(-1.8)=0.5\text{ V}
> $$
>
> El margen efectivo es el menor:
>
> $$
> \boxed{NM=0.5\text{ V}}
> $$
>
> **Problema 3**
>
> Para una NAND CMOS de cuatro entradas:
>
> - La red de elevación contiene cuatro pMOS en paralelo entre $V_{DD}$ y $Y$.
> - La red de descenso contiene cuatro nMOS en serie entre $Y$ y tierra.
> - Cada entrada $A$, $B$, $C$ y $D$ controla un pMOS y un nMOS correspondientes.
>
> Todos los nMOS conducen simultáneamente únicamente con $A=B=C=D=1$, por lo que:
>
> $$
> \boxed{Y=(ABCD)'}
> $$
>
> Para una NOR CMOS de cuatro entradas:
>
> - La red de elevación contiene cuatro pMOS en serie.
> - La red de descenso contiene cuatro nMOS en paralelo.
>
> Cualquier entrada igual a $1$ activa un camino hacia tierra. La salida solo es alta si todas las entradas son cero:
>
> $$
> \boxed{Y=(A+B+C+D)'}
> $$

<p align="center">
  <a href="./Tema%206.md">← Tema anterior</a>
</p>
