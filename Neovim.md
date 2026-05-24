# Neovim

General notes on Neovim and my setup

## TODO
- Look at pyproject.toml in Aus school maths and note 
[tool.poetry.group.dev.dependencies] section
- Also see 
[tool.ruff]
line-length = 79


 - What is vulture and how and why did I install it
 - Note how vulture and depugpy are listed as dependiencies in the virtual
   poetry environment
 - Should debugpy  be installed by Mason rather than the pacman?
 - Same for treesitter
 - Write a section on folder structure

#### Lazy.nvim
 - My current choice of package manager for neovim
 - It fetches code directly from github
 - Lazy.nvim manages neovim plugins - it does not install binaries 
 - Lazy.nvim is installed by relevant code in below directories - it is
   generally not installed by the OS package manager
     - ~/.config/nvim/init.lua
     - ~/.config/nvim/lua/config/lazy.lua
 - Lazy.nvim, like Lua itself, uses Lua as a scripting language for setup


 #### Mason
 - mason.nvim is a Neovim plugin that allows you to easily manage external
   editor tooling  editor tooling such as LSP servers, DAP servers, linters,and
   formatters through a single interface.
 - It manages external CLI (command line interface) binaries
 - Unlike an OS package manager the install is not system-wide but is resricted
    to Neovim (which makes it easier to switch the neovim config between
    different computers or operating systems)
 - Packages installed via Mason can be manually installed or via the OS package
   manager, but Mason provides a convenient single point of installation and
   limits install scope to Neovim
 - Mason itself can be installed using Lazy.nvim

#### Language Server Protocol (LSP)
 - The Language Server Protocol (LSP) is an open, JSON-RPC-based protocol for use
between source-code editors or integrated development environments (IDEs) and
servers that provide "language intelligence tools": programming
language-specific features like code completion, syntax highlighting and marking
of warnings and errors, as well as refactoring routines. The goal of the
protocol is to allow programming language support to be implemented and
distributed independently of any given editor or IDE.
 - Python LSP examples include pyright, basedpyright and ruff.  Note that these
   tools sometimes play dual roles for example pyright is first and foremost a
   static type checker but also takes on the role as LSP in Neovim.
 - nvim-lspconfig is a collection of LSP server configurations for the Nvim LSP
   client.
<!-- - TODO:   How does this interact with the individual LSP plugin setups? --> 

#### Parser
 - A parser takes input data (typically text) and builds a data structure often
   some kind kind of parse tree, abstract syntax tree or other hierarchical
   structure, giving a structural representation of input while checking for the
   correct syntax.
 - Practically above enables the parser to perform below functions:
    - improved syntax higlighting over the basic regex syntax higlighting that
      could otherwise be perfored by the editor.
    - code folding
    - smarter code navigation and indentation
    - Using a companion plugin like nvim-treesitter-textobjects, you can use
      intuitive keymaps to select, delete, or change entire structures.
 - I am currently using nvim-treesitter as a parser.  This requires the
   installation of treesitter-cli which can be installed via Mason

 ### Expand
 - Linter
 - Type checker
 - Debuger
 - Reason why I am using both pyright and ruff



#### Treesitter
 - TODO: Maybe better installed via Mason rather than Pacman
 - Need to install treesitter-cli, otherwise checkhealth nvim-treesitter will
   throw an error
 - Note treesitter itself is a dependency of Neovim itself so should already be
   installed

