---
title: "Como Configurar Neovim Como VsCode"
date: 2020-12-06T20:55:47-06:00
description: El día de hoy vamos a instalar y a configurar Neovim para utilizarlo como IDE para programar en Python. Vamos a utilizar algunos "Plugins" para obtener las ventajas que cualquier otro IDE tiene, tales como validación de sintaxis y autocompletado.
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
  - vim
  - neovim
  - python
  - ide
series:
  -
categories:
  - neovim
image: /images/posts/neovim-python/neovim-logo.png
---

## Introducción

<a href="https://neovim.io/" target="_blank">Neovim</a> es VIM pero mejorado. La documentación oficial lo define como:

> Neovim es un editor de texto basado en Vim diseñado para la extensibilidad y usabilidad, para fomentar nuevas aplicaciones y contribuciones.

## Instalación

### Prerequisitos

Como lo dice el título de este artículo, me estaré concentrando en configurar Neovim como IDE para python, entonces voy a asumir que ya tienes Python y Pip instalados en tu sistema.

#### Python, Pip y Pipenv

Para verificar que tengas Python instalado corre uno de los siguientes comandos en la consola:

```bash
$ python --version
$ python3 --version
```

El comando anterior deberá decirte la versión de Python instalada en tu sistema

Para verificar si Pip está instalado, solo ejecuta pip en la consola. El comando deberá regresar la "ayuda".

```bash
$ pip

Usage:
  pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
  check                       Verify installed packages have compatible dependencies.
  config                      Manage local and global configuration.
  search                      Search PyPI for packages.
  cache                       Inspect and manage pip's wheel cache.
  wheel                       Build wheels from your requirements.
  hash                        Compute hashes of package archives.
  completion                  A helper command used for command completion.
  debug                       Show information useful for debugging.
  help                        Show help for commands.
```

Si no tienes instalado pip, lo puedes instalar ejectando:

```bash
$ sudo apt-get install python3-pip
```

Para verificar si Pipenv está instalado, puedes ejecutar:

```bash
$ pipenv --version

pipenv, version 2020.11.15
```

Para instalar Pipenv, utiliza el siguiente comando:

```bash
$ pip3 install pipenv
```

#### Neovim

Por supuesto, debemos tener instalado Neovim con soporte para python, si no lo tienes aún, lo puedes instalar de la siguiente manera:

```bash
$ sudo apt-get install neovim python3-neovim
```

#### Node.js

Para poder instalar los plugins que nos van a permitir utilizar Neovim como IDE y reemplazar de una vez por todas VsCode, es necesario también instalar Node.js.

```bash
$ sudo apt-get install npm
```

#### Nerdfont (Tipo de Letra)

Para poder visualizar los iconos en la terminal es necesario tener un tipo de letra que lo soporte, en lo personal a mi me gustan las Nerdfonts.

Puedes descargar <a href="https://github.com/ryanoasis/nerd-fonts/releases/download/v2.1.0/DejaVuSansMono.zip">DejaVuSansMono Nerd Font</a>.

Una vez que hayas descargado el archivo, descomprimelo y copia los archivos (\*.ttf) al siguiente directorio:

```
$HOME/.local/share/fonts/
```

Si por alguna razón no tienes el directorio, lo puedes crear.

Ya que hayas copiado los archivos, para poder utilizar este nuevo tipo de letra, debes ejecutar:

```bash
$ fc-cache -fv
```

**Asegurate de configurar este nuevo tipo de letra en la terminal que estés utilizando:**

![Terminal](/images/posts/neovim-python/nerdfont-terminal.png)

## Configuración

### Directorio nvim

Aunque puedes tener toda la configuración en tu archivo **init.vim** yo prefiero tener archivos separados para una mejor organización y fácil manejo.

Crea la siguiente estructura de direcorios:

```
📂 $HOME/.config/nvim/
├──📁 general
├──📁 keymapping
├──📁 nvim-plugins
└──📁 plug-config
```

```bash
mkdir $HOME/.config/nvim
mkdir $HOME/.config/nvim/general
mkdir $HOME/.config/nvim/keymapping
mkdir $HOME/.config/nvim/nvim-plugins
mkdir $HOME/.config/nvim/plug-config
```

### Ambiente de Python para Neovim

Vamos a crear un ambiente virtual dentro de la carpeta de configuración de Neovim. Para eso, en la terminal tecleamos lo siguiente:

```bash
cd ~/.config/nvim

pipenv install
```

