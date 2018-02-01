# Apuntadores, arreglos y memoria dinamica

> **Objetivos**
> * Conocer y utilizar los apuntadores para el uso eficiente de la memoria.
> * Presentar el uso de las funciones y establecer cómo se realiza el paso de parámetros a funciones.
> * Conocer y aplicar el concepto de arreglos de una y más dimensiones en la resolución de problemas mediante algoritmos.
> * Conocer cómo es posible asignar de forma eficiente espacio en memoria.

## 1. Conceptos previos - ¿Que sucede cuando se declara una variable?
Cuando una variable se declara esta pasa a ocupar un lugar de memoria cuyo tamaño dependerá del número de bytes asociados al tipo de dato con el cual esta se declara. Suponiendo que se tienen las siguientes instrucciones en C:

```C
int i;
i = 35;
```

La siguiente figura ilustra su representacion en memoria:

![var_mem](https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/var_memoria.png)

**Figura 1**. Representación de una variable en memoria.

Desde el punto de vista del mapa de memoria y suponiendo que una variable tipo int ocupa 4 bytes tenemos el siguiente resultado por instruccion:

<table>
<tbody>
<tr>
<td>&nbsp;Instruccion</td>
<td>Representacion en&nbsp;memoria&nbsp;</td>
</tr>
<tr>
<td>int i;</td>
<td><img src="https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/rep_mapa_inst1.png" alt="var_mm1"></td>
</tr>
<tr>
<td>i = 35;</td>
<td><img src="https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/rep_mapa_inst2.png" alt="var_mm2"></td>
</tr>
</tbody>
</table>

Como se puede ver en la figura anterior, lo que se modifica cuando se hace manipulacion sobre variables es el contenido almacenado en un lugar especifico de memoria. Entender esto es de vital importancia para manejar el proximo tema.

## 2. Entrando en materia - Algunos aspectos sobre los apuntadores

### 2.1. ¿Que es un apuntador?

Un apuntador es una variable que almacena una **dirección de memoria y no un valor** como ocurre en el caso de las variables normales. La siguiente tabla resalta este hecho:

<table>
    <tr>
        <td colspan="2">Instrucciones</td>
    </tr>
    <tr>
        <td><b>int *p;<br>
            p = 1000;</b><br><br>
            <b>Nota</b>: Supóngase que <b>el apuntador</b> se guarda en la posición 500.
        </td>
        <td>
            <img src="https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/rep_mapa_ptr_inst1.png">
        </td>
    </tr>
    <tr>
        <td colspan="2">Mapa de memoria</td>
    </tr>
    <tr>
        <td>
          <b>int p;<br>
            p = 1000;</b><br><br>
            <b>Nota</b>: Supóngase que la <b>variable p</b> se guarda en posición 500.
          </td>
        <td>
            <img src="https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/rep_mapa_var_inst1_comp.png"> 
         </td>
    </tr>
</table>

Como se puede notar en la figura anterior, cuando el valor almacenado en el apuntador hará referencia a la dirección 1000 y no al valor de 1000, esto nos permitirá acceder a dicho lugar de memoria desde el apuntador. Más tarde veremos cómo. Así mismo, como un apuntador guarda una dirección de memoria y teniendo en cuenta que para el ejemplo se supone una arquitectura en la cual se manejan 32 bits ( equivalentes a 4 bytes), esto hará que una variable tipo apuntador sin importar el tipo de dato al que apunte tenga un tamaño de 4 bytes. (Este tamaño se define por la arquitectura. Por ejemplo si la maquina es de 64 bits entonces el tamaño ocupado por una variable tipo apuntador será de 8 bytes).

### 2.2. ¿Como se declara un apuntador?

Un apuntador se declara de la siguiente manera (donde las cosas que se encuentran entre corchetes son opcionales):

```C
tipo *[modificadores_del_tipo] nombre [=valor inicial];
```
Dónde:
* **Tipo**: Tipo de dato al cual se desea apuntar, puede ser un tipo de dato simple (char, int, etc.) o un tipo de dato complejo como una estructura).
* **Modificadores del tipo**: Puede contener cualquier combinación de los modificadores de tipo const, volatile y restrict.
* **Nombre**: Nombre del apuntador.
* **Valor inicial**: Valor inicial del apuntador. 

