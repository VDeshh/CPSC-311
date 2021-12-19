# **CPSC 311 Study Guide**

**Contents:**

# Stepping Rules Guide With Examples

# AE Language

**Stepping Rules:**

   - A is of type AE which can be stepped further
   - B is of type AE which can be stepped further
   - v, v' and v" represent values

| **AE Expression** | **Steps To** | **Given That**     |
| ----------------- | ------------ | ------------------ |
| `{+ A B}`         | `{+ A’ B}`   | A steps to A’      |
| `{+ v B}`         | `{+ v B’}`   | B steps to B’      |
| `{+ v v’}`        | `v”`         | `v` + `v'`  = `v"` |
| `{− A B}`         | `{− A’ B}`   | `A` steps to `A’`  |
| `{− v B}`         | `{− v B’}`   | `B` steps to `B’`  |
| `{− v v’}`        | `v”`         | `v` − `v'`  = `v"` |

---

**Stepping Examples:**

**Example 1 :**

`(define AEFS1 4)`

Step 0: `'4`

---

**Example 2:**

`(define AEFS2 `{+ ,AEFS1 5})`

Step 0: `'{+ 4 5}`

Step 1: `'9`

---

**Example 3:**

`(define AEFS3 '{- 6 3})`

Step 0: `'{- 6 3}`

Step 1: `'3`

---

**Example 4 :**

`(define AEFS4 '{+ {- 6 3} {- 7 {+ 3 1}}})`

Step 0: `'{+ {- 6 3} {- 7 {+ 3 1}}}`

Step 1: `'{+ 3 {- 7 {+ 3 1}}}`

Step 2: `'{+ 3 {- 7 4}}`

Step 3: `'{+ 3 3}`

Step 4: `'6`

# WAE Language (Lexical Scoping)

**Stepping Rules:**

   - A is of type WAE which can be stepped further
   - B is of type WAE which can be stepped further
   - v, v' represent values

| **AE Expression**                    | **Steps To**        | **Given That**                                              |
| ------------------------------------ | ------------------- | ----------------------------------------------------------- |
| Addition and Subtraction expressions | Same as AE language |                                                             |
| `{with {x A} B}`                     | `{with {x A’} B}`   | `A` steps to `A'`                                           |
| `{with {x v} B}`                     | `B’`                | `B'` is `B` with free instances of `x` substituted with `v` |

#### runtime error ::= ::unbound identifier error::

| **AE Expression**    | **Steps To**                     | **Given That**             |
| -------------------- | -------------------------------- | -------------------------- |
| `x`                  | **::unbound identifier error::** |                            |
| `{with {x A} B}`     | **runtime error**                | `A` steps to runtime error |
| `{+ A B} or {− A B}` | **runtime error**                | `A` steps to runtime error |
| `{+ v B} or {− v B}` | **runtime error**                | `B` steps to runtime error |
| `{+ A v} or {− A v}` | **runtime error**                | `A` steps to runtime error |

---

**Stepping - Unbound Identifier Examples:**

Example 1 :  x occurs free in this WAE expression - **::unbound identifier error::**

`'x`

---

Example 2 : x occurs free in this WAE expression - **::unbound identifier error::**

`'{with {y x} y}`

---

Example 3 : x occurs free in this WAE expression - **::unbound identifier error::**

`'{with {y x} y}`

---

Example 4 : x occurs bound, z occurs free, y is bound but does not occur - **::unbound identifier error::**

`'{with {x 7} {with {y x} z}}`

Let's step this thru,

Step 0 : `'{with {x 7} {with {y x} z}} ;Copying the question`

Step 1 : `'{with {y 7} z} ;Replacing free instances of x with 7`

Step 2: `'z ;There are no free instances of y so it doesn't occur`

**::unbound identifier error  -::** since `'z` is an unbound identifier and we can't step it to a value

---

---

**Stepping - Normal Examples :**

Example 1:

`(define WAES1 '{with {x 4} x})`

Step 0: `'{with {x 4} x}`

