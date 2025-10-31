---
layout: cover
background: ./img/title.png
---

# Comprendre les compilateurs
## Plus simple qu’il n’y paraît!

#### Philippe Vlérick - philippe.vlerick@worldline.com

---
layout: quote
---

# _Qu'est-ce qu'un compilateur?_

> ## un logiciel qui traduit un langage que je comprend en un langage que l'ordinateur comprend.

---
layout: quote
---

# _Plus formelle_

> ## un logiciel qui lit un programme dans un langage - le langage source - et le traduit en un logiciel équivalent dans un autre lagage - le langage cible

![Dragon Book](./img/dragon_book.jpg "Dragon Book")

<style>
img {display: block; margin: auto; height: 300px}
</style>

---
layout: cover
background: ./img/wizard.png
---

# Les étapes de la compilation

---
layout: fact
---

## Un compilateur exécute une séquence d'étapes qui transforment une représentation du logiciel source en une autre

```mermaid
flowchart LR
    source@{ shape: doc, label: "logiciel source"}
    steps@{ shape: processes, label: "étapes" }
    target@{ shape: doc, label: "logiciel cible"}
    source --> steps --> target
    style source fill:#00137F
    style target fill:#00137F
```

---
layout: fact
---

## Le résultat de chaque _étape_ est utilisé comme entrée de l'_étape_ suivante

```mermaid
flowchart TB
    subgraph step_n [Étape N]
    direction LR
    sourcen@{ shape: doc, label: "logiciel source"}
    stepn["étape"]
    targetn@{ shape: doc, label: "logiciel cible"}
    sourcen --> stepn --> targetn
    end
    subgraph step_o [Étape N+1]
    direction LR
    sourceo@{ shape: doc, label: "logiciel source"}
    stepo["étape"]
    targeto@{ shape: doc, label: "logiciel cible"}
    sourceo --> stepo --> targeto
    end
    step_n ~~~ step_o
    style sourcen fill:#00137F
    style targetn fill:#7F0000
    style sourceo fill:#7F0000
    style targeto fill:#00137F
```

---
layout: fact
--

## Prise individuellement, ces _étapes_ ne sont pas aussi intimidantes

---
layout: cover
background: ./img/wizard.png
---

# Scanning (ou _Lexing_)

---
layout: fact
---

## Consumes Characters, Produces Tokens

```mermaid
flowchart LR
    char_stream@{ shape: doc, label: "character stream"}
    scanner["Scanner"]
    token_stream@{ shape: doc, label: "token stream"}
    char_stream --> scanner --> token_stream
```

---

# Source Code

```c
char *foo = "bar";
```

---

```mermaid
flowchart TB
    subgraph char_stream [character stream]
    c~~~h~~~a~~~r~~~sp1["SP"]~~~star1[#42;]~~~f~~~etc@{ shape: braces, label: "..." }~~~sc1["#59;"]
    end
    subgraph token_stream [token stream]
    char~~~star2[#42;]~~~foo~~~eq2["#61;"]~~~str["#34;bar#34;"]~~~sc2["#59;"]
    end
    char_stream --> token_stream
```

---
layout: cover
background: ./img/wizard.png
---

# Parsing

---
layout: fact
---

## Consumes Token, Produces Abstract Syntax Tree (AST)

```mermaid
flowchart LR
    token_stream@{ shape: doc, label: "token stream"}
    parser["Parser"]
    syntax_tree@{ shape: doc, label: "Abstract Syntax Tree"}
    token_stream --> parser --> syntax_tree
```

---
layout: fact
---

```mermaid
flowchart TB
    subgraph token_stream [token stream]
    char~~~star2[#42;]~~~foo~~~eq2["#61;"]~~~str["#34;bar#34;"]~~~sc2["#59;"]
    end
    subgraph syntax_tree [syntax tree]
        direction TB
        a["assignment (=)"]
        u["unary (*)"]
        c["constant (#34;bar#34;)"]
        d["variable (foo)"]
        a --- u & c
        u --- d
    end
    token_stream --> syntax_tree
```

---
layout: cover
background: ./img/wizard.png
---

# _Compiler_ or _Interpreter_?

---
layout: fact
---

## A Compiler _translates_ a program

