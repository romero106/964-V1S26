---
Sección: A
Período: Vacaciones Primer Semestre 2026
Catedrático: Carlos Amilcar Lozano
Auxiliar: Carlos José Blanco Guzmán
Actualizado: 2026-08-24
Tags:
  - macrooperaciones
  - instrucciones
  - código-de-operación
  - programa-almacenado
  - transferencia-entre-registros
---

# Macrooperaciones

## 1. Organización de un sistema digital

La organización interna de un sistema digital queda determinada por:

- Los registros que utiliza.
- La secuencia de microoperaciones ejecutada sobre la información almacenada.

En un sistema de propósito especial, esa secuencia está fijada para realizar repetidamente una tarea determinada. En cambio, un computador de propósito general puede ejecutar distintas secuencias según las instrucciones que recibe.

| Sistema de propósito especial                      | Computador de propósito general                          |
| -------------------------------------------------- | -------------------------------------------------------- |
| La secuencia de operaciones está prefijada         | La secuencia depende de un programa                      |
| Se diseña para una tarea concreta                  | Puede ejecutar tareas diferentes                         |
| Cambiar la tarea puede exigir rediseñar el control | Cambiar la tarea puede requerir únicamente otro programa |

## 2. Programa y programa almacenado

Un **programa** es un conjunto ordenado de instrucciones que especifica:

- Las operaciones que deben realizarse.
- Los datos u operandos utilizados.
- El orden en que ocurre el procesamiento.

La tarea puede cambiar proporcionando instrucciones diferentes o utilizando las mismas instrucciones con datos distintos.

En un computador con **programa almacenado**, tanto las instrucciones como los datos se guardan en memoria. Para ejecutar una instrucción, el control debe:

1. Leerla de la memoria.
2. Transferirla a un registro de control o de instrucción.
3. Interpretar su código.
4. Emitir las funciones de control necesarias.
5. Ejecutar las microoperaciones correspondientes.

> [!note]
> Una instrucción almacenada también es una palabra binaria. Lo que la distingue de un dato es la forma en que el computador la interpreta durante el ciclo de instrucción.

## 3. Código de instrucción

Un **código de instrucción** es un grupo de bits que indica al computador cómo realizar una operación específica. Normalmente se divide en campos con significados diferentes.

La parte fundamental es el **código de operación** u **opcode**, que identifica acciones como:

- Sumar.
- Restar.
- Transferir.
- Complementar.
- Desplazar.

Los campos restantes pueden identificar operandos, registros o direcciones de memoria.

### 3.1 Cantidad de bits del código de operación

Si el computador debe distinguir $N$ operaciones, el número mínimo de bits $n$ satisface:

$$
2^n\geq N
$$

Por tanto:

$$
n=\left\lceil\log_2N\right\rceil
$$

> **Ejemplo 1:** Código para 32 operaciones
> 
> El libro considera un computador con 32 operaciones diferentes:
>
> $$
> 2^5=32
> $$
>
> Por ello, el código de operación necesita cinco bits. Una combinación como:
>
> $$
> 10010
> $$
>
> puede asignarse, por ejemplo, a la operación SUMA. La unidad de control reconoce ese patrón y produce las señales requeridas para ejecutar la operación.

> [!warning]
> Que existan $n$ bits no obliga a utilizar las $2^n$ combinaciones. Algunas pueden quedar sin asignar o reservarse para ampliaciones futuras.

## 4. Operandos y destinos

Además de indicar la operación, una instrucción debe permitir localizar los datos y, cuando sea necesario, el destino del resultado.

### 4.1 Especificación explícita

Un registro o una dirección se especifica **explícitamente** cuando la instrucción contiene bits destinados a identificarlo.

Por ejemplo, un campo de dirección dentro de la instrucción puede seleccionar una palabra particular de memoria.

### 4.2 Especificación implícita

Un registro se especifica **implícitamente** cuando forma parte de la definición del código de operación y no necesita un campo propio.

Si una instrucción significa siempre “complementar el acumulador $A$”, el destino $A$ puede estar implícito en el opcode.

La especificación implícita reduce la cantidad de bits de la instrucción, pero limita la libertad de elegir registros.

## 5. Formato de instrucción

El **formato** muestra cómo se distribuyen los bits de una instrucción. Cada campo posee una función particular.

