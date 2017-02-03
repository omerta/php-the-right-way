---
layout: page
title:  Los Fundamentos
sitemap: true
---

# Los Fundamentos

## Operadores de Comparación

Las operadores de comparación son un aspecto comunmente desestimado de PHP, lo que
puede conducir a muchas consecuencias inesperadas. Un problema de este tipo resulta
de las comparaciones estrictas (la comparación de un booleano como un entero).

{% highlight php %}
<?php
$a = 5;   // 5 as an integer

var_dump($a == 5);       // compare value; return true
var_dump($a == '5');     // compare value (ignore type); return true
var_dump($a === 5);      // compare type/value (integer vs. integer); return true
var_dump($a === '5');    // compare type/value (integer vs. string); return false

/**
 * Strict comparisons
 */
if (strpos('testing', 'test')) {    // 'test' is found at position 0, which is interpreted as the boolean 'false'
    // code...
}

// vs.

if (strpos('testing', 'test') !== false) {    // true, as strict comparison was made (0 !== false)
    // code...
}
{% endhighlight %}

* [Operadores de comparación](http://php.net/language.operators.comparison)
* [Tabla de comparación](http://php.net/types.comparisons)
* [Comparison cheatsheet](http://phpcheatsheets.com/index.php?page=compare)

## Sentencias condicionales

### Sentencia if

Cuando usamos la sentencia 'if/else' dentro de una función o método de una clase,
hay un error común al creer que 'else' debe ser usada para manejar un posible resultado.
Sin embargo si el resultado es para fijar el valor de retorno, 'else' no es necesario
porque 'return' terminará la función, con lo que resulta que 'else' es irrelevante.

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
    return false;    // else is not necessary
}

// or even shorter:

function test($a)
{
    return (bool) $a;
}

{% endhighlight %}

* [If statements](http://php.net/control-structures.if)

### Sentencia Switch

Las sentencias _switch_ son una buena manera de evitar escribir interminables
sentencias _if_ y _else_, pero hay algunas cosas de las que debemos saber:

- La sentencia _switch_ solo compara el valor y no el tipo (equivale a '==')
- La sentencia _switch_ itera caso por caso hasta encontrar la coincidencia.
Si no se encuentra una coincidencia el caso 'default', en caso de estar definido,
será usado.
- Sin un 'break' los casos continuarán implementandose hasta alcanzar un break/return.
- Dentro de una función el uso de 'return' sustrae la necesidad de un 'break',
Aquel finaliza la función.

{% highlight php %}
<?php
$answer = test(2);    // the code from both 'case 2' and 'case 3' will be implemented

function test($a)
{
    switch ($a) {
        case 1:
            // code...
            break;             // break is used to end the switch statement
        case 2:
            // code...         // with no break, comparison will continue to 'case 3'
        case 3:
            // code...
            return $result;    // within a function, 'return' will end the function
        default:
            // code...
            return $error;
    }
}
{% endhighlight %}

* [Switch statements](http://php.net/control-structures.switch)
* [PHP switch](http://phpswitch.com/)

## Espacio de nombre globales

Cuando se usan espacios de nombres te encontraras que las funciones internas son
oculatadas por las funciones que tu escribiste. Esto se arregla haciendo referencia
a la funcióm global con el uso de la barra inversa antes del nombre de la función.

{% highlight php %}
<?php
namespace phptherightway;

function fopen()
{
    $file = \fopen();    // Our function name is the same as an internal function.
                         // Execute the function from the global space by adding '\'.
}

function array()
{
    $iterator = new \ArrayIterator();    // ArrayIterator is an internal class. Using its name without a backslash
                                         // will attempt to resolve it within your namespace.
}
{% endhighlight %}

* [Global space](http://php.net/language.namespaces.global)
* [Global rules](http://php.net/userlandnaming.rules)

## Strings

### Concatenación

- Si una linea se extiende más allá de la longitud de linea recomendada (120 caracteres),
considere concatenar la linea.
- Por legibilidad es mejor usar el _operador de concatenación_ antes que el _operador
de asignación sobre concatenación_.
- Mientras se este en el alcance de la variables se debe identar cuando la concatenación
precisa una nueva linea.

{% highlight php %}
<?php
$a  = 'Multi-line example';    // concatenating assignment operator (.=)
$a .= "\n";
$a .= 'of what not to do';

// vs

$a = 'Multi-line example'      // concatenation operator (.)
    . "\n"                     // indenting new lines
    . 'of what to do';
{% endhighlight %}

* [String Operators](http://php.net/language.operators.string)

### Cadenas de caracteres

Las cadenas de caracteres son una serie de caracteres, esto suena bastante simple.
Pero hay diferentes tipos de cadenas y ellas tienen una sintaxis un poco diferente
y un comportamiento un poco diferente.

#### Comillas simples

Las comillas simples son usadas para denotar una "cadena de caracteres literal".
Las cadenas de caracteres literales no intentan resolver los caracteres especiales
o las variables.

Si se usan comillas simples y se introduce un nombre de variable en una cadena de
caracteres, por ejemplo: 'some $thing', se tendrá como salida exactamente el mismo
nombre de la variable $thing. Por el contrario si se usan comillas dobles se intentará
evaluar el nombre de variable $thing y se muestra un error si la variable no fue
encontrada.

{% highlight php %}
<?php
echo 'This is my string, look at how pretty it is.';    // no need to parse a simple string

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

#### Nowdoc syntax

Nowdoc syntax was introduced in 5.3 and internally behaves the same way as single quotes except it is suited toward the
use of multi-line strings without the need for concatenating.

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
evitando la necesidad de concatenar.,

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

### Which is quicker?

There is a myth floating around that single quote strings are fractionally quicker than double quote strings. This is
fundamentally not true.

If you are defining a single string and not trying to concatenate values or anything complicated, then either a single
or double quoted string will be entirely identical. Neither are quicker.

If you are concatenating multiple strings of any type, or interpolate values into a double quoted string, then the
results can vary. If you are working with a small number of values, concatenation is minutely faster. With a lot of
values, interpolating is minutely faster.

Regardless of what you are doing with strings, none of the types will ever have any noticeable impact on your
application. Trying to rewrite code to use one or the other is always an exercise in futility, so avoid this micro-
optimization unless you really understand the meaning and impact of the differences.

* [Disproving the Single Quotes Performance Myth](http://nikic.github.io/2012/01/09/Disproving-the-Single-Quotes-Performance-Myth.html)


## Ternary operators

Ternary operators are a great way to condense code, but are often used in excess. While ternary operators can be
stacked/nested, it is advised to use one per line for readability.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? 'yay' : 'nay';
{% endhighlight %}

In comparison, here is an example that sacrifices all forms of readability for the sake of reducing the line count.

{% highlight php %}
<?php
echo ($a) ? ($a == 5) ? 'yay' : 'nay' : ($b == 10) ? 'excessive' : ':(';    // excess nesting, sacrificing readability
{% endhighlight %}

To 'return' a value with ternary operators use the correct syntax.

{% highlight php %}
<?php
$a = 5;
echo ($a == 5) ? return true : return false;    // this example will output an error

// vs

$a = 5;
return ($a == 5) ? 'yay' : 'nope';    // this example will return 'yay'

{% endhighlight %}

It should be noted that you do not need to use a ternary operator for returning a boolean value. An example of this
would be.

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? true : false; // Will return true or false if $a == 3

// vs

$a = 3;
return $a == 3; // Will return true or false if $a == 3

{% endhighlight %}

This can also be said for all operations(===, !==, !=, == etc).

#### Utilising brackets with ternary operators for form and function

When utilising a ternary operator, brackets can play their part to improve code readability and also to include unions
within blocks of statements. An example of when there is no requirement to use bracketing is:

{% highlight php %}
<?php
$a = 3;
return ($a == 3) ? "yay" : "nope"; // return yay or nope if $a == 3

// vs

$a = 3;
return $a == 3 ? "yay" : "nope"; // return yay or nope if $a == 3
{% endhighlight %}

Bracketing also affords us the capability of creating unions within a statement block where the block will be checked
as a whole. Such as this example below which will return true if both ($a == 3 and $b == 4) are true and $c == 5 is
also true.

{% highlight php %}
<?php
return ($a == 3 && $b == 4) && $c == 5;
{% endhighlight %}

Another example is the snippet below which will return true if ($a != 3 AND $b != 4) OR $c == 5.

{% highlight php %}
<?php
return ($a != 3 && $b != 4) || $c == 5;
{% endhighlight %}

Since PHP 5.3, it is possible to leave out the middle part of the ternary operator.
Expression "expr1 ?: expr3" returns expr1 if expr1 evaluates to TRUE, and expr3 otherwise.

* [Ternary operators](http://php.net/language.operators.comparison)

## Variable declarations

At times, coders attempt to make their code "cleaner" by declaring predefined variables with a different name. What
this does in reality is to double the memory consumption of said script. For the example below, let us say an example
string of text contains 1MB worth of data, by copying the variable you've increased the scripts execution to 2MB.

{% highlight php %}
<?php
$about = 'A very long string of text';    // uses 2MB memory
echo $about;

// vs

echo 'A very long string of text';        // uses 1MB memory
{% endhighlight %}

* [Performance tips](http://web.archive.org/web/20140625191431/https://developers.google.com/speed/articles/optimizing-php)
