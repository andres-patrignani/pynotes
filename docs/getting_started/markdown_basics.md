# Markdown basics

Author: Andres Patrignani

Github-flavored (Kramdown) official documentation: https://github.github.com/gfm/

## Comments

Markdown does not seem to have an official way of adding comments. However, we can fool several Markdown interpreters by preceding text with the following expression `[//]:`

`[//]: This is a comment`

Note that this trick might not work in some Markdown editors like Typora, but it does work in Github.


## Line breaks
Pressing the `enter` key will not generate empty lines. Because Markdown eventually is converted into HTML (HyperText markup Language, the language we use to write websites), we can use HTML tags to expand the editing and styling possibilities in our document. So, to add a line break, we can use the self-closing line break tag: `<br/>`. 

```markdown
some text
</br>
more text
```



## Headers

Represented by adding 1 to 6 leading # signs

```none
# Header
## Header
### Header
#### Header
##### Header
###### Header
```
# Header
## Header
### Header
#### Header
##### Header
###### Header



# Emphasis

```
*italic text*
_italic text_
**bold text**
__bold text__
~~striked text~~
```



*italic text*

_italic text_

**bold text**

__bold text__

~~text~~




## Highlighting
```none
`This text is highlighted using backward ticks`
```



`This text is highlighted using backward ticks`

## Monospace font
    Indent using the Tab key to generate a monospace font.

## Inline equations

```none
$y = ax+b$
```



$y = ax+b$


## Block equations

Example equation for calculating actual vapor pressure (Eq. 17, FAO-56):

```none
$$ea = \frac{eTmin\frac{RHmax}{100}+eTmax\frac{RHmin}{100}}{2}$$ 
```



$$ea = \frac{eTmin\frac{RHmax}{100}+eTmax\frac{RHmin}{100}}{2}​$$ 


    ea = actual vapor pressure (kPa)
    
    eTmax = saturation vapor pressure at temp Tmax (kPa)
    
    eTmin = saturation vapor pressure at temp Tmin (kPa)
    
    RHmax = maximum relative humidity (%)
    
    RHmin = minimum relative humidity (%)

## Block quotes

Use the `>` character to generate block quotes.

```none
>"Every great developer you know got there by solving problems they were unqualified to solve until they actually did it." *- Patrick McKenzie*
```

> "Every great developer you know got there by solving problems they were unqualified to solve until they actually did it." *- Patrick McKenzie*

## Bullet lists
```none
- item 1
- item 2
- item 3
```
or
```none
* item 1
* item 2
* item 3
```

- item 1
- item 2
- item 3


## Numbered Lists

```none
1. item 1
2. item 2
3. item 3
```
1. item 1
2. item 2
3. item 3

## Mixed lists



```none
1. item 1
    * item 1a
    * item 1b
```
1. item 1
    - item 1a
    - item 1b



## In-line links
```none
[Github-flavored markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
```



[Github-flavored markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

## Referenced links

```none
[Try a live Markdown editor in your browser][1]

Some text
Some more text

[1] https://stackedit.io "Optional title to identify your source"
```



[Try a live Markdown editor in your browser][1]

[1]: https://stackedit.io	" Stackedit browser Markdown editor"



## In-line images

You can use a URL to link images in the web, relative links in repositories, or the path of an image in your local directory

```none
![alt_text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")
```



Here is some text followed by an image: 
![alt_text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Instead of this image from an URL you can also link images in your own computer using the fullpath of the image. You can also use pure HTML code
```none
<img src="sketch.jpg" alt="sketch_image" width="500"/>
```
## Referenced images

```none
logo https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png Logo "Example image"
```



![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Example image"



## Horizontal lines

You can use three consecutive dashes, astericks, or underscores

```none
---
***
___
```



[//]: # "Dashes"
---

[//]: # "Asterics"
***

[//]: # "Underscores"
___



## Code

We can write inline or block code. Inline code:

```none
`s = "Python inline code syntax highlighting"`
```



`s = "Python inline code syntax highlighting"`

and block code:

```none
​```python
# Creating a matrix or 2D array
M = [[1, 4, 5],
    [-5, 8, 9]]
print(M)
​```
```

```pyhon
# Creating a matrix or 2D array
M = [[1, 4, 5],
    [-5, 8, 9]]
print(M)
```



## Tables

Simple tables are easy to write in Markdown. However, adding more than a handful of rows and/or columns can turn out to be a pain. So, if you want to display many lines I suggest using a Markdown table generator. Some Markdown editors have shortcuts and table generators and there are websites exclusively dedicated to generate Markdown tables. Below I show a trivial example:

```none 
| item     | quantity  | price |
|:---------|:---------:|------:|
| oranges  | 5         | 0.45  |
| carrots  | 14        | 1.65  |
| goldfish | 1         | 5.00  |
```
The leftmost column is left-aligned `:---`, the center column is center-aligned `:---:`, and the righmost column is right-aligned `---:`. The `|` characters don't need to be aligned in order for the Mardown interpreter to properly render the table, but it certainly helps while constructing the table by hand.

| item     | quantity  | price |
|:---------|:---------:|------:|
| oranges  | 5         | 0.45  |
| carrots  | 14        | 1.65  |
| goldfish | 1         | 5.00  |