Copia la ruta donde se creó el ambiente virtual pues lo usaremos en el siguiente paso:

![pipenv output](/images/posts/neovim-python/pipenv-install.png)

### Archivos .vim

#### general/settings.vim

```settings.vim
syntax enable

set guicursor=                                     " Disable blinking for the n-v-c modes
set termguicolors
set guioptions-=T                                   " No Tool bar

set cursorline                                     " Highlight the current line

set hidden                                         " When on a buffer becomes hidden when it is abandoned
set path+=**
set nowrap
set encoding=UTF-8

set number relativenumber

set smartindent
set smarttab
set tabstop=4 softtabstop=4
set shiftwidth=4
set expandtab
set smartcase
set incsearch
set nohlsearch
set completeopt=menuone,noinsert,noselect
set signcolumn=yes
set colorcolumn=80
highlight ColorColumn ctermbg=0 guibg=lightgrey

set noswapfile
set nobackup
set undofile
execute 'set undodir=' . g:nvim_data_root . '/undodir'

" Python virtual environment detection in linux
let g:loaded_python_provider = 0
" Ruta donde se creó el ambiente de Python con pipenv.
let g:python3_host_prog = '/home/{usuario}/.local/share/virtualenvs/{nvim-999999}/bin/python'

```

### Plugins

#### $HOME/.config/nvim/nvim-plugins/plugins.vim

```plugins.vim
call plug#begin(stdpath('data').'/plugged')

    " NERDTree
    Plug 'preservim/nerdtree'

    " VIM-DEVICONS icons for NERDTree
    Plug 'ryanoasis/vim-devicons'

    " Conquer Of Completion
    Plug 'neoclide/coc.nvim', {'branch': 'release'}

    " Auto Pairs insert or delete brackets, parens, quotes in pair.
    Plug 'jiangmiao/auto-pairs'

    " Surround.vim
    Plug 'tpope/vim-surround'

    " Airline status bar
    Plug 'vim-airline/vim-airline'
    Plug 'vim-airline/vim-airline-themes'

    " NERDCommenter
    Plug 'preservim/nerdcommenter'

call plug#end()

```

#### $HOME/.config/nvim/plug-config/coc.vim

El siguiente es un ejemplo funcional tomado de <a href="https://github.com/neoclide/coc.nvim" target="_blank">la documentación oficial</a> en github.

