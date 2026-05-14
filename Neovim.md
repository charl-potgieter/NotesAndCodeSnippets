# Neovim

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


## General notes

 - Mason<br>
 It is possible to install programs with OS package manager or manually install.
 Mason however seems to simplify the process if one is happy ----------h the fact that
 programs are geittign installed and updatd via the text editor.
 - Linter
 - Type checker
 - Debuger
 - Reason why I am using both pyright and ruff



## Treesitter
 - Need to install treesitter-cli, otherwise checkhealth nvim-treesitter will
   throw an error
 - Note treesitter itself is a dependency of Neovim itself so should already be
   installed
