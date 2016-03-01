Python Regular Expressions Cheat Sheet
================

A cheat sheet for working with `Python` Regular Expressions (aka `regex`). [Offical Regex Docs](https://docs.python.org/dev/library/re.html)


### Common Symbols 

`\d` Any digit, matches `[0-9]`

`\D` Any non-digit, matches `[^0-9]`, opposite of `\d`


`\w` Any character, matches `[a-zA-Z0-9_]`

`\W` Any non-character, matches `[^a-zA-Z0-9_]`, opposite of `\w`


`\s` Any space (or whitespace), matches `[ \t\n\r\f\v]` *See Note Below

`\S` Any non-space (or whitespace), matches `[^ \t\n\r\f\v]`, opposite of `\s`

> _Note_ `\t\n\r\f\v` refers to escaped characters (below). When using `\s` (or `\S`) the escaped characters will be treated as whitespace:
```
\t: Tab
\n: Newline
\r: Carriage return
\f: Formfeed
\v: Vertical Tab
```

`*` Means 0 or more characters are needed for match. `\d*` Must be 0 digits or more

`+` Means 1 or more characters are needed for match. `\d+` Must be 1 digit or more

`^` Denotes start of string

`$` Denotes end of string


### Basic Examples

#### Example: Find 4 Digits in a String
```
import re

some_string = "Here is some text. We want to find matches in. Let's find the number 1234 and the number 5342 but not the number 942003. "

regex_pattern = re.compile(r"\D(\d{4})\D")
matching_numbers = re.findall(regex_pattern, some_string)

print(matching_numbers)

```

The important part here is the `regex pattern`. Let's break it apart into 3 sections:

`r"<first><desired_match><last>"`

`<first>` is `\D`

`desired_match` is `(\d{4})`

`<last>` is `\D`


`<first>` is telling python that anything that isn't a digit, just ignore it.

`<last>` is telling python that anythingn that isn't a digit, just ignore it.

`desired_match` is saying that the pattern in `()` should be considered as a match. 

Putting `\d` would yeild all 3 sets of numbers `1234`, `5342`, and `942003`. 

Putting `\d{4}` is saying that the digits must be a at least 4 digits to match. 

The `<last>` portion prevents digits larger than 4.


#### Example: Extract Capitalized Words

```
import re

txt = "Can you find A Match with some words: Car, House, Shirt, Apple, Google, Facebook."

regex_pattern = re.compile("([A-Z]{1}[a-z]+)\s?")
matches = re.findall(regex_pattern, txt)

print(matches)
```



All items in `()` are what we want to return

`[A-Z]{1}` means matching item has 1 capital letter

`[a-z]+` means the matching item has lower case letter(s). If we change this portion to: `[a-z]*` we would also have `A` in our results.

`\s` means: do not include spaces in the result

`?` means repeat the previous pattern as needed, or repeat `([A-Z]{1}[a-z]+)\s` as many as needed.


#### Example: Capitalized and Punctuated. 

```
import re

txt = "this is poor syntax"
txt2 = "This isn't poor syntax."

regex_pattern = re.compile("^[A-Z](.*)[\.\?\!]$")
matches = re.match(regex_pattern, txt2)

print(matches)
```

`^` Denotes start of string

`$` Denotes end of string

`^[A-Z]` means string must start capitalized

`(.*)` nearly any value in between start and end of the string

`[\.\?\!]$` Means that the string must end with `.`, `?`, or `!`


### Example: Named Groups

```
import re

p1 = re.compile(r'(?P<word>\b\w+\b)')

m1 = p1.search( '(((( Lots of punctuation )))' )

m1.group('word')

m1.group(1)


p2 = re.compile(r'(?P<slug>[\w-]+)/(?P<slug2>[\w-]+)')

m2 = p2.search( '/some-another/new-slug/' )

m2.group('slug')
m2.group(1)

m2.group('slug2')

m2.group(2)


p3 = re.compile(r'^(?P<id>\d+)')

m3 = p3.search('some-another/4232/')

if m3:
    print(m3.group("id"))

m4 = p3.search('4232/adsfs/asdf')

if m4:
    print(m4.group('id'))

txt = "123/string"

new_ = re.findall(p3, txt)

print(new_)

```








