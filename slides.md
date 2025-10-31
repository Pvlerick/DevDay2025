---
layout: cover
background: ./img/title.png
---

# Comprendre les compilateurs
## Plus simple qu’il n’y paraît!

<br />
<br />

#### Philippe Vlérick - philippe.vlerick@worldline.com

![Dragon Book](./img/Logo_Worldline_-_2021.svg.png "Worldline")

<style>
img {display: block; margin: auto; height: 100px;}
</style>

---
layout: quote
---

# _Qu'est-ce qu'un compilateur?_

> ## un logiciel qui traduit un langage que je comprends en un langage que l'ordinateur comprend.

---
layout: quote
---

# _Plus formelle_

> ## un logiciel qui lit un logiciel dans un langage - le langage source - et le traduit en un logiciel équivalent dans un autre langage - le langage cible

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
---

## Prise individuellement, ces _étapes_ ne sont pas aussi intimidantes

---
layout: cover
background: ./img/wizard.png
---

# Scanning (ou _Lexing_)

---
layout: fact
---

## Caractères -> _Tokens_

```mermaid
flowchart LR
    char_stream@{ shape: doc, label: "flux de caractères"}
    scanner["Scanner"]
    token_stream@{ shape: doc, label: "flux de _tokens_"}
    char_stream --> scanner --> token_stream
```

---

# Code source

```csharp
var name = "foo";
```

---

```mermaid
flowchart TB
    subgraph char_stream [flux de caractères]
    v~~~a~~~r~~~sp1["#60;SP#62;"]~~~n~~~etc@{ shape: braces, label: "..." }~~~o~~~qt["#34;"]~~~sc1["#59;"]
    end
    subgraph token_stream [flux de _tokens_]
    var~~~name~~~eq["#61;"]~~~str["#34;foo#34;"]~~~sc2["#59;"]
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

## _Tokens_ -> _Abstract Syntax Tree_ (AST)

```mermaid
flowchart LR
    token_stream@{ shape: doc, label: "flux de _tokens_"}
    parser["Parser"]
    syntax_tree@{ shape: doc, label: "Abstract Syntax Tree"}
    token_stream --> parser --> syntax_tree
```

---
layout: fact
---

```mermaid
flowchart TB
    subgraph token_stream [flux de _tokens_]
    var~~~name~~~eq["#61;"]~~~str["#34;foo#34;"]~~~sc2["#59;"]
    end
    subgraph syntax_tree [Abstract Syntax Tree]
        direction TB
        a["assignation (=)"]
        c["constante (#34;foo#34;)"]
        d["variable (name)"]
        a --- d & c
    end
    token_stream --> syntax_tree
```

---
layout: cover
---

# Demo

---
layout: cover
background: ./img/wizard.png
---

# _Compilateur_ ou _Interpréteur_?

---
layout: fact
---

## Un compilateur _traduit_ un logiciel

```mermaid
flowchart LR
    source@{ shape: doc, label: "logiciel source"}
    compiler["Compilateur"]
    target@{ shape: doc, label: "logiciel cible"}
    source --> compiler --> target
    style source fill:#00137F
    style target fill:#00137F
```

## ...mais ne l'exécute pas

---
layout: fact
---

## Un interpréteur _exécute_ un logiciel à partir des sources

```mermaid
flowchart LR
    source@{ shape: doc, label: "logiciel source"}
    input@{ shape: doc, label: "entrée"}
    interpreter["Interpréteur"]
    output@{ shape: doc, label: "sortie"}
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

# Structure d'un compilateur

---
layout: fact
---

## _Design en trois phase_

```mermaid
flowchart LR
    source@{ shape: doc, label: "logiciel source"}
    target@{ shape: doc, label: "instructions machine"}
    subgraph compiler [Compilateur]
        direction LR
        frontend["_Frontend_"]
        optimizer["_Optimisateur_"]
        backend["_Backend_"]
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
- "Desugaring"
- Analyse sémantique
- Conversion en représentation intermédiaire (IR)

---
layout: normal
---

# Optimisateur

- _Dead Code Elimination_
- _Inlining_
- _Constant Folding_
- _Constant Propagation_
- _Loop Unrolling_
- ...

---
layout: normal
---

# Backend

- Génération du code machine
  - Instruction spécifiques à la plateforme cible
  - Optimisations spécifiques à la plateforme cible

---
layout: fact
---

## Exemple: LLVM

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
    subgraph optimizer [Optimisateurs]
        optimizers@{ shape: processes, label: "optimisateur" }
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

## Exemple: _dotnet_

```mermaid
flowchart LR
    source_cshap@{ shape: doc, label: ".cs"}
    source_vb@{ shape: doc, label: ".vb"}
    source_fsharp@{ shape: doc, label: ".fs"}
    target_x86@{ shape: doc, label: "x86"}
    target_arm@{ shape: doc, label: "ARM"}
    subgraph frontend [Frontend]
        direction TB
        csharp["_csc.dll_"]
        vb["_vb.dll_"]
        fshap["_fsc.dll_"]
    end
    subgraph optimizer [Optimisateur]
    direction TB
        jit_op[JIT]
    end
    subgraph backend [Backend]
    direction TB
        jit_be[JIT]
    end
    source_cshap --> csharp
    source_vb --> vb
    source_fsharp --> fshap
    csharp & vb & fshap -- .dll --> optimizer
    optimizer --> backend
    backend --> target_x86 & target_arm
    style source_cshap fill:#00137F
    style source_vb fill:#00137F
    style source_fsharp fill:#00137F
    style target_x86 fill:#00137F
    style target_arm fill:#00137F
```

---
layout: cover
background: ./img/wizard.png
---

# _Desugaring_

---
layout: fact
---

## Transformer des instructions de _haut niveau_ en instructions plus simple (et souvent plus longues) du même langage

---
layout: cover
background: ./img/wizard.png
---

# Optimisations

---
layout: normal
---

- _Constant Folding_
- _Constant Propagation_
- _Dead code elimination_

---
layout: cover
background: ./img/wizard.png
---

# Conclusion

---
layout: normal
--- 

# Les compilateurs sont des logiciels
- Ils peuvent être compris, pas à pas
- Vous pourriez en écrire un vous-même, il n'y a aucune magie

---
layout: normal
--- 

# Pour aller plus loin - en faisant
- https://craftinginterpreters.com/

![Crafting Interpreters](./img/crafting_interpreters.png "Crafting Interpreters")

---
layout: center
---

![Feedback](./img/feedback.png "Feedback QR code")

<style>
img {display: block; margin: auto; height: 300px;}
.slidev-layout {height: 100%; background: #E0E0E0;}
</style>