El libro presenta tres posibilidades básicas.

### 5.1 Formato implícito

| Código de operación |
| :-----------------: |

El opcode determina la operación y también implica los registros utilizados. Puede representar acciones como:

$$
A\leftarrow R
$$

No aparece un operando ni una dirección adicional.

### 5.2 Operando inmediato

| Código de operación | Operando |
| :-----------------: | :------: |

El valor utilizado por la operación se encuentra inmediatamente después del opcode dentro de la instrucción o en la siguiente palabra, según el ancho disponible.

Una transferencia inmediata puede expresarse como:

$$
A\leftarrow\text{operando}
$$

El campo contiene el dato mismo, no la dirección donde buscarlo.

### 5.3 Dirección directa

| Código de operación | Dirección del operando |
| :-----------------: | :--------------------: |

El campo no contiene el dato, sino la dirección de memoria donde se encuentra:

$$
A\leftarrow M[\text{dirección}]
$$

La ejecución necesita una lectura adicional de memoria para obtener el operando.

## 6. Comparación de los formatos

| Pregunta                                            | Implícito                   | Inmediato             | Dirección directa     |
| --------------------------------------------------- | --------------------------- | --------------------- | --------------------- |
| ¿Dónde se identifica el registro?                   | En la definición del opcode | Puede estar implícito | Puede estar implícito |
| ¿Qué contiene el campo adicional?                   | No existe                   | El dato               | La dirección del dato |
| ¿Se necesita leer el operando en otra localización? | No                          | No                    | Sí                    |
| Ejemplo                                             | $A\leftarrow R$             | $A\leftarrow44$       | $A\leftarrow M[70]$   |

> **Ejemplo 2:** Dato inmediato y dirección directa
> 
> Considérense dos palabras adicionales con el mismo patrón binario:
>
> $$
> 00101100_2=44_{10}
> $$
>
> En una instrucción inmediata, ese patrón es el operando:
>
> $$
> A\leftarrow44
> $$
>
> En una instrucción directa, el mismo patrón se interpreta como una dirección:
>
> $$
> A\leftarrow M[44]
> $$
>
> El valor final de $A$ dependería entonces del contenido de la localización 44.

## 7. Representación de instrucciones en memoria

La Figura 8-13 del libro supone una memoria con palabras de ocho bits y códigos de operación de ocho bits. Por ello, las instrucciones que necesitan un operando o una dirección ocupan dos palabras.

| Localización | Contenido  | Interpretación                              |
| :----------: | :--------: | ------------------------------------------- |
|      25      | $00000001$ | Opcode 1: $A\leftarrow R$                   |
|      35      | $00000010$ | Opcode 2: $A\leftarrow\text{operando}$      |
|      36      | $00101100$ | Operando inmediato $44$                     |
|      45      | $00000011$ | Opcode 3: $A\leftarrow M[\text{dirección}]$ |
|      46      | $01000110$ | Dirección del operando: $70$                |
|      70      | $00011100$ | Operando almacenado: $28$                   |

### 7.1 Instrucción implícita

La palabra situada en la localización 25 contiene el opcode completo. Al ejecutarla:

$$
A\leftarrow R
$$

### 7.2 Instrucción inmediata

La localización 35 contiene el opcode y la 36 contiene el dato:

$$
A\leftarrow44
$$

### 7.3 Instrucción de dirección directa

La localización 45 contiene el opcode; la localización 46 contiene la dirección 70; finalmente, la localización 70 contiene el operando 28:

$$
A\leftarrow M[70]=28
$$

> [!warning]
> La localización de una instrucción, el campo de dirección y la localización del operando son conceptos diferentes. En el ejemplo directo, 45 y 46 almacenan la instrucción, 70 identifica dónde está el dato y 28 es el dato.

# Macrooperaciones y microoperaciones

## 8. Operación especificada por una instrucción

Una instrucción almacenada especifica una operación visible para el programador. La unidad de control obtiene la instrucción, interpreta su opcode y genera una secuencia de funciones de control para realizarla mediante los registros internos.

Se distinguen así tres niveles:

| Nivel          | Descripción                                           |
| -------------- | ----------------------------------------------------- |
| Instrucción    | Código binario almacenado que ordena una operación    |
| Macrooperación | Efecto global expresado mediante una proposición LTR  |
| Microoperación | Paso elemental ejecutado por los componentes internos |

