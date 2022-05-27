## **Flags/Modifiers**

> Example of regular expression: 
> - /[\w+]/gi
>
> A regular expression pattern are delimited by the slashes `/` and `/`.
> 
> The `g` and `i` after the slash are **modifiers** or **flags**, by which we can change the mode of the regex engine.

- `g` = Find all matches (The engine finds only the first match if not specified)
- `m` = The position indicators can work on each line (By default, the indicators work on the whole string)
- `i` = Case insensitive
- `x` = ignore all whitespace and allow for comment in the regex pattern (Whitespace must be escaped for being included)
- `s` = The `.` meta-character can match newlines
- `u` = The string will be treated as UTF-16. Unicode characters ar included in `[a-z]` and `\w`
- `X` = If the character being escaped is not meta-character, it will raise an error.
- `U` = 
- `A` = Force to become anchored at the beginning of the string
- `J` = 

---

## **Meta-characters**

> By the term "meta-character", it means that a character has special meaning during processing regex pattern. When these characters are used, their special meaning, rather than the original literal meaning, would be taken into account. If we want to use their literal meaning instead, we must place an escaped character ("\\") in front of each of them. 
> 
> The 14 meta-characters are listed below:

- `.`
- `?`
- `$`
- `^`
- `*`
- `|`
- `\`
- `+`
- `(` and `)`
- `{` and `}`
- `[` and `]`

---

## **Character classes**

`.` = Any one character except newline. equivalent to `[^0-9]`

`\d` = Any one digital character. equivalent to `[0-9]`

`\D` = Any one **non-digital character**. equivalent to `[^0-9]`

`\w` = Any one word character. equivalent to `[a-zA-Z0-9_]`

`\W` = Any one **non-word character**. equivalent to `[^a-zA-Z0-9_]`

`\s` = Any one space character. equivalent to `[ \n\r\t\f\v]`

`\S` = Any one **non-space character**. equivalent to `[^ \n\r\t\f\v]`

`\n` = New line character

`\r` = Carriage return character (Return the print head to the beginning of a line)

`\t` = Tab character

---

## **Repetition operators**

`?` = Articulate optional character (Zero of one time)

`*` = Zero or more times

`+` = One or more times

`{}` = Define number(s) of repetition
- {n} = Exact number of repetition
- {m,} = Greater than or equal to m times
- {,n} = Lesser than or equal to n times
- {m,n} = From m to n times

---

## **Position indicators**

`^` = Character that at the position of the beginning of line

`$` = Character that at the position of the end of line

`\b` = Character that at the position of the beginning of line or the end of line

`\B` = Character that neither at the position of the beginning of line or the end of line

---
## **Alternatives**

`|` = OR Operator

`[]` = Any one character that presents in the list
- `[^]` = Any one character that **does not** present in the list

---

`()` =  Group pattern and create a back reference
- `\1, \2, \3...` = to capture (back reference) the group

> Example: 
>
> 

---

## ****

`(?=x)`= Lookahead. 

`(?!x)`= Negative lookahead.

`(?<=x)`= Lookbehind.

`(?<!x)`= Negative lookbehind.

---


## References:

1. [Nanyang Technological University - Regex](https://www3.ntu.edu.sg/home/ehchua/programming/howto/Regexe.html)
2. [Regex 101](https://regex101.com/)
3. [Regex Tutorials](http://regextutorials.com/index.html)
