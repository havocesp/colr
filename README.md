Colr
====

A python module for using terminal colors in linux. It contains a simple
`color` function that accepts style and color names, and outputs a string
with escape codes, but also has all colors and styles as chainable methods
on the `Colr` object.

_______________________________________________________________________________

Examples:
---------

###Simple:

```python
from colr import color
print(color('Hello world.', fore='red', style='bright'))
```

###Chainable:
```python
from colr import Colr as C
print(
    C()
    .bright().red('Hello ')
    .normal().blue('World')
)

# Background colors start with 'bg', and AttributeError will be raised on
# invalid method names.
print(C('Hello ', fore='red').bgwhite().blue('World'))

```

Examples (256 Colors):
----------------------

###Simple:

```python
from colr import color
# Invalid color names/numbers raise a ValueError.
print(color('Hello world', fore=125, back=80))
```

###Chainable:

```python
from colr import Colr as C
# Foreground colors start with 'f_'
# Background colors start with 'b_'
print(C().f_125().b_80('Hello World'))
```

_______________________________________________________________________________


Other methods:
--------------

The `Colr` object has several helper methods.
The `color()` method returns a `str`, but the rest return a `Colr` instance
so they can be chained.
A chainable version of `color()` does exist (`chained()`), but it's not really
needed outside of the `colr` module itself.

###Colr.center

Like `str.center`, except it ignores escape codes.

```python
Colr('Hello', fore='green').center(40)
```

###Colr.format

Like `str.format`, except it operates on `Colr.data`.

```python
Colr('Hello').blue(' {}').red(' {}').format('my', 'friend').center(40)
```

###Colr.gradient

Like `rainbow()`, except a known name can be passed to choose the color
(same names as the basic fore colors).

```python
(Colr('Wow man, ').gradient(name='red')
.gradient('what a neat feature that is.', name='blue'))
```

###Colr.gradient_black

Builds a black and white gradient. The default starting color is black, but
white will be used if `reverse=True` is passed. Like the other `gradient/rainbow`
functions, if you pass a `fore` color, the background will be gradient.

```python
(C('Why does it have to be black or white?').gradient_black(step=3)
.gradient_black(' ' * 10, fore='reset', reverse=True))
```

###Colr.join

Joins `Colr` instances or other types together.
If anything except a `Colr` is passed, `str(thing)` is called before
joining. `join` accepts multiple args, and any list-like arguments are
flattened at least once (simulating str.join args).

```python
Colr('alert', 'red').join('[', ']').yellow(' This is neat.')
```

###Colr.ljust

Like `str.ljust`, except it ignores escape codes.

```python
Colr('Hello', 'blue').ljust(40)
```

###Colr.rainbow