## 9. Definición de macrooperación

Una **macrooperación** es una proposición que resume una operación cuya implementación requiere una secuencia de microoperaciones.

Por ejemplo:

$$
A\leftarrow M[\text{dirección}]
$$

expresa el efecto final deseado. Sin embargo, el computador debe buscar la instrucción, decodificarla, obtener la dirección, leer la memoria y cargar el registro $A$.

Una instrucción definida mediante notación de transferencia entre registros constituye normalmente una macrooperación, porque su ejecución completa incluye más de un paso interno.

## 10. Definición de microoperación

Una **microoperación** es una operación elemental limitada por los componentes disponibles en el sistema. Suele realizarse durante un período de reloj y activarse con una función de control.

Ejemplos:

$$
AR\leftarrow PC
$$

$$
PC\leftarrow PC+1
$$

$$
A\leftarrow MBR
$$

Cada una describe una acción directa sobre registros existentes, siempre que el camino de datos y el material requerido estén disponibles.

## 11. La notación no basta para clasificarlas

Una proposición aislada como:

$$
A\leftarrow R
$$

puede representar una microoperación o una macrooperación según el contexto.

- Si una señal de control conecta directamente $R$ con $A$ y habilita la carga, es una microoperación.
- Si la proposición define una instrucción almacenada, su ejecución incluye búsqueda y decodificación antes de la transferencia; entonces es una macrooperación.

La clasificación depende de:

- Los componentes internos disponibles.
- Los caminos de datos.
- El número de funciones de control necesarias.
- El nivel de descripción adoptado.

> [!note]
> La edición consultada contiene una inconsistencia en este punto: después de definir que una secuencia de dos o más funciones de control constituye una macrooperación, repite por error la palabra “microoperación”. La definición, el ejemplo y el resto del apartado confirman que allí corresponde **macrooperación**.

## 12. Descomposición de una macrooperación inmediata

El libro representa una instrucción inmediata como:

$$
A\leftarrow\text{operando}
$$

En la Figura 8-13, el opcode está en la localización 35 y el operando en la 36. La ejecución requiere conceptualmente:

1. Leer de la memoria el código de operación almacenado en 35.
2. Transferirlo al registro de control.
3. Decodificarlo y reconocer que se trata de una instrucción inmediata.
4. Leer el operando de la localización 36.
5. Transferir el operando al registro $A$.

El último paso produce el efecto visible, pero los anteriores son indispensables para saber qué operación ejecutar y dónde encontrar el dato.

> **Ejemplo 3:** Del efecto global a los pasos internos
> 
> La macrooperación:
>
> $$
> A\leftarrow44
> $$
>
> no afirma que el valor 44 esté permanentemente conectado a $A$. En el ejemplo del libro:
>
> - El control obtiene el opcode 2 de la localización 35.
> - Lo decodifica como transferencia inmediata.
> - Obtiene $00101100_2$ de la localización 36.
> - Habilita la transferencia del dato hacia $A$.
>
> Solo al finalizar la secuencia se cumple:
>
> $$
> A=00101100_2=44_{10}
> $$

## 13. Descomposición de una macrooperación directa

La operación:

$$
A\leftarrow M[70]
$$

exige más que una transferencia simple:

1. Buscar y decodificar la instrucción.
2. Leer el campo de dirección, cuyo valor es 70.
3. Transferir la dirección al registro de direcciones de memoria.
4. Ejecutar una lectura en la localización 70.
5. Transferir el dato leído hacia $A$.

En el ejemplo del libro:

$$
M[70]=00011100_2=28_{10}
$$

por lo que el efecto final es:

$$
A\leftarrow28
$$

## 14. Una instrucción implícita también puede ser macrooperación

La proposición:

$$
A\leftarrow R
$$

parece una sola transferencia. Sin embargo, cuando representa la instrucción almacenada en la localización 25, el control debe:

1. Leer la instrucción.
2. Transferirla al registro de control.
3. Decodificar el opcode.
4. Emitir otra función de control para efectuar $A\leftarrow R$.

Por eso la instrucción completa es una macrooperación, aunque su fase de ejecución contenga una transferencia sencilla.

