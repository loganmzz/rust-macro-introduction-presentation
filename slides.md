---
info: |
  Présentation concernant le développement de macros en Rust
  Voir https://github.com/loganmzz/rust-macro-introduction-presentation
theme: default
highlighter: shiki
lineNumbers: false
css: unocss
---

# Développer des macros en Rust

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-presentation"/>

---

# Logan MAUZAIZE
MonkeyPatch - Expert DevOps

<Logos>
  <Logo name="terraform"/>
  <Logo name="kubernetes"/>
  <Logo name="ansible"/>
</Logos>

<QRLink url="https://www.monkeypatch.io/"/>

---
layout: two-cols-header
---

# Logan MAUZAIZE
Auteur de Macon

::left::

<Logo name="macon"/>

::right::

<QRLink url="https://www.monkeypatch.io/blog/2024-02-06-rust-macon-new-derive-builder"/>

---
layout: section
---

# 101 Macro

---
layout: two-cols
---

# 101 Macro - Rust
<br>

* Compilation native
* Ownership / Borrowing
* Sureté mémoire et concurrence
* Macro
* ...

<QRLink url="https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=8d04a7c5d467aa3f88ea450d741cc4aa"/>

::right::

```rust
let (ping, receiver) = sync_channel(1);

let pong = ping.clone();
thread::spawn(move|| {
    for i in 0..3 {
        thread::sleep(Duration::from_millis(10));
        pong.send("Pong !").unwrap();
    }
});

thread::spawn(move|| {
    for i in 0..3 {
        ping.send("Ping !").unwrap();
        thread::sleep(Duration::from_millis(10));
    }
});


while let Ok(value) = receiver.recv() {
    println!("{}", value);
}
println!("Complete");
```

---

# 101 Macro - Principes
<br>

```rust
FnOnce(TokenStream) -> TokenStream
```

* Fonctions

```rust
my_macro!(...)
```

* Dérivées

```rust
#[derive(MyMacro)]
```

 * Attributs

```rust
#[MyMacro]
```

---

# 101 Macro - Déclaratives
_ou Macro par l'exemple_

```rust
macro_rules! hello {
    ($e:expr) => {
        println!("Hello {}", $e);
    };
    () => {
        hello!("world");
    };
}

hello!(); // Print "Hello world"
hello!("you"); // Print "Hello you"
```

<!--
  * Pattern matching
  * Fonction uniquement
-->

---

# 101 Macro - Procédurales
<br>

```rust
/// Fonction
#[proc_macro]
pub fn hello(tokens: TokenStream) -> TokenStream { ... }

/// Dérivée
#[proc_macro_derive(Hello)]
pub fn derive_hello(tokens: TokenStream) -> TokenStream { ...  }

/// Attribut
#[proc_macro_attribute]
pub fn helloize(attr: TokenStream, tokens: TokenStream) -> TokenStream { ... }

```

---
layout: section
---

# 101 Théorie des compilateurs

---

# 101 Compilateur - Texte
<br>

<Tokens>
  <Token type="Ident">pub</Token>
  <Token type="Ident">fn</Token>
  <Token type="Ident">hello</Token>
  <Token type="Group">
    <Token type="Delimiter">(</Token>
    <Token type="Ident">name</Token>
    <Token type="Punct">:</Token>
    <Token type="Ident">Option</Token>
    <Token type="Punct">&lt;</Token>
    <Token type="Ident">String</Token>
    <Token type="Punct">&gt;</Token>
    <Token type="Delimiter">)</Token>
  </Token>
  <Token type="Group">
    <Token type="Delimiter">{</Token>
    <Token type="Delimiter">}</Token>
  </Token>
</Tokens>

---

# 101 Compilateur - Découpage
<br>

<Tokens format="cut">
  <Token type="Ident">pub</Token>
  <Token type="Ident">fn</Token>
  <Token type="Ident">hello</Token>
  <Token type="Group">
    <Token type="Delimiter">(</Token>
    <Token type="Ident">name</Token>
    <Token type="Punct">:</Token>
    <Token type="Ident">Option</Token>
    <Token type="Punct">&lt;</Token>
    <Token type="Ident">String</Token>
    <Token type="Punct">&gt;</Token>
    <Token type="Delimiter">)</Token>
  </Token>
  <Token type="Group">
    <Token type="Delimiter">{</Token>
    <Token type="Delimiter">}</Token>
  </Token>
</Tokens>

---