Beautiful rainbow gradients in the same style as [lolcat](https://github.com/busyloop/lolcat).
This method is incapable of doing black and white gradients. That's what
`gradient_black()` is for.
```python
C('This is really pretty.').rainbow(freq=.5)
```

###Colr.rjust

Like `str.rjust`, except it ignores escape codes.

```python
Colr('Hello', 'blue').rjust(40)
```

###Colr.str

The same as calling `str()` on a `Colr` instance.
```python
Colr('test', 'blue').str() == str(Colr('test', 'blue'))
```

###Colr.\_\_add\_\_

Strings can be added to a `Colr` and the other way around.
Both return a `Colr` instance.

```python
Colr('test', 'blue') + 'this' == Colr('').join(Colr('test', 'blue'), 'this')
'test' + Colr('this', 'blue') == Colr('').join('test', Colr(' this', 'blue'))

```

###Colr.\_\_call\_\_

`Colr` instances are callable themselves.
Calling a `Colr` will append text to it, with the same arguments as `color()`.

```python
Colr('One', 'blue')(' formatted', 'red')(' string.', 'blue')
```

###Colr.\_\_eq\_\_, \_\_ne\_\_

`Colr` instances can also be compared with other `Colr` instances.
They are equal if `self.data` is equal to `other.data`.

```python
Colr('test', 'blue') == Colr('test', 'blue')
Colr('test', 'blue') != Colr('test', 'red')
```

###Colr.\_\_lt\_\_, \_\_gt\_\_, \_\_le\_\_, \_\_ge\_\_
Escape codes are stripped for less-than/greater-than comparisons.

```python
Colr('test', 'blue') < Colr('testing', 'blue')
```

###Colr.\_\_getitem\_\_

Escape codes are stripped when subscripting/indexing.

```python
Colr('test', 'blue')[2] == Colr('s')
Colr('test', 'blue')[1:3] == Colr('es')
```

###Colr.\_\_mul\_\_

`Colr` instances can be multiplied by an `int` to build color strings.
These are all equal:

```python
Colr('*', 'blue') * 2
Colr('*', 'blue') + Colr('*', 'blue')
Colr('').join(Colr('*', 'blue'), Colr('*', 'blue'))
```

_______________________________________________________________________________

Color Translation:
------------------

The `colr` module also includes several tools for converting from one color
value to another:

###ColorCode

A class that automatically converts hex, rgb, or terminal codes to the other
types. They can be accessed through the attributes `code`, `hexval`, and `rgb`.

```python
from colr import ColorCode
print(ColorCode(30))
# Terminal:  30, Hex: 008787, RGB:   0, 135, 135

print(ColorCode('de00fa'))
# Terminal: 165, Hex: de00fa, RGB: 222,   0, 250

print(ColorCode((75, 50, 178)))
# Terminal:  61, Hex: 4b32b2, RGB:  75,  50, 178
```

Printing `ColorCode(45).example()` will show the actual color in the terminal.

###hex2rgb

Converts a hex color (`#000000`) to RGB `(0, 0, 0)`.

###hex2term

Converts a hex color to terminal code number.

```python
from colr import color, hex2term
print(color('Testing', hex2term('#FF0000')))
```
###hex2termhex

Converts a hex color to it's closest terminal color in hex.

```python
from colr import hex2termhex
hex2termhex('005500') == '005f00'
```

###rgb2hex

Converts an RGB value `(0, 0, 0)` to it's hex value (`000000`).

###rgb2term

Converts an RGB value to terminal code number.

```python
from colr import color, rgb2term
print(color('Testing', rgb2term(0, 255, 0)))
```

###rgb2termhex

Converts an RGB value to it's closest terminal color in hex.

```python
from colr import rgb2termhex
rgb2termhex(0, 55, 0) == '005f00'
```

###term2hex

Converts a terminal code number to it's hex value.

```python
from colr import term2hex
term2hex(30) == '008787'
```

###term2rgb

Converts a terminal code number to it's RGB value.

```python
from colr import term2rgb
term2rgb(30) == (0, 135, 135)
```
_______________________________________________________________________________


Colr Tool:
----------

The `colr` package can be used as a command line tool:
```
python3 -m colr --help
```

It will do fore, back, style, gradients, rainbows, justification,
translation, and code stripping of input (as an argument or stdin).

lolcat emulation:
```
fortune | python3 -m colr --rainbow
```
_______________________________________________________________________________

Notes:
------

* In the past, I used a simple `color()` function because I'm not fond of the
    string concatenation style that other libraries use. The 'clor' javascript
    library uses method chaining because that style suits javascript, but I wanted
    to make it available to Python also, at least as an option.

* The reset code is appended to all text unless the text is empty.
    This makes it possible to build background colors and styles, but
    also have separate styles for separate pieces of text.

* I don't really have the desire to back-port this to Python 2. It wouldn't need
    too many changes, but I like the Python 3 features (`yield from`, `str/bytes`).

* Windows is not supported yet, but I'm working on it.

* This library may be a little too flexible, and that may change:

```python
from colr import Colr as C
warnmsg = lambda s: C('warning', 'red').join('[', ']')(' ').green(s)
print(warnmsg('The roof is on fire again.'))
```

![The possibilities are endless.](https://welbornprod.com/static/media/img/colr-warning.png)