## 15. Macrooperación e independencia del material

Una macrooperación puede expresar el comportamiento deseado sin comprometerse todavía con una organización concreta:

$$
F\leftarrow A\times B
$$

El efecto es claro, pero su realización podría utilizar:

- Un multiplicador combinacional en un solo ciclo.
- Varias sumas y desplazamientos.
- Una unidad especializada con varios ciclos internos.

La misma operación externa puede descomponerse de maneras diferentes según el material, el costo y el rendimiento del sistema.

## 16. Cuatro usos de la LTR

El libro identifica cuatro niveles de uso del método de transferencia entre registros:

1. **Definir instrucciones** de manera concisa mediante macrooperaciones.
2. **Expresar operaciones deseadas** sin relacionarlas todavía con componentes específicos.
3. **Describir la organización interna** mediante funciones de control y microoperaciones.
4. **Diseñar el sistema digital** especificando componentes e interconexiones.

El proceso avanza desde una descripción externa hacia una implementación concreta:

$$
\text{Instrucción}
\longrightarrow
\text{Macrooperación}
\longrightarrow
\text{Secuencia de microoperaciones}
\longrightarrow
\text{Componentes y señales}
$$

## 17. Propiedades de una macrooperación

- Resume una secuencia interna.
- Expresa un efecto global sobre registros o memoria.
- Puede definir una instrucción de computador.
- No posee necesariamente una única implementación.
- Puede plantearse antes de escoger los componentes.
- Debe poder refinarse hasta obtener microoperaciones realizables.

## 18. Instrucción, opcode y macrooperación

Los conceptos se relacionan, pero no son equivalentes:

| Concepto         | Qué representa                                   |
| ---------------- | ------------------------------------------------ |
| Opcode           | Campo binario que identifica una operación       |
| Instrucción      | Opcode más los campos necesarios para ejecutarla |
| Macrooperación   | Descripción simbólica del efecto global          |
| Microoperaciones | Pasos internos elementales                       |

Por ejemplo, una instrucción directa puede contener:

| Opcode | Dirección |
| :----: | :-------: |
| CARGAR |    70     |

Su macrooperación es:

$$
A\leftarrow M[70]
$$

y su ejecución se descompone en varias microoperaciones de búsqueda, lectura y transferencia.

## 19. Procedimiento para analizar una macrooperación

1. **Identificar el efecto final.** Determinar qué registro o memoria cambia.
2. **Reconocer el formato.** Distinguir si los operandos son implícitos, inmediatos o se obtienen mediante una dirección.
3. **Separar instrucción y datos.** Localizar el opcode, los campos adicionales y el operando real.
4. **Incluir búsqueda y decodificación.** Una instrucción no comienza directamente con la transformación visible.
5. **Descomponer el efecto.** Enumerar lecturas, transferencias y operaciones elementales.
6. **Comprobar el material.** Cada microoperación debe ser realizable con los registros y caminos disponibles.
7. **Asignar funciones de control.** Establecer el orden temporal de los pasos.
8. **Verificar el resultado global.** La secuencia completa debe producir exactamente la macrooperación original.

## 20. Errores comunes

- Creer que toda proposición con una flecha es automáticamente una microoperación.
- Clasificar una operación solo por su apariencia y no por el material disponible.
- Confundir el código de operación con la instrucción completa.
- Interpretar un operando inmediato como si fuera una dirección.
- Confundir la localización de la instrucción con la localización del dato.
- Omitir las fases de búsqueda y decodificación.
- Suponer que una macrooperación posee una única descomposición posible.
- Pretender ejecutar una microoperación sin un camino de datos o componente que la realice.

# Verificación del aprendizaje

Los problemas 1 y 2 corresponden a los ejercicios 8-28 y 8-29 del libro. El problema 3 reconstruye el ejemplo de memoria de instrucciones de la Figura 8-13.

## Problema 1

Un computador posee palabras de memoria de 24 bits y un conjunto de 190 operaciones diferentes. Cada instrucción ocupa una palabra y contiene un código de operación y una dirección.

Determine:

### a)

¿Cuántos bits necesita el código de operación?

### b)

¿Cuántos bits quedan para la dirección?

### c)

¿Cuántas palabras puede contener la memoria direccionable?

### d)

