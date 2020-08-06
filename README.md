# NOG - Notes Generator
Flex, Bash and PDFLaTex to take easy beautiful notes.
***
#### Contents

- [What is NOG?](#what-is-nog?)
  - [And why not just markdown and pandoc?](#and-why-not-just-markdown-and-pandoc?)
- [Usage](#usage)
- [Build NOG](#build-nog)

## What is NOG?

LaTex is pretty, we can all agree in that. But taking notes in LaTex can be really tedious, too. NOG is a way to simplify this process. It's a pseudo-language which compiles to latex, and then to pdf. It is intended to be some kind of medium point between markdown and latex, something easy to write but extensible and customizable.

### And why not just markdown and pandoc?

Writing notes in markdown and translating to LaTex with pandoc is great, and you should also try it. But NOG doesn't intend to be a format translator. Its purpose is to both simplify part of the LaTex syntax and unify some aspects that may require some boilerplate LaTex code, like the section tocs or the glossary, for example.

And, of course, another equally important reason is the possibility of extending NOG with new rules in Flex or LaTex commands. In the end the idea is to have nicer results with less typing.

## Usage

`nog [options] <input_files>`

 Options | Description 
---|---
`-h`| Display a help message. 
`-v`| Display the version of the software. 
`-a <author>`| Set the author of the notes. It is empty by default. 
`-t <title>`| Set the title of the notes. It is 'Notes' by default. 
`-d `| Omit the date in the title. 
`-o <output_file>`| Specify name of output file without the pdf extension. It is 'Notes' by default (produces 'Notes.pdf'), 
`-s, --save`| Save all temporal files (including the `.tex`) in a directory called`nogtemp`. 
`-g`| Add an appendix with the glossary. 
`-f`| Add an appendix with the list of fixme. 
`-k <seconds>`| Set how much to wait until killing `pdflatex` if it doesn't compile. 
`-l <language>`| Set the language for the package `babel`. 

## Build NOG

The three targets of the makefile are quite straightforward: 

- `make all` to compile the binaries and the man page.
- `make install` to install the binaries and the man page.
- `make uninstall`  to uninstall the binaries and the man page.

If you want to install NOG:

```
$ cd path/to/the/project
$ sudo make install
...
$ nog -v
nog 1.1
```

If you just want to build it, without installing:

```
$ cd path/to/the/project
$ make
...
$ ./nog -v
nog 1.1
```

## How does it work

The files passed to the `nog` command are processed in order, so you could think of them as a single concatenated file. Their content are passed to the body of the LaTex document.

### Features

#### No more useless escaping

There are some characters in LaTex that must be escaped when not in math mode. NOG escapes them in the context where it is clear that they must be escaped:
- `_` is escaped everywhere outside math mode.
- `$` is escaped within the name of units, subunits and subsubunits, keywords and snippets.(see below)
#### Sectioning with Units

To keep it simple, NOG uses just three levels of sectioning: _Unit_, _Unit section_ and _Unit subsection_. The main Table of Contents contains just the Units, and each Unit contains another ToC with its unit sections and unit subsections.

```
Unit
****
Unit section
============
Unit subsection
---------------
```
#### Emphasis

Double asterisks for bold text and double underscore for italic test to mark keywords.

```
**bold**
__italic__
```
#### Footnotes

Footnotes use arabic numbers, restarting in each page.

```
_(footnote)_
```
#### Lists

Lists between `{*` and `*} `use bullets.

Lists between `{#` and `#} `use numbers.

Lists between `{.` and `.}` use none.

Items starting with `-` are normal items.

items starting with `+` start with bold text until a dot (`.`), a colon (`:`) or a new line.

```
{*
- first item
- second item
+ important: this is another item
*}
```



#### Code
The insertion of code works the same way as in markdown.
```
	`code word`
    '''[language]
    code block
    '''
```
_Note_: For format reasons I've used  ''' , but the correct way is \`\`\`.

#### Fixmes

Also, a list of incomplete or wrong parts can be added. For this purpose you can just insert `((FIXME))` to any point of the document, and a link to it will be added in an optional appendix (see [`-f` option](#usage)).

```
((FIXME)) Complete this section
((FIXME)) Correct this formula
```

#### Keywords

Keywords are marked by two exclamation signs. They are  appear in bold text and have a hand glyph pointing at them in the margin of the page. A link to them is added in an optional appendix (see [`-g` option](#usage)).

```
!!keyword!!
```

_Note_: Keywords doesn't support non ascii characters.

#### Symbol substitution

NOG also makes some substitution to commonly used symbols that would be easier to write and understand in a graphical way.

##### Arrows (Text and math mode)
```
-> --> => ==>
<- <-- <= <==
<-> <--> <=> <==>
```
##### Function defined by parts (Math mode)

```
$$
abs(x) = {{
	-x & if x < 0 \\
	 x & otherwise
}}
$$
```
#### LaTex commands

Latex commands can be used normally inside the notes.

```
\begin{center}
This text is centered.
\end{center}
```



## To Do

- List of features pending to be implemented (they can currently be done via LaTex, but )
    - hyperlinks
    - tables
    - multicolumn text
    - boxed text
    - graphs/trees (maybe automatize tikz)


```

```