```mermaid
flowchart LR
    source@{ shape: doc, label: "source program"}
    compiler["Compiler"]
    target@{ shape: doc, label: "target program"}
    source --> compiler --> target
    style source fill:#00137F
    style target fill:#00137F
```

## ...but does not run it

---
layout: fact
---

## An Interpreter _runs_ a program from source

```mermaid
flowchart LR
    source@{ shape: doc, label: "source program"}
    input@{ shape: doc, label: "input"}
    interpreter["Interpreter"]
    output@{ shape: doc, label: "output"}
    source --> interpreter --> output
    input --> interpreter
    style source fill:#00137F
    style input fill:#00137F
    style output fill:#00137F
```

---
layout: cover
background: ./img/wizard.png
---

# Compilers' Structure

---
layout: fact
---

## _Three Phases Design_

```mermaid
flowchart LR
    source@{ shape: doc, label: "source code"}
    target@{ shape: doc, label: "machine code"}
    subgraph compiler [Compiler]
        direction LR
        frontend["Frontend"]
        optimizer["Optimizer"]
        backend["Backend"]
        frontend --> optimizer
        optimizer --> backend
    end
    source --> compiler
    compiler --> target
    style source fill:#00137F
    style target fill:#00137F
```

---
layout: normal
---

# Frontend

- Scanning/_Lexing_
- Parsing
- Semantic Analysis
- Conversion to Intermediate Representation

---
layout: normal
---

# Optimizer

- Dead Code Elimination
- Inlining
- Constant Folding
- Constant Propagation
- Loop Unrolling

---
layout: normal
---

# Backend

- Machine Code Generation
  - Target-specific Instructions
  - Target-specific Optimizations

---
layout: fact
---

## LLVM Example

```mermaid
flowchart LR
    source_c@{ shape: doc, label: ".c/.cpp"}
    source_swift@{ shape: doc, label: ".swift"}
    source_rust@{ shape: doc, label: ".rs"}
    target_x86@{ shape: doc, label: "x86"}
    target_arm@{ shape: doc, label: "ARM"}
    subgraph frontend [Frontend]
        direction TB
        clang["_clang_"]
        swift["_swiftc_"]
        rust["_rustc_"]
    end
    subgraph optimizer [Optimizer]
        optimizers@{ shape: processes, label: "optimizers" }
    end
    subgraph backend [Backend]
        direction TB
        x86
        ARM
    end
    source_c --> clang
    source_swift --> swift
    source_rust --> rust
    clang & swift & rust -- .ll --> optimizer
    optimizer --> x86 & ARM
    x86 --> target_x86
    ARM --> target_arm
    style source_c fill:#00137F
    style source_swift fill:#00137F
    style source_rust fill:#00137F
    style target_x86 fill:#00137F
    style target_arm fill:#00137F
```

---
layout: fact
---

## JVM Example

```mermaid
flowchart LR
    source_java@{ shape: doc, label: ".java"}
    source_kotlin@{ shape: doc, label: ".kt"}
    source_scala@{ shape: doc, label: ".scala"}
    target_x86@{ shape: doc, label: "x86"}
    target_arm@{ shape: doc, label: "ARM"}
    subgraph frontend [Frontend]
        direction TB
        java["_javac_"]
        kotlin["_kotlinc_"]
        scala["_scalac_"]
    end
    subgraph optimizer [Optimizer]
    direction TB
        jit_op[JIT]
    end
    subgraph backend [Backend]
    direction TB
        jit_be[JIT]
    end
    source_java --> java
    source_kotlin --> kotlin
    source_scala --> scala
    java & kotlin & scala -- .class --> optimizer
    optimizer --> backend
    backend --> target_x86 & target_arm
    style source_java fill:#00137F
    style source_kotlin fill:#00137F
    style source_scala fill:#00137F
    style target_x86 fill:#00137F
    style target_arm fill:#00137F
```

---
layout: cover
background: ./img/wizard.png
---

# Conclusion

---
layout: normal
--- 

# Compilers are Software
- They can be understood one step at the time
- You could write a simple one yourself, there is no magic

---
layout: normal
--- 

# If you want to learn more by doing
- https://craftinginterpreters.com/

![Crafting Interpreters](./img/crafting_interpreters.png "Crafting Interpreters")