¿Cuál es el mayor número binario positivo con signo que puede almacenarse en una palabra de 24 bits?

## Problema 2

Especifique un formato de instrucción para ejecutar:

$$
A\leftarrow M[\text{dirección}]+R
$$

donde $R$ puede ser cualquiera de ocho registros del procesador. Indique los campos mínimos y el número de bits requerido para seleccionar $R$.

## Problema 3

Considere la memoria de instrucciones del libro:

| Localización | Contenido  |
| :----------: | :--------: |
|      25      | $00000001$ |
|      35      | $00000010$ |
|      36      | $00101100$ |
|      45      | $00000011$ |
|      46      | $01000110$ |
|      70      | $00011100$ |

Los opcodes 1, 2 y 3 representan respectivamente:

$$
A\leftarrow R
$$

$$
A\leftarrow\text{operando inmediato}
$$

$$
A\leftarrow M[\text{dirección}]
$$

Para cada instrucción, indique cuántas palabras ocupa, de dónde se obtiene el operando y cuál es el efecto final.

> **Soluciones**
>
> **Problema 1**
>
> **a)** Se busca el menor $n$ que satisfaga:
>
> $$
> 2^n\geq190
> $$
>
> Como:
>
> $$
> 2^7=128<190\leq256=2^8
> $$
>
> se necesitan:
>
> $$
> \boxed{8\text{ bits de opcode}}
> $$
>
> **b)** La palabra posee 24 bits:
>
> $$
> 24-8=16
> $$
>
> Por tanto:
>
> $$
> \boxed{16\text{ bits de dirección}}
> $$
>
> **c)** Con 16 bits pueden identificarse:
>
> $$
> 2^{16}=65\,536
> $$
>
> palabras de memoria:
>
> $$
> \boxed{65\,536\text{ palabras}}
> $$
>
> **d)** Un número positivo con signo utiliza un bit de signo igual a $0$ y 23 bits de magnitud. El mayor patrón positivo es:
>
> $$
> \boxed{0\underbrace{11111111111111111111111}_{23\text{ bits}}}
> $$
>
> Su valor decimal es:
>
> $$
> 2^{23}-1=\boxed{8\,388\,607}
> $$
>
> **Problema 2**
>
> El registro $A$ es el destino implícito. La instrucción debe contener al menos:
>
> |Campo|Función|
> |---|---|
> |Código de operación|Identifica la suma de memoria con registro|
> |Selector de registro|Identifica uno de los ocho registros $R$|
> |Dirección|Localiza el operando en memoria|
>
> Para seleccionar ocho registros:
>
> $$
> 2^3=8
> $$
>
> se necesitan:
>
> $$
> \boxed{3\text{ bits para }R}
> $$
>
> El formato general es:
>
> |Opcode|Registro $R$|Dirección|
> |:---:|:---:|:---:|
> |$k$ bits|3 bits|$m$ bits|
>
> Los valores de $k$ y $m$ dependen del número de operaciones y del tamaño de la memoria. Durante la ejecución se lee $M[\text{dirección}]$, se suma su contenido al registro seleccionado y se almacena el resultado en $A$.
>
> **Problema 3**
>
> **Instrucción en la localización 25**
>
> - Ocupa una palabra.
> - Los registros están implícitos en el opcode 1.
> - Su efecto es:
>
> $$
> \boxed{A\leftarrow R}
> $$
>
> **Instrucción en las localizaciones 35 y 36**
>
> - Ocupa dos palabras.
> - La localización 35 contiene el opcode 2.
> - La localización 36 contiene directamente $00101100_2=44_{10}$.
> - Su efecto es:
>
> $$
> \boxed{A\leftarrow44}
> $$
>
> **Instrucción en las localizaciones 45 y 46**
>
> - Ocupa dos palabras.
> - La localización 45 contiene el opcode 3.
> - La localización 46 contiene $01000110_2=70_{10}$, que se interpreta como dirección.
> - La localización 70 contiene $00011100_2=28_{10}$, que es el operando.
> - Su efecto es:
>
> $$
> \boxed{A\leftarrow M[70]=28}
> $$

<p align="center">
  <a href="./Tema%202.md">← Tema anterior</a> | <a href="./Tema%204.md">Siguiente tema →</a>
</p>
