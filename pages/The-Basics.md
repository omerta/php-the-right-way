---
layout: page
title:  Los Fundamentos
sitemap: true
---

# Los Fundamentos

## Operadores de Comparación

Las operadores de comparación son un aspecto comúnmente desestimado de PHP, lo que puede conducir a muchos resultados inesperadas. Un problema de este tipo
resulta de las comparaciones estrictas (la comparación de booleanos como un enteros).

{% highlight php %}
<?php
$a = 5;   // 5 como un entero

var_dump($a == 5);       // compara el valor; regresa true
var_dump($a == '5');     // compara el valor (ignora el tipo de dato); regresa true
var_dump($a === 5);      // compara el tipo de dato y el valor (integer vs. integer); regresa true
var_dump($a === '5');    // compara el tipo de dato y el valor (integer vs. string); regresa false

//Comparaciones de igualdad
if (strpos('testing', 'test')) {    // 'test' es encontrado en la posición 0, por lo que es interpretado como un booleano 'false'
    // code...
}

// vs. comparaciones estrictas
if (strpos('testing', 'test') !== false) {    // true, con la comparación estricta se evalúa (0 !== false)
    // code...
}
{% endhighlight %}

* [Operadores de comparación](http://php.net/language.operators.comparison)
* [Tabla de comparación](http://php.net/types.comparisons)
* [Comparison cheatsheet](http://phpcheatsheets.com/index.php?page=compare)

## Sentencias condicionales

### Sentencia if

Cuando usamos la sentencia 'if/else' dentro de una función o método de una clase, hay un error común al creer que 'else' debe ser usada
para manejar un posible resultado. Sin embargo si el resultado es para establecer el valor de retorno, 'else' no es
necesaria porque 'return' terminará la función, con lo que resulta que 'else' es irrelevante.

{% highlight php %}
<?php
function test($a)
{
    if ($a) {
        return true;
    } else {
        return false;
    }
}

// vs.

function test($a)
{
    if ($a) {
        return true;
    }
    return false;    // else no es necesario
}

// o aún más corto:

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [If statements](http://php.net/control-structures.if)

### Sentencia Switch

Las sentencias _switch_ son una buena manera de evitar escribir interminables sentencias _if_ y _else_, pero hay algunas cosas de las que debemos saber:

- La sentencia _switch_ solo compara el valor y no el tipo (equivale a '==')
- La sentencia _switch_ itera caso por caso hasta encontrar la coincidencia. Si no se encuentra una coincidencia el caso 'default', en caso de estar definido, será usado.
- Sin un 'break' los casos continuarán implementandose hasta alcanzar un break/return.
- Dentro de una función el uso de 'return' sustrae la necesidad de un 'break', la sentencia 'return' finaliza la función.

{% highlight php %}
<?php
$answer = test(2);    // el código para el 'case 2' y el 'case 3' será ejecuado.

function test($a)
{
    switch ($a) {
        case 1:
            // code...
            break;             // break se usa para finalizar la sentencia switch
        case 2:
            // code...         // sin la sentencia break la comparación continuará en el 'case 3'
        case 3:
            // code...
            return $result;    // dentro de una función la sentecia 'return' finalizará la función
        default:
            // code...
            return $error;
    }
}
{% endhighlight %}

* [Switch statements](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## Espacio de nombre globales

Cuando se usan espacios de nombres encontraras que las funciones internas son ocultadas por las funciones que tu escribiste. Esto se arregla haciendo referencia a
la función global con el uso de la barra inversa antes del nombre de la función.

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // El nombre de nuestra función es el mismo que una función interna.
                         // Para ejecutar la función que esta en el espacio de nombres global se agrega '\'
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator is an internal class. Using its name without a backslash
                                         // will attempt to resolve it within your namespace.
}
{% endhighlight %}

* [Global space](http://php.net/language.namespaces.global)
* [Global rules](http://php.net/userlandnaming.rules)

## Cadena de caracteres

### Concatenación

- Si una linea se extiende más allá de la longitud de linea recomendada (120 caracteres), considere concatenar la linea.
- Por legibilidad es mejor usar el _operador de concatenación_ antes que el _operador de asignación sobre concatenación_.
- Mientras se este en el alcance original de la variable se debe identar cuando la concatenación precisa una nueva linea.

{% highlight php %}
<?php
$a  = 'Multi-line example';    // operador de asignación sobre concatenación (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // operador de concatenación (.)
    . "\n"                     // identar las nuevas lineas
    . 'of what to do';
{% endhighlight %}

* [String Operators](http://php.net/language.operators.string)

### Cadenas de caracteres

Las cadenas de caracteres son una serie de caracteres, esto suena bastante simple. Pero hay diferentes tipos de
cadenas y ellas tienen una sintaxis un poco diferente con un comportamiento un poco diferente.

#### Comillas simples

Las comillas simples son usadas para denotar una "cadena de caracteres literal". Las cadenas de caracteres literales no intentan resolver los caracteres especiales
o las variables.

Si se usan comillas simples y se introduce un nombre de variable en una cadena de caracteres, por ejemplo: `'some $thing'`, se tendrá como salida exactamente
el mismo nombre de la variable: `some $thing`. Por el contrario si se usan comillas dobles se intentará evaluar el nombre de variable `$thing` y se muestra
un error si la variable no fue encontrada.

{% highlight php %}
<?php
echo 'This is my string, look at how pretty it is.';    // no es necesario evaluar una cadenad de caracteres simple

/**
 * Output:
 *
 * This is my string, look at how pretty it is.
 */
{% endhighlight %}

* [Single quote](http://php.net/language.types.string#language.types.string.syntax.single)

#### Comillas dobles

Las comillas dobles son la navaja de la armada suiza. Ellas no solo evaluan variables
como ya hemos dicho, además, evaluan todo el conjunto de caracteres especiales,
como `\n` para una nueva linea o `\t` para una tabulación.


{% highlight php %}
<?php
echo 'phptherightway is ' . $adjective . '.'     // a single quotes example that uses multiple concatenating for
    . "\n"                                       // variables and escaped string
    . 'I love learning' . $code . '!';

// vs

echo "phptherightway is $adjective.\n I love learning $code!"  // Instead of multiple concatenating, double quotes
                                                               // enables us to use a parsable string
{% endhighlight %}

Las comillas dobles pueden contener variables, a esto se llama interpolación.

{% highlight php %}
<?php
$juice = 'plum';
echo "I like $juice juice";    // Output: I like plum juice
{% endhighlight %}

Cuando se usa la interpolación es común añadir a la variable otro
caracter. Resultará un poco confuso determinar que es un nombre de una variable
y que es un caracter literal.

Para resolver este problema se debe envolver la variable dentro de un par de llaves.

{% highlight php %}
<?php
$juice = 'plum';
echo "I drank some juice made of $juices";    // $juice cannot be parsed

// vs

$juice = 'plum';
echo "I drank some juice made of {$juice}s";    // $juice will be parsed

/**
 * Complex variables will also be parsed within curly brackets
 */

$juice = array('apple', 'orange', 'plum');
echo "I drank some juice made of {$juice[1]}s";   // $juice[1] will be parsed
{% endhighlight %}

* [Double quotes](http://php.net/language.types.string#language.types.string.syntax.double)

#### Nowdoc

Nowdoc fue agregado en 5.3 e internamente se comporta de la misma manera que las comillas simples
con la excepción de que es más útil cundo se usan cadenas de caracteres de multiples lineas sin la
necesidad de concatenar.

{% highlight php %}
<?php
$str = <<<'EOD'             // initialized by <<<
Example of string
spanning multiple lines
using nowdoc syntax.
$a does not parse.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using nowdoc syntax.
 * $a does not parse.
 */
{% endhighlight %}

* [Nowdoc syntax](http://php.net/language.types.string#language.types.string.syntax.nowdoc)

#### Heredoc

Heredoc internamente se comporta de la misma manera que las comillas dobles con
la excepción de que es más útil cuando se usan codenas de caracteres de muchas lineas
evitando la necesidad de concatenar.

{% highlight php %}
<?php
$a = 'Variables';

$str = <<<EOD               // initialized by <<<
Example of string
spanning multiple lines
using heredoc syntax.
$a are parsed.
EOD;                        // closing 'EOD' must be on it's own line, and to the left most point

/**
 * Output:
 *
 * Example of string
 * spanning multiple lines
 * using heredoc syntax.
 * Variables are parsed.
 */
{% endhighlight %}

* [Heredoc syntax](http://php.net/language.types.string#language.types.string.syntax.heredoc)

### ¿Cual es más rápido?

Hay un mito alrededor de las cadenas de caracteres con comillas simples en las que
estas son un poco mas rápidas que las cadenas de caracteres con comillas dobles.
Esto es básicamente falso.

Si se esta definiendo una cadena de caracteres simple y no se intenta concatenar
valores ni nada complicado entonces tanto las comillas simples como las dobles serán
completamente identícas. Ninguno será más rápido.

Si se esta concatenando multiples cadenas de caracteres de cualquier tipo o interpolando valores
dentro de una cadena de caracteres con comillas dobles, entonces, el resultado puede
variar. Si se esta trabajando con un pequeño número de valores la concatenación es
minimamente más rápida. Con muchos valores la interpolación es minimanmente más rápido.

Sin importar que se este haciedno con las cadenas de caracteres, ninguno de los tipos
tendra algún impacto notable en su aplicación. Intentar escribir código de una u otra
manera como ejercicio es inútil, así que evite esta micro-optimización a menos que
realmente entienda el significado e impacto de la diferencia.

* [Disproving the Single Quotes Performance Myth](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## Operadores ternarios

Los operadores ternarios son un bueno forma de condesar código pero a menudo son usados en exceso.
Mientras que los operadores ternarios pueden ser amontonados/anidados, es aconsejable usar uno por
linea por legibilidad.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

En comparación este ejemplo sacrifica totalmente la legibilidad para reducir el número de lineas.

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

Para 'return' un valor con operadores ternarios se debe usar una correcta sintaxis.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'

{% endhighlight %}

Debe ser notado que no se necesita usar un operador ternario para regresar un valor boleano. Un ejemplo de esto
pude ser.

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3

{% endhighlight %}

Esto mismo se puede decir para todos los operadores (===, !==, !=, ==, etc).

#### Usando paréntesis con operadore ternarios en fornularios y fubciones

Cuando se usa un operador ternario los paréntesis pueden mejorar la legibilidad del código y además incluir
uniones dentro de un blocke de sentencias. Un ejemplo de cuando no hay necesidad de usar parentesis es:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
{% endhighlight %}

Usar paréntesis tambien nos ofrece la capacidad de crear uniones dentro de un bloque de sentencia en donde el bloque
será comprobado como un todo. Así, el ejemplo de abajo regresará true si ambos ($a == 3 and $b == 4) son true y $c == 5 es
tambíen true.

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Otro ejemplo es el pedazo de abajo que regresará true si ($a != 3 AND $b != 4) o $c == 5.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

Desde PHP 5.3 es posible omitir la parte media del operador ternario.
La expresión "expr1 ?: expr3" regresa expr1 si expr1 es evaluada como TRUE de lo contrario expr3.

* [Ternary operators](http://php.net/language.operators.comparison)

## Declaración de variables

A veces los programadores intentan hacer su código más "limpio" declarando variables predefinidas con un nombre diferente. Lo que
esto hace en realidad es duplicar el consumo de memoria del código en cuestión. En el ejemplo que se muestra abajo consideramos
una cadena de caracteres de texto que pesa 1MB al copiar la variable se ha incrementado la ejecución del código a 2MB.

{% highlight php %}
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
{% endhighlight %}

* [Performance tips](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