Step 1: `'4 ;Replacing free instances of x with 4`

---

Example 2:

`(define WAES2 '{with {x 4} {+ x x}})`

Step 0:  `'{with {x 4} {+ x x}}`

Step 1:  `'{+ 4 4}`

Step 2:  `'8`

---

Example 3:

`;(define WAES3 '{with {x {+ 5 5}} {+ x x}})`

Step 0:  `'{with {x {+ 5 5}} {+ x x}}`

Step 1:  `'{with {x 10} {+ x x}} ;Step the with expression to a value`

Step 2:  `'{+ 10 10} ;Substitute free instances of x with 10`

Step 3:  `'20`

---

Example 4:

`;(define WAES4 '{with {x {+ 5 5}} {with {y {- x 3}} {+ y y}}})`

Step 0:  `'{with {x {+ 5 5}} {with {y {- x 3}} {+ y y}}}`

Step 1:  `'{with {x 10} {with {y {- x 3}} {+ y y}}}`

Step 2: `'{with {y {- 10 3}} {+ y y}}`

Step 3:  `'{with {y 7} {+ y y}}`

Step 4:  `'{+ 7 7}`

Step 5:  `'14`

---

Example 5:

`(define WAES5 '{with {x 5} {+ x {with {x 3} 10}}})`

Step 0: `'{with {x 5} {+ x {with {x 3} 10}}}`

Step 1:  `'{+ 5 {with {x 3} 10}}`

Step 2: `'{+ 5 10}`

Step 3:  `'15`

---

Example 6:

`(define WAES6 '{with {x 5} {+ x {with {x 3} x}}})`

Step 0: '`{with {x 5} {+ x {with {x 3} x}}}`

Step 1:  `'{+ 5 {with {x 3} x}}`

Step 2:  `'{+ 5 3}`

Step 3:  `'8`

---

Example 7:

`(define WAES7 '{with {x 5} {+ x {with {y 3} x}}})`

Step 0:  `'{with {x 5} {+ x {with {y 3} x}}}`

Step 1:  `'{+ 5 {with {y 3} 5}}`

Step 2:  `'{+ 5 5}`

Step 3:  `'10`

---

Example 8:

Step 0:

```other
'{with {y 9}
       {with {x y}
             {with {y 3}
                   {+ y x}}}}
```

Step 1:

```other
'{with {x 9}
      {with {y 3}
            {+ y x}}}
```

Step 2:

```other
'{with {y 3}
      {+ y 9}}
```

Step 3:

```other
'{+ 3 9}
```

Step 4:

```other
'12
```

Example 9:

Step 0:

```other
'{with {y 9}
      {with {y 3}
            {+ y y}}}
```

Step 1:

```other
'{with {y 3}
       {+ y y}}
```

Step 2:

```other
'{+ 3 3}
```

Step 3:

```other
'6
```

# F1WAE Language ( Lexical Scoping)

**Stepping Rules:**

   - A is of type AE which can be stepped further
   - B is of type AE which can be stepped further
   - C is of type AE which can be stepped further
   - v, v' and v" represent values

| F1W**AE Expression**                 | **Steps To**         | **Given That**                                                                                      |
| ------------------------------------ | -------------------- | --------------------------------------------------------------------------------------------------- |
| Addition and Subtraction expressions | Same as WAE language |                                                                                                     |
| **with** expressions                 | Same as WAE language |                                                                                                     |
| `{if0 A B C}`                        | `{if0 A' B C}`       | A steps to A’                                                                                       |
| `{if0 v B C}`                        | `B`                  | `v` = 0                                                                                             |
| `{if0 v B C}`                        | `C`                  | `v is not 0`                                                                                        |
| `{fun-name A}`                       | `{fun-name A’}`      | `A` steps to `A’`                                                                                   |
| `{fun-name v}`                       | `B’`                 | `B’` is fun-name’s body `B` with free instances of fun-name’s formal parameter substituted with `v` |

---

Error Cases:

| F1W**AE Expression** | **Steps To**                 | **Given That**             |
| -------------------- | ---------------------------- | -------------------------- |
| `{fun-name v}`       | ::unbound identifier error:: | fun-name is unbound        |
| `{if0 A B C}`        | runtime error                | `A` steps to runtime error |
| `{fun-name A}`       | runtime error                | `A` steps to runtime error |

---

**Stepping Examples:**

**Example 1:**

Step 0:  `'{if0 {+ 1 -1} {+ 9 1} 7}`

Step 1: `'{if0 0 {+ 9 1} 7}`

Step 2:  `'{+ 9 1}`

Step 3:  `'10`

---

**Example 2:**

Step 0:  `'{- {if0 {+ 1 -2} 9 7} 2}`

Step 1:  `'{- {if0 -1 9 7} 2}`

Step 2:  `'{- 7 2}`

Step 3:  `'5`

---

**Example 3**

Step 0:

```other
'{define-fn {double x} {+ x x}}
'{with {x 5} {double x}}
```

Step 1: `'{double 5}`

Step 2: `'{+ 5 5}`

Step 3: `'10`

---

**Example 4**

Step 0:

```other
'{define-fn {even x} {if0 x 1 {odd {- x 1}}}}
'{define-fn {odd x} {if0 x 0 {even {- x 1}}}}
'{even 3}
```

Step 1: `'{if0 3 1 {odd {- 3 1}}}`

Step 2: `'{odd {- 3 1}}`

Step 3: `'{odd 2}`

Step 4: `'{if0 2 0 {even {- 2 1}}}`

Step 5: `'{even {- 2 1}}`

Step 6: `'{even 1}`

Step 7: `'{if0 1 1 {odd {- 1 1}}}`

Step 8: `'{odd {- 1 1}}`

Step 9: `'{odd 0}`

Step 10: `'{if0 0 0 {even {- 0 1}}}`

Step 11: `'0`

---

Example 5:

Step 0:

```other
'{define-fn {f x} y}
'{with {y 7}
       {f 6}}
```

Step 1: `'{f 6}`

Step 2: `'y`  ::undefined variable error::

# FWAE Language (Lexical Scoping)

**Stepping Rules:**

   - `A` is of type FWAE which can be stepped further
   - `B` is of type FWAE which can be stepped further
   - v, v' and v" represent values

| FW**AE Expression**                  | **Steps To**           | **Given That**                                                                                            |
| ------------------------------------ | ---------------------- | --------------------------------------------------------------------------------------------------------- |
| Addition and Subtraction expressions | Same as WAE language   |                                                                                                           |
| **with** expressions                 | Same as WAE language   |                                                                                                           |
| **if0** expressions                  | Same as F1WAE language |                                                                                                           |
| `{A B}`                              | `{A’ B}`               | `A` steps to `A’`                                                                                         |
| `{v B}`                              | `{v B’}`               | `v is not 0`                                                                                              |
| `{v v’}`                             | `C’`                   | `v` is a function `{fun {x} C}` and `C’` is the body `C` with free instances of `x` substituted with `v’` |

---

#### runtime error ::= ::unbound identifier error:: | ::type error::

> #### ::type error cases::

- > We cannot add/subtract anonymous functions
- > We cannot apply a number as a function.

Error Cases:

| FW**AE Expression**       | **Steps To**       | **Given That**              |
| ------------------------- | ------------------ | --------------------------- |
| `{+ v v’}`  or  `{− v v}` | **::type error::** | `v` or `v’` is not a number |
| `{v v’}`                  | **::type error::** | `v` is not a function       |
| `{A B}`                   | **runtime error**  | `A` steps to runtime error  |
| `{v B}`                   | **runtime error**  | `B` steps to runtime error  |

---

**Stepping Examples:**

**Example 1:**

Step 0:

```other
'{with {double {fun {x} {+ x x}}}
       {with {x 5} {double x}}}
```

Step 1:

```other
'{with {x 5} {{fun {x} {+ x x}} x}} ; Substitute Free instances of double with {fun {x} {+ x x}}
```

Step 2:

```other
'{{fun {x} {+ x x}} 5} ;Substitute Free instances of x with 5
```

Step 3:

```other
'{+ 5 5} ;Pass in 5 as the argument to the function and replace the instances of formal parameter with 5
```

Step 4:

```other
'10 ;Compute the result of the addition
```

---

**Example 2:**

Step 0:

```other
'{with {k {fun {x} {fun {y} x}}}
      {{k 7} 9}}
```

Step 1:

```other
'{{{fun {x} {fun {y} x}} 7} 9}}
```

Step 2:

`'{{fun {y} 7} 9}`

Step 3:

`'7`

---

**Example 3:**

Step 0:

```other
'{with {y 9}
       {{with {y 7}
              {fun {x} y}} 8}}
```

Step 2:

```other
'{{with {y 7}
        {fun {x} y}} 8} ;Outer with doesn't replace anything since the y inside fun has local scope
```

Step 3:

```other
'{{fun {x} 7}} 8}
```

Step 3:

```other
'7
```

---

**Example 5:**

Step 0:

```other
'{if0 {fun {x} x}
      8
      0}
```

Step 2:

```other
'0 ; Since the function is not a zero, it yields the alternate branch
```

---

# FFWAE Language (Fix Stepping - Lexical Scope)

**Stepping Rules:**

   - A is of type WAE which can be stepped further
   - B is of type WAE which can be stepped further
   - v, v' represent values

| **FFWAE Expression**                 | **Steps To**              | **Given That**                                                   |
| ------------------------------------ | ------------------------- | ---------------------------------------------------------------- |
| Addition and Subtraction expressions | Same as WAE language      |                                                                  |
| **with** expressions                 | Same as FWAE language     |                                                                  |
| **if0** expressions                  | Same as FWAE language     |                                                                  |
| function application                 | Same as FWAE language     |                                                                  |
| `{fix x A}`                          | `A’`                      | `A’` is the body `A` with free `A’` instances of `x` substituted |
| `{fixFun f {x} A}`                   | `{fix f {fun {x} A}}`     |                                                                  |
| `{rec {x A} B}`                      | `{with {x {fix x A}} B }` |                                                                  |

---

**Stepping Examples:**

**Example 1:**

Step 0: `'{fix f 4}`

Step 1: `'4`

---

**Example 2:**

Step 0: `'{fix f f}`

Step 1:  `'{fix f f}`

Step 2:  `'{fix f f}`

Step 3:  `'{fix f f}`

...

---

**Example 3:**

Step 0: `'{fix a {fix b a}}`

Step 1:  `'{fix b {fix a {fix b a}}}`

`;; There isn't any free instance of b (the only b we have is in a binding position), we just do nothing`

Step 2:  `'{fix a {fix b a}}`

Step 3:  `'{fix b {fix a {fix b a}}}`

It keeps Alternating

---

**Example 4: (Recursive Example)**

Step 0: `'{fix f {fun {x} {if0 x 9 {f {- x 1}}}}}`

Step 1:

'`{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}`

---

**Example 5: (Recursive Example)**

Step 0:

```other
'{with {down {fix f {fun {x} {if0 x 9 {f {- x 1}}}}}}
       {down 1}}
```

Step 1:

```other
'{with {down {fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}}
       {down 1}}
```

Step 2:

```other
'{{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}} 1}
```

Step 3:

```other
'{if0 1 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- 1 1}}}
```

Step 4:

```other
'{{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- 1 1}}
```

Step 5:

```other
'{{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}
  {- 1 1}}
```

Step 6:

```other
'{{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}
  0}
```

Step 7:

```other
'{if0 0 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}
```

Step 8:

```other
'9
```

---

**Example 6:**

Step 0:

```other
'{with {down {fix f {fun {x} {if0 x 9 {f {- x 1}}}}}}
       {+ {down 1} {down 0}}}
```

Step 1:

```other
'{with {down {fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}}
       {+ {down 1} {down 0}}}
```

Step 2:

```other
'{+ {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 1}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 3:

```other
'{+ {if0 1 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- 1 1}}}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 4:

```other
'{+ {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- 1 1}}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 5:

```other
'{+ {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} {- 1 1}}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 6:

```other
'{+ {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 7:

```other
'{+ {if0 0 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- 0 1}}}
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 8:

```other
'{+ 9
    {{fun {x} {if0 x 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}} 0}}
```

Step 9:

```other
'{+ 9
    {if0 0 9 {{fix f {fun {x} {if0 x 9 {f {- x 1}}}}} {- x 1}}}}
```

Step 10:

```other
'{+ 9 9}
```

Step 11:

```other
'18
```

---

**Example 7:**

Step 0:

```other
'{with {down {fun {x}
                    {if0 x
                         0
                         {down {- x 1}}}}}
         {down 1}})
```

Step 1:

```other
'{{fun {x}
         {if0 x
              0
              {down {- x 1}}}} 1}
```

Step 2:

```other
'{if0 1
        0
        {down {- 1 1}}}
```

Step 3:

```other
'{down {- 1 1}}
```

Step 4:

```other
"Error ~ Undefined Function"
```

---

# FWAE Language (Dynamic Scoping)

**Stepping Rules:**

   - `A` is of type FWAE which can be stepped further
   - `B` is of type FWAE which can be stepped further
   - v, v' and v" represent values

| FW**AE Expression**                  | **Steps To**          | **Given That**                                            |
| ------------------------------------ | --------------------- | --------------------------------------------------------- |
| Addition and Subtraction expressions | Same as WAE language  |                                                           |
| **if0** expressions                  | Same as FWAE language |                                                           |
| `{with {x A} B}`                     | `{with {x A'} B}`     | `A` steps to `A’`                                         |
| `{with {x v} B}`                     | `{bind {x v} B}`      |                                                           |
| `{A B}`                              | `{A’ B}`              | `A` steps to `A’`                                         |
| `{v B}`                              | `{v B'}`              | `B` steps to `B'`                                         |
| `{v v’}`                             | `{bind {x v’} C}`     | `v` is `{fun {x} C}`                                      |
| `x`                                  | `v`                   | the innermost binding that surrounds `x` binds `x` to `v` |
| `{bind {x v} B}`                     | `{bind {x v} B’}`     | `B` steps to `B'`                                         |
| `{bind {x v} v’}`                    | `v’`                  |                                                           |

---

Error Cases:

| FW**AE Expression** | **Steps To**                     | **Given That**                                      |
| ------------------- | -------------------------------- | --------------------------------------------------- |
| `x`                 | **::Unbound Identifier error::** | no surrounding bind expression binds `x` to a value |

---

**Stepping Examples:**

**Example 1:**

Step 0:

```other
'{with {y 9}
       {{with {y 7}
              {fun {x} y}} 8}}
```

Step 1:

```other
'{bind {y 9}
       {{with {y 7}
              {fun {x} y}} 8}}
```

Step 2:

```other
'{bind {y 9}
       {{bind {y 7}
              {fun {x} y}} 8}}
```

Step 3:

```other
'{bind {y 9}
       {{fun {x} y} 8}}
```

Step 4:

```other
'{bind {y 9}
       {bind {x 8}
             y}}
```

Step 5:

```other
'{bind {y 9}
       {bind {x 8}
             9}}
```

Step 6:

```other
'{bind {y 9}
       9}
```

Step 7:

```other
'9
```

---

**Example 2:**

Step 0:

```other
'{define-fn {f x} y}
'{with {y 7}
       {f 6}}
```

Step 1:

```other
'{bind {y 7}
       {f 6}}
```

Step 2:

```other
'{bind {y 7}
       {bind {x 6}
             y}}
```

Step 3:

```other
'{bind {y 7}
       {bind {x 6}
             7}}
```

Step 4:

```other
'{bind {y 7}
       7}
```

Step 5:

```other
'7
```

---

**Example 3:**

Step 0:

# AE Language

# AE Language

# AE Language

# Concept Guide

# Parsing

# Interpreter Designs