```coc.vim
let g:coc_global_extensions = [
    \ 'coc-snippets',
    \ 'coc-pairs',
    \ 'coc-python',
    \ 'coc-prettier',
    \ 'coc-vimlsp',
    \ 'coc-marketplace',
\ ]

" TextEdit might fail if hidden is not set.
set hidden

" Some servers have issues with backup files, see #649.
set nobackup
set nowritebackup

" Give more space for displaying messages.
set cmdheight=2

" Having longer updatetime (default is 4000 ms = 4 s) leads to noticeable
" delays and poor user experience.
set updatetime=300

" Don't pass messages to |ins-completion-menu|.
set shortmess+=c

" Always show the signcolumn, otherwise it would shift the text each time
" diagnostics appear/become resolved.
if has("patch-8.1.1564")
  " Recently vim can merge signcolumn and number column into one
  set signcolumn=number
else
  set signcolumn=yes
endif

" Use tab for trigger completion with characters ahead and navigate.
" NOTE: Use command ':verbose imap <tab>' to make sure tab is not mapped by
" other plugin before putting this into your config.
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" Use <c-space> to trigger completion.
if has('nvim')
  inoremap <silent><expr> <c-space> coc#refresh()
else
  inoremap <silent><expr> <c-@> coc#refresh()
endif

" Make <CR> auto-select the first completion item and notify coc.nvim to
" format on enter, <cr> could be remapped by other vim plugin
inoremap <silent><expr> <cr> pumvisible() ? coc#_select_confirm()
                              \: "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"

" Use `[g` and `]g` to navigate diagnostics
" Use `:CocDiagnostics` to get all diagnostics of current buffer in location list.
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)

" GoTo code navigation.
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Use K to show documentation in preview window.
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  elseif (coc#rpc#ready())
    call CocActionAsync('doHover')
  else
    execute '!' . &keywordprg . " " . expand('<cword>')
  endif
endfunction

" Highlight the symbol and its references when holding the cursor.
autocmd CursorHold * silent call CocActionAsync('highlight')

" Symbol renaming.
nmap <leader>rn <Plug>(coc-rename)

" Formatting selected code.
xmap <leader>f  <Plug>(coc-format-selected)
nmap <leader>f  <Plug>(coc-format-selected)

augroup mygroup
  autocmd!
  " Setup formatexpr specified filetype(s).
  autocmd FileType typescript,json setl formatexpr=CocAction('formatSelected')
  " Update signature help on jump placeholder.
  autocmd User CocJumpPlaceholder call CocActionAsync('showSignatureHelp')
augroup end

" Applying codeAction to the selected region.
" Example: `<leader>aap` for current paragraph
xmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)

" Remap keys for applying codeAction to the current buffer.
nmap <leader>ac  <Plug>(coc-codeaction)
" Apply AutoFix to problem on the current line.
nmap <leader>qf  <Plug>(coc-fix-current)

" Map function and class text objects
" NOTE: Requires 'textDocument.documentSymbol' support from the language server.
xmap if <Plug>(coc-funcobj-i)
omap if <Plug>(coc-funcobj-i)
xmap af <Plug>(coc-funcobj-a)
omap af <Plug>(coc-funcobj-a)
xmap ic <Plug>(coc-classobj-i)
omap ic <Plug>(coc-classobj-i)
xmap ac <Plug>(coc-classobj-a)
omap ac <Plug>(coc-classobj-a)

" Remap <C-f> and <C-b> for scroll float windows/popups.
if has('nvim-0.4.0') || has('patch-8.2.0750')
  nnoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? coc#float#scroll(1) : "\<C-f>"
  nnoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? coc#float#scroll(0) : "\<C-b>"
  inoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? "\<c-r>=coc#float#scroll(1)\<cr>" : "\<Right>"
  inoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? "\<c-r>=coc#float#scroll(0)\<cr>" : "\<Left>"
  vnoremap <silent><nowait><expr> <C-f> coc#float#has_scroll() ? coc#float#scroll(1) : "\<C-f>"
  vnoremap <silent><nowait><expr> <C-b> coc#float#has_scroll() ? coc#float#scroll(0) : "\<C-b>"
endif

" Use CTRL-S for selections ranges.
" Requires 'textDocument/selectionRange' support of language server.
nmap <silent> <C-s> <Plug>(coc-range-select)
xmap <silent> <C-s> <Plug>(coc-range-select)

" Add `:Format` command to format current buffer.
command! -nargs=0 Format :call CocAction('format')

" Add `:Fold` command to fold current buffer.
command! -nargs=? Fold :call     CocAction('fold', <f-args>)

" Add `:OR` command for organize imports of the current buffer.
command! -nargs=0 OR   :call     CocAction('runCommand', 'editor.action.organizeImport')

" Add (Neo)Vim's native statusline support.
" NOTE: Please see `:h coc-status` for integrations with external plugins that
" provide custom statusline: lightline.vim, vim-airline.
set statusline^=%{coc#status()}%{get(b:,'coc_current_function','')}

" Mappings for CoCList
" Show all diagnostics.
nnoremap <silent><nowait> <space>a  :<C-u>CocList diagnostics<cr>
" Manage extensions.
nnoremap <silent><nowait> <space>e  :<C-u>CocList extensions<cr>
" Show commands.
nnoremap <silent><nowait> <space>c  :<C-u>CocList commands<cr>
" Find symbol of current document.
nnoremap <silent><nowait> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols.
nnoremap <silent><nowait> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent><nowait> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent><nowait> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list.
nnoremap <silent><nowait> <space>p  :<C-u>CocListResume<CR>
```

#### $HOME/.config/nvim/plug-config/nerdcommenter.vim

```nerdcommenter.vim
" Add spaces after comment delimiters by default
let g:NERDSpaceDelims = 1

" Use compact syntax for prettified multi-line comments
let g:NERDCompactSexyComs = 1

" Align line-wise comment delimiters flush left instead of following code indentation
let g:NERDDefaultAlign = 'left'

" Set a language to use its alternate delimiters by default
let g:NERDAltDelims_java = 1

" Add your own custom formats or override the defaults
let g:NERDCustomDelimiters = { 'c': { 'left': '/**','right': '*/'  }  }

" Allow commenting and inverting empty lines (useful when commenting a region)
let g:NERDCommentEmptyLines = 1

" Enable trimming of trailing whitespace when uncommenting
let g:NERDTrimTrailingWhitespace = 1

" Enable NERDCommenterToggle to check all selected lines is commented or not
 let g:NERDToggleCheckAllLines = 1"

```

#### $HOME/.config/nvim/plug-config/airline.vim

