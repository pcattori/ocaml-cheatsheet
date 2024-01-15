# ocaml cheatsheet

Notes from when I went on [Dillon Mulroy's Twitch stream](https://www.twitch.tv/dmmulroy) to talk about [getting started with OCaml](https://www.youtube.com/watch?v=FtI5hxDcVKU).

## learning

The official ["Learn OCaml"](https://ocaml.org/docs) are always improving, but here are some other resources I found invaluable when starting out.

#### Real World OCaml

[👉](https://dev.realworldocaml.org/)

Highlights : tools and techniques

- [Command-line parsing](https://dev.realworldocaml.org/command-line-parsing.html)
- [Error handling](https://dev.realworldocaml.org/error-handling.html)
- [Maps and hash tables](https://dev.realworldocaml.org/maps-and-hashtables.html)
- [Serialization](https://dev.realworldocaml.org/data-serialization.html)
- [JSON](https://dev.realworldocaml.org/json.html)
- [Testing](https://dev.realworldocaml.org/testing.html)

#### OCaml Programming: Correct + Efficient + Beautiful

[👉](https://cs3110.github.io/textbook/cover.html#)

Highlights : programming language implementation and theory

- [Implementing an interpreter in OCaml](https://cs3110.github.io/textbook/chapters/interp/intro.html)
- [The Curry-Howard Correspondence](https://cs3110.github.io/textbook/chapters/adv/curry-howard.html)

## project setup

```sh
dune init proj <name>
cd <name>
opam switch create . --deps-only
```

Minimal `dune-project` for CLIs:

```lisp
(lang dune 3.11)

(generate_opam_files true)

(package
 (name <name of your cli>)
 (depends ocaml dune))
```

#### git

Initialize a git repo:

```sh
git init
```

Then add a `.gitignore`:

```txt
_build/
_opam/
```

#### opam switch

Setup `opam` to auto-enable the switch when entering the directory:

```sh
opam init --enable-shell-hook
```

Or do this every time:

```sh
eval $(opam env)
```

#### dev dependencies

```sh
opam install ocaml-lsp-server odoc ocamlformat utop
```

## neovim

Do not do the `rtp` thing that `opam` suggests.
Instead, do this if using `lazy.nvim` as your package manager:

```lua
-- ~/.config/nvim/lua/plugins/ocaml.lua

return {
  {
    "neovim/nvim-lspconfig",
    opts = {
      servers = {
        -- use local lsp installed within project's opam switch
        ocamllsp = { mason = false },
      },
    },
  },
  {
    "nvim-treesitter/nvim-treesitter",
    opts = function(_, opts)
      if type(opts.ensure_installed) == "table" then
        vim.list_extend(opts.ensure_installed, { "ocaml", "ocaml_interface" })
      end
    end,
  },
}
```

[dmmulroy]: https://www.twitch.tv/dmmulroy
[getting-started-with-ocaml]: https://www.youtube.com/watch?v=FtI5hxDcVKU
[learn-ocaml]: https://ocaml.org/docs
