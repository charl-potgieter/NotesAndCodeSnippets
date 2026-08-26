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
 - Write a section on folder structure
 - See "Updating" section below and consider as this seems like a lot of
   different areas that need to be updated

#### Lazy.nvim
 - My current choice of package manager for neovim
 - It fetches code directly from github
 - Lazy.nvim manages neovim plugins - it does not install binaries 
 - Lazy.nvim is installed by relevant code in below directories - it is
   generally not installed by the OS package manager
     - ~/.config/nvim/init.lua
     - ~/.config/nvim/lua/config/lazy.lua
 - Lazy.nvim, like Neovim itself, uses Lua as a scripting language for setup
 - There is a new plugin manager which is now native to Neovim called vim.pack.
   Potentially still lacking too many features, or requires complexity tyo
   implement anythign other than basic setup.   Keep an eye on development
   status though.


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
 - The Language Server Protocol (LSP) is an open, JSON-RPC-based protocol for
   use between source-code editors or integrated development environments (IDEs)
   and servers that provide "language intelligence tools"
 - The goal of the protocol is to allow programming language support
   to be implemented and distributed independently of any given editor or IDE.
 - LSP features include:
     - go-to-definition
     - find references
     - hover
     - completion
     - rename
     - format
     - refactor
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
 - A parser helps other tools understand the code.  Linters and formatters (see
   below) may use a parser to build the abstract syntax tree (AST) to enable
   them to perform their functions.
 - I am currently utilising treesitter as a parser (as of Aug 2026 there are not
   any genuine alternatives)



#### Treesitter
 - As of Neovim 0.12, the treesitter library is directy implemented into Neovim 
 - There are however only a limited number of parsers that are natively included
   in Nvim (there is no parser for python for example)
 - A plugin such as https://github.com/nvim-treesitter/nvim-treesitter is
   required to install additional parsers
 - As at August 2026 the nvim-treestitter github page states that treesitter-cli
   is required to be installed by the systems package manager (not npm)

#### Linter

 - A linter is a static code analysis tool that inspects source code to
   flag potential errors, syntax bugs, bad practices, and formatting style
   issues
 - Linters perform below functions:
     - Catches Syntax Errors & Bugs Early: Identifies typos, undefined variables,
     unreachable code, or missing imports before you compile or execute your
     program.

    - Enforces Code Style & Guidelines: Ensures code matches established standards
    (like PEP 8 in Python or standard rules in JavaScript) regarding naming
    conventions, unused code, or line lengths.

    - Improves Security & Maintainability: Warns about dangerous code patterns (e.g.,
    hardcoded passwords, unsafe function calls, or memory leaks).

    - Ruff is my current choice of linter for python (note that ruff
   performs dual roles - both fomratting and linting)

#### Code formatter

-  Code formatters focus on visual presentation and style compliance.

- Specific functions include:
     - Indentation and spacing
     - Line length and wrapping
     - Import and declaration sorting
     - Quote and syntax consitency

 - Ruff is my current choice of code formatter for python (note that ruff
   performs dual roles - both fomratting and linting)

#### Type checker

- A type checker verifies that data types are are being used correctly and
  consistently throughout your codebase—without actually running your code.
- Examples of responsibilities: 
    - Type Verification: Ensures you aren't passing invalid data types into
      functions (e.g., passing a string to a math function expecting an int).
    - Signature Checking: Ensures functions return the exact types they claim to
      return and receive all required arguments.
    - Type Inference: Traces values and control flow through your code to infer
      the type of variables, even if you haven't explicitly written type hints
      for every line.
    - Catching Null/Undefined Errors: Flags code paths where an object might
      unexpectedly be None/null/undefined before performing an operation on it.
- Pyright (by microsoft) is my current preferred python type checker.  Pyrefly
  (meta) is faster but is generally regarded as less accurate.  Ty (by Astral)
  is faster but still in beta Aug 26 and may be worth reviewing in 2027.


#### Debugging in python
<!-- TODO:  -->
- Need to add general notes re nvim-dap etc 


#### Debugpy installation

 - There are a few options
     - install via package manager
     - install via Mason
     - install via python project / package manager such as poetry or UV
 - I am currently leaning towards a Mason install.  
 - I don't see any need to for project level install with poetry or UV as each
   time I want to use a debugger I will need to list as a dependency in poetry
   or UV.
 - Mason seems to be a clean approach in keeping Neovim related binaries bundled
   without poluting the global environment.


#### Updating 
<!-- TODO: Consider -->
With my current setup there seems to be a lot of areas where updates need to be
run:
 - pacman for system wide installs
 - Lazy for neovim plugins
 - Mason for Neovim related binaries
 - Poetry to update python libraries - should I be doing this?  I recently ran
   into an issue where pandas import generated an error despite it having
   previously work and correct specification in pyproject.toml.  Potentially the
   issue was caused by an update to python.  Should I try and control the
   version of python that is being used?  Makes sense to use  the latest if
   possible?






 ### Expand
 - Debuger
 - Reason why I am using both pyright and ruff and how to handle settings to
   prevent overlap in functionalities.