```airline.vim
" enable tabline
let g:airline#extensions#tabline#enabled = 1
" let g:airline#extensions#tabline#left_sep = ''
" let g:airline#extensions#tabline#left_alt_sep = ''
" let g:airline#extensions#tabline#right_sep = ''
" let g:airline#extensions#tabline#right_alt_sep = ''

" enable powerline fonts
let g:airline_powerline_fonts = 1
" let g:airline_left_sep = ''
" let g:airline_right_sep = ''

" Always show tabs
set showtabline=2

" We don't need to see things like -- INSERT -- anymore
set noshowmode
```

#### $HOME/.config/nvim/keymapping/keys.vim

```keys.vim
" Edit vimrc faster
nnoremap <Leader>v :e $MYVIMRC<cr>

" Reload init.vim
nnoremap <silent> <Leader><Leader> :source $MYVIMRC<cr>

"Change working directory
nnoremap <leader>cd :cd %:p:h<cr>

"Mapping NERDTree
map <C-n> :NERDTreeToggle<cr>

" Use alt + hjkl to resize windows
nnoremap <M-j>  :resize -2<CR>
nnoremap <M-k>  :resize +2<CR>
nnoremap <M-h>  :vertical resize -2<CR>
nnoremap <M-l>  :vertical resize +2<CR>


" NerdCommenter Toggle remap to CTRL+/
vmap <C-_> <Plug>NERDCommenterToggle<cr>
nmap <C-_> <Plug>NERDCommenterToggle<cr>

" Start Terminal using PowerShell 7 (Preview)
" For default shell, remove "://pwsh.exe"
" You can customize the shell by replacing "pwsh.exe" with your shell for example:
"       :edit term://bash
"       :vsplit term://top
" For more :help terminal
nmap <leader>t :vsplit term://bash<cr>

" Move between buffers
nnoremap <leader><tab> :bn<cr>
nnoremap <leader><S-tab> :bp<cr>

" Closing and hiding current buffer
nnoremap <leader>w :bd<cr>
```

#### $HOME/.config/nvim/init.vim

```init.vim
let g:nvim_data_root = stdpath('data')
let g:nvim_config_root = stdpath('config')
let g:config_file_list = ['/nvim-plugins/plugins.vim',
    \ '/general/settings.vim',
    \ '/keymapping/keys.vim',
    \ '/plug-config/airline.vim',
    \ '/plug-config/coc.vim',
    \ '/plug-config/nerdcommenter.vim',
    \ ]

for f in g:config_file_list
    execute 'source ' . g:nvim_config_root . f
endfor
```

## Bonus

### Gruvbox 💻

En lo personal, el mejor tema para programar en Neovim, es <a href="https://github.com/morhetz/gruvbox" target="_blank"> Gruvbox</a>, la combinación de colores se me hace seria y sobretodo muy cómodo para la vista, simple y sencillamente... ¡Me Encanta!

Primero agregamos el plugin en $HOME/.config/nvim/nvim-plugins/plugins.vim

```plugins.vim
    " Other themes
    Plug 'morhetz/gruvbox'
```

Recuerda que una vez que guardes los cambios, debes salir de Neovim y volver a entrar, luego correr :PlugInstall

Después hay que editar $HOME/.config/nvim/general/settings.vim para agregar gruvbox como tema por defecto.

```settings.vim
" Gruvbox por defecto
colorscheme gruvbox
let g:gruvbox_contrast_dark='hard'
set background=dark
```

Por último, vamos a configurar Airline con el mismo tema de colores. Para eso hay que editar $HOME/.config/nvim/plug-config/airline.vim

```airline.vim
"Gruvbox
let g:airline_theme='gruvbox'
```

## Conclusión

![Neovim Como VSCode](/images/posts/neovim-python/neovim-como-vscode.png)

<a href="https://neovim.io" target="_blank">Neovim</a> es un editor de textos ligero y sobretodo muy "configurable", al principio puede parecer intimidante pero con el tiempo y la práctica se vuelve una herramienta muy útil para el desarrollo.

Si deseas conocer que otras extensiones existen para CoC, visita la <a href="https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions" target="_blank">wiki oficial</a> donde encontraras basta información.

Si tienes algún comentario o recomendación de algún artículo que te gustaría que cubriera en este blog, no dudes en mandarme un mensaje en twitter a <a href="https://twitter.com/jorgers" target="\_blank">@JorgeRS</a>

Si te gustó este artículo por favor compartelo en cualquiera de las redes sociales utilizando los botones más abajo.
