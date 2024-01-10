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

<!--
-->

---

# Logan MAUZAIZE
MonkeyPatch

---
layout: section
---

# 101 Macro

---

# 101 Macro - Rust
<br>

* Compilation native
* Ownership / Borrowing
* Sureté mémoire et concurrence
* Macro
* ...

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

* `TokenStream` : Découpage en flux de tokens
* `TokenTree`: Organisation des tokens en arbre

---

# 101 Compilateur - tokenisation
<br>


---

# Live-code

* Cargo init (`proc-macro`, `syn`, `quote`)
* Blueprint (TDD)
* Arrange / Act / Assert
* Blueprint -> Test
* main entrypoint `builder_macro_derive` +  `syn::parse_macro_input!()` + secondary entrypoint
* tests + `parse_quote!()`
* model + generator
