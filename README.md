<p align="center">
<img width="615" alt="image" src="https://github.com/ongteckwu/Revice/assets/3834724/bdb660c2-e534-4525-8460-22215c32c8ed">
</p>

<p align="center">
<b>Revice</b> is a high-level regex transpiler library for readability and reusability.
</p>

## Current Progress

Proposal Stage. Any feedback is appreciated on the Issues page with the tag `feedback`.

## Examples

### Supports Permutations

```
(?perm(,):apple,pear,blueberry)
```

transpiles to

```
ap{2}le,(?:pear,blueber{2}y|blueber{2}y,pear)|pear(?:ap{2}le,blueber{2}y|blueber{2}y,ap{2}le)|blueber{2}y(?:ap{2}le,pear|pear,ap{2}le)
```

### base64

```
\#base64#*
```

transpiles to

```
(?:[A-Za-z0-9+/]{4})*(?:[A-Za-z0-9+/]{2}(?:==|[A-Za-z0-9+/]=))?
```

---

```
\#base64#+
```

transpiles to

```
(?:[A-Za-z0-9+/]{4})*?:[A-Za-z0-9+/]{2}(?:==|[A-Za-z0-9+/](?:=|[A-Za-z0-9+/])))?
```

---

```
\#base64#{1,2}
```

transpiles to

```
(?:[A-Za-z0-9+/]{4}){0,1}(?:[A-Za-z0-9+/]{2}==|[A-Za-z0-9+/]{3}=|[A-Za-z0-9+/]{4})
```

---

```
\#b64-char#
```

transpiles to

```
[A-Za-z0-9+/]
```

### if-behind far-else

```
(if-behind<tag>(cond):xxx)[a-z]+(else<tag>:xxx)
```

transpiles to

```
(?:(?!cond)xxx[a-z]+|[a-z]+xxx)
```

## Features

- "Batteries-included" (in a yaml file)
  - modifiers e.g. permutation modifier `(?perm(<delimiter>):`, case-insensitive flag (not in JS), S flag (not in JS),
  - classes e.g. \#b64#, \#b64-char#, \#hex#, \#oct#, \#bound# = `(?:_|\b)`
  - syntactic sugars e.g. `(?ahead:), (?behind:), (?ahead-neg:), (?behind-neg:)
- Supports Custom YAML file
  - building your own classes and modifiers for reuse
  - (optional) create your own exportable regex expressions with automated test cases testing, that can also use your own custom classes and modifiers within
  - Choose to import or not "batteries-included" yaml file
  - Choose to import only certain "batteries-included" variables
- Supports command line transpiler

## Architecture

Modules:

- Argparser for CLI transpiler
- Transpiler
  - transpiles the Customs
  - gets parsed file
  - Tokenizer
    - tokenize based on file
  - Parser
    - parse based on file
  - Error handling
    - handling wrong regexes
      - error message should be based on built expression
- Code Generator
  - generates Variables, have to make use of the Customs
  - ability to add custom prefix (no prefix added if none specified)
  - generates in target language (rust and js first)
    - generates in /generated with the yaml file copied and the revice-gen.rs
- File Reader
  - check correct yaml file
  - convert to json

YAML file:

- Customs
- Variables