La siguiente figura muestra esto lo anterior en términos del mapa de memoria:

<table>
    <tr>
        <td><b>Instrucciones</b></td>
        <td><b>Mapa de memoria</b></td>
    </tr>
    <tr>
        <td><b>    
        short i = 5; <br>
        short *ptr = &i; 
         </b> <br> <br>
         <b>Nota</b>:
        <ul>
        <li>El tamaño de una variables short es de 2 bytes.</li>
        <li>El tamaño de una variable tipo apuntador es de 4 bytes.</li>
        <li>En el dibujo del mapa de memoria cada dirección aumenta de 1 en 1</li>
        </ul> 
        </td>       
        <td>
            <img src="https://github.com/dannymrock/SO-UdeA-20181/blob/master/Lab2/Parte2/teoria/imagenes/rep_ptr_map1.png"> 
        </td>
    </tr>
</table>

Como se puede notar en la figura anterior, lo que se guarda en el apuntador es la dirección base (dirección del byte de menor peso) de la variable a la cual se apunta. Para el caso anterior, la variable i ocupa 2 bytes (103 y 104) sin embargo, en el apuntador se almacena la parte menos correspondiente al byte pesado (byte 103). 

Si observa la segunda instrucción anteriormente mostrada, la forma como se obtuvo la dirección de i fue por medio del operador dirección (&) antepuesto a la variable. La siguiente tabla se llena con base en la figura anterior:


| Expresion	| Significado |	Valor |
|-----------|-------------|-------|
|i	| Contenido de i	|5|
|&i	| Dirección de i	|103|
|p	| Contenido del apuntador p	|103|
|&p	| Dirección del apuntador p	|106|

Nótese de la tabla anterior y la figura previa que con & lo que se obtiene es dirección base de una variable no importa su tipo ya sea una variable normal (char, int, float, etc), apuntador u otro. A continuacion se muestra una forma simplificada (tomada de la sección **Pointers basics** de [How Stuff Works](https://computer.howstuffworks.com/c22.htm)) para visualizar los apuntadores y las variables comunes de manera grafica sin tener que recurrir al bosquejo del mapa de memoria previamente realizado. 



### 2.2. Manipulación de memoria mediante apuntadores

Una de las aplicaciones más importantes es el acceso directo a memoria para su manipulación. Para ello, se manejan dos operadores importantes los cuales el operador referencia (&) y el operador desreferencia (*).

### 2.2.1. Referenciar un apuntador

Consiste en asociar el apuntador a una dirección específica (durante la declaración o después de esta), para esto se suele usar el operador & para obtener la dirección de la variable en cuestión. A continuación se muestra la forma como normalmente se hace esto:

```C
apuntador = &variable;
```
También es posible referenciar un apuntador pasándole el valor que se tiene en otro apuntador. Note que no se hizo uso del operador & en este caso:

```C
apuntador = &variable;
```
Todo apuntador debe inicializarse antes de usarse. Si esto no se hace, cuando intente usarlo para hacer alguna operación en memoria el programa sacara un error. Un puntero que no ha sido inicializado se conoce como **Wild pointer**.

### 2.2.2. Desreferenciar un apuntador
Para poder acceder al lugar de memoria que está siendo apuntado por el puntero y realizar operaciones de lectura y escritura sobre esta dirección de memoria se debe desreferenciar el apuntador. Para ello se hace uso del operador desreferencia (*) después de la declaración del apuntador. El contenido del lugar de memoria apuntado (lectura) se obtiene de la siguiente manera:

```C
variable = *apuntador;
```
Ahora si lo que se desea hacer es escribir en el lugar de memoria apuntado se hace lo siguiente:

```C
*apuntador = variable;
```
En la siguiente figura se ilustra un poco mejor las 2 operaciones anteriores:




## 7. Enlaces de interes
* https://www.geeksforgeeks.org/data-types-in-c/
* https://www.programiz.com/c-programming/c-enumeration




