# ocaml project cheatsheet

Notes from when I went on [Dillon Mulroy's Twitch stream][dmmulroy] to talk about [getting started with OCaml][getting-started-with-ocaml].

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
