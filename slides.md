---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
# use UnoCSS (experimental)
css: unocss
---

# Développer des macros en Rust

Logan MAUZAIZE

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Logan MAUZAIZE
MonkeyPatch

---

# Rust
<br>

* Compilation native
* Ownership / Borrowing
* Sureté mémoire et concurrence
* Macro

---

# Macro
<br>

* `FnOnce(TokenStream)->TokenStream`

* Macros _déclaratives_ (Macro par l'exemple)

```rust
macro_rules! hello {
    ($e:expr) => {
        println!("Hello {}", $e);
    };
    () => {
        hello!("world");
    };
}
```

* Macros _procédurales_

  * Fonctions : `my_macro!(...)`
  * Dérivées : `#[derive(MyMacro)]`
  * Attributs : `#[MyMacro]`

---

# 101 Théorie des compilateurs
<br>

* `TokenStream` : Découpage en flux de tokens
* `TokenTree`: Organisation des tokens en arbre

---

# Live-code

* Cargo init (`proc-macro`, `syn`, `quote`)
* Blueprint (TDD)
* Arrange / Act / Assert
* Blueprint -> Test
* main entrypoint `builder_macro_derive` +  `syn::parse_macro_input!()` + secondary entrypoint
* tests + `parse_quote!()`
* model + generator