# 101 Compilateur - Tokenisation
<br>

<Tokens format="decorate">
  <Token type="Ident">pub</Token>
  <Token type="Ident">fn</Token>
  <Token type="Ident">hello</Token>
  <Token type="Group">
    <Token type="Delimiter">(</Token>
    <Token type="Ident">name</Token>
    <Token type="Punct">:</Token>
    <Token type="Ident">Option</Token>
    <Token type="Punct">&lt;</Token>
    <Token type="Ident">String</Token>
    <Token type="Punct">&gt;</Token>
    <Token type="Delimiter">)</Token>
  </Token>
  <Token type="Group">
    <Token type="Delimiter">{</Token>
    <Token type="Delimiter">}</Token>
  </Token>
</Tokens>

<div class="legend">
    <div>Legend:</div>
    <Tokens format="decorate">
      <Token type="Ident">Ident</Token>
      <Token type="Delimiter">Delimiter</Token>
      <Token type="Punct">Punct</Token>
      <Token type="Group">Group</Token>
    </Tokens>
</div>

---

# 101 Compilateur - AST
<br>

<Tokens>
  <Token type="Ident">pub</Token>
  <Token type="Ident">fn</Token>
  <Token type="Ident">hello</Token>
  <Token type="Group">
    <Token type="Delimiter">(</Token>
    <Token type="Ident">name</Token>
    <Token type="Punct">:</Token>
    <Token type="Ident">Option</Token>
    <Token type="Punct">&lt;</Token>
    <Token type="Ident">String</Token>
    <Token type="Punct">&gt;</Token>
    <Token type="Delimiter">)</Token>
  </Token>
  <Token type="Group">
    <Token type="Delimiter">{</Token>
    <Token type="Delimiter">}</Token>
  </Token>
</Tokens>

```yaml
- kind: "function"
  visibility: "pub"
  signature:
    name: "hello"
    arguments:
    - name: "name"
      type:
        name: "Option"
        arguments:
        - name: String
    return: null
  block: []
```

---
layout: section
---

# Demo

---

# Demo

* Derive Data
* Impl `Default` + `Debug` + getters
* Type `String` + `usize` + `bool`

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code"/>

---

# Demo - 00 - Initialisation

* `cargo init`
* `proc-macro2`, `quote`, `syn`

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/00-init"/>

---

# Demo - 01 - Blueprint

* `impl ::std::default::Default`

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/01-blueprint"/>

---

# Demo - 02 - impl `Default`

* `quote::quote!`
* `syn::Fields::Named`, `syn::Fields::Unnamed`
* `proc_macro2::Group`

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/02-impl-default"/>

---

# Demo - 03 - Modules

```rust
mod model;
mod parser;
mod generator;
```

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/03-modules"/>

---

# Demo - 04 - impl `Debug`

* `impl ::std::fmt::Debug`
* `debug_struct()`, `debug_tuple()`
* `proc_macro2::Literal`
* `quote::format_ident`

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/04-debug"/>

---

# Demo - 05 - Attribut

* `#[proc_macro_derive(attributes(...))]`
* `Attribute::parse_nested_meta`
* [`darling`](https://crates.io/crates/darling)


<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/05-attribute"/>

---

# Demo - 06 - Gestion des erreurs

* `syn::Result`
* `syn::Error::into_compile_error`


<QRLink url="https://github.com/loganmzz/rust-macro-introduction-code/tree/06-errors"/>

---
layout: section
---

# Pour aller plus loin

---

# Pour aller plus loin - Bilan de la démo

* ⚠️ NE PAS FAIRE les étapes une à une
* 😄 A FAIRE implémenter les getters
* 🚑 A CREUSER l'assainissement (visibilité, features, ...)
* 🕵️ A ÉTUDIER parser des struct via `syn::parse`
* 📄 IDÉES gérer des fichiers de configuration (DRY)
* 🧪 TESTER ! TESTER ! TESTER !

---

# Pour aller plus loin - Liens

https://doc.rust-lang.org/book/ch19-06-macros.html

https://ferrous-systems.com/blog/testing-proc-macros/

https://github.com/dtolnay/proc-macro-workshop

https://github.com/dtolnay/case-studies/tree/master

https://developerlife.com/2022/03/30/rust-proc-macro/

---
layout: two-cols-header
---

# Questions ?

::left::

![Do you have questions?](/img/question.gif)

::right::

<QRLink url="https://github.com/loganmzz/rust-macro-introduction-presentation"/>
