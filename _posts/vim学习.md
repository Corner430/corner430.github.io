---
title: vim学习
date: 2023-06-08 10:07:14
tags:
    - Linux
    - vim
    - 笔记
declare: true
---
## 教程

[Vimtutor_1.5](http://www2.geog.ucl.ac.uk/~plewis/teaching/unix/vimtutor)
[vimtutor](https://vimhelp.org/usr_01.txt.html#vimtutor)：有一些书籍进行推荐，还有电子版
[VIM无废话极简入门](https://www.bilibili.com/video/BV1qg4y13766/?spm_id_from=333.880.my_history.page.click&vd_source=2dd00fcea46a9c5a26706a99eb12ea3f)
《Vim实用技巧》
《精通vim》
[lazyvim](https://github.com/LazyVim/LazyVim)
[Vimium](https://github.com/philc/vimium)
[Vimium-c](https://github.com/gdh1995/vimium-c)

- 下载最新中文版本的vimtutor
`vim --clean -c 'e $VIMRUNTIME/tutor/tutor.zh_cn.utf-8' -c 'w! TUTORCOPY' -c 'q'`<!--more-->

------------------------------
## Vim 使用技巧

### 常用命令

- `W`或`E`或`B` 逐个广义单词移动
- `{` 或 `}` 逐个段落移动
- `b` 回退
- `0` 或者 `^` 行首
- `t`或`f` 在行内查找下一指定字符
- `T`或`F` 在行内查找上一指定字符
- `di` 删除范围内的字符
- `da` 删除范围内的字符(包括范围字符)
- `daw` 删除一个单词
- `dap` 删除一个段落
- `zz` 将当前行显示在窗口正中
- `:num` 等同于 `num G`
- `:+num` 向下移动 num 行
- `:-num` 向上移动 num 行
- `#` 在当前光标所在单词上按`#`，进入查找该单词状态
- `:%s/old/new` 全局替换
- `:34s/old/new` 替换第34行的字符
- `:34,40s/old/new` 替换第34到40行的字符
- `:10,20/your_search_term` 在第10到20行搜索
- `/hel.\{1,3\}` 正则表达式搜索
    - `hel`：匹配字母 "hel" 作为开头。
    - `.`：匹配除换行符外的任意字符。
    - `\{1,3\}`：表示前一个字符或子表达式的重复次数范围。这里指定了 1 到 3 次重复，表示匹配一个到三个任意字符。
- `yj` 复制下一行
- `yi` 范围复制
- `ya` 范围复制(包括范围字符)
- `P` 前粘贴
- `v` 普通可视化
- `V` 行可视化
- `Ctrl + v` (矩形)块可视化
- `Ctrl + G` 显示当前行号、列号及文件信息
- `Ctrl + a` 增加数字，例如：`10<C-a>` 将数字增加 10。**如果当前光标所在位置不是数字，那么会在当前行正向查找一个数字，然后增加。**
- `Ctrl + x` 减少数字，同上。**Vim 把以 0 开头的数字当作八进制处理。可以通过 `set nrformats=` 来进行设置。**
- `;` 快速跳转到下一个与当前光标位置处的字符匹配的位置的命令。
- `,` 快速跳转到上一个与当前光标位置处的字符匹配的位置的命令。
- `/` 向下查找
- `?` 向上查找
- `&` 重复上一次使用 `:s` 命令执行的替换操作
- `qx{changes}q` 录制宏
- `*` 在当前光标所在单词上按`*`，进入查找该单词状态。**并且会高亮显示所有匹配的单词，如果没有高亮，可以使用`:set hls`命令开启高亮显示。**
- `cc` 清除整行，然后进入插入模式。保持原来的缩进。
- `Shift + (` 和 `Shift + )` 分别将光标移动到上一句和下一句的开头。
- `H` 将光标移动到屏幕的顶部。
- `L` 将光标移动到屏幕的底部。
- `:term[inal]` 打开终端

插入模式下：
- `<C-h>` 删除前一个字符（同退格键）
- `<C-w>` 删除前一个单词
- `<C-u>` 删至行首

> 这些命令并非插入模式独有，在Vim的命令行模式中，以及在 bash shell 中，也可以使用它们。

- `<Esc>` 切换到普通模式
- `<C-[>` 切换到普通模式
- `<C-o>` 切换到 **插入 - 普通** 模式，可以执行一条普通模式命令，之后自动进入到插入模式
高频 `<C-o>zz`

--------------------------------------------------
### 一箭双雕

- `>G` 增加从当前行到文档末尾处的缩进层级
- `.` 重复上一次修改

一箭双雕
| 复合命令 | 等效的长命令 |
| :--- | :---: |
| C | c$ |
| s | cl |
| S | ^C |
| I | ^i |
| o | A<CR> |
| O | ko |
> 这些命令的共同点：都会从普通模式切换到插入模式，这将会对`.`命令产生影响。

-------------------------------------
### 操作符 + 动作命令 = 操作

此处相当精彩，参见 技巧 12 双剑合璧，天下无敌

例如知道 `daw` 删除一个单词，那么 `gUaw` 就可以把当前单词转换成大写形式，这就是所谓 `d{motion}`

| 命令 | 用途 |
| :--- | :---: |
| `c` | 修改 |
| `d` | 删除 |
| `y` | 复制 |
| `g~` | 反转大小写 |
| `gu` | 转换为小写 |
| `gU` | 转换为大写 |
| `>` | 增加缩进 |
| `<` | 减少缩进 |
| `=` | 自动缩进 |
| `!` | 运行外部过滤器过滤{motion}所跨越的行 |

-------------------------------------
### Vim 使用技巧

`.` 命令是一个微型的宏，Vim可以录制任意数目的按键操作，然后在以后重复执行它们。这让我们可以把最常重复的工作流程录制下来，并用一个按键重放它们。

控制好对 `<Esc>` 键的使用，就可以 `u` 控制撤销的粒度。**如果再插入模式中使用了 `<Up>`、`<Down>`、`<Left>`、`<Right>` 这些光标键，将会产生一个新的撤销块。等同于先切换到了普通模式，然后用 `h`、`j`、`k`、`l` 移动光标。**

如果必要，请使用次数，例如：`c3w`、`b3w`。

当一个操作符命令被连续调用两次时，它会作用于当前行，例如：`dd`，`>>`


------------------------------------------
### 折叠
- `zo` 打开当前行折叠
- `zc` 关闭当前折叠块
- `za` 打开或关闭当前折叠块
- `zR` 打开所有折叠块
- `zM` 关闭所有折叠块

-----------------------------------
### 插件
`mkdir -p ~/.vim ~/.vim/autoload ~/.vim/backup ~/.vim/colors ~/.vim/plugged`

- `vim-plug`
- `NERDTree`
  - `t`: Open the selected file in a new tab
  - `i`: Open the selected file in a horizontal split window
  - `s`: Open the selected file in a vertical split window
  - `I`: Toggle hidden files
  - `m`: Show the NERD Tree menu
  - `R`: Refresh the tree, useful if files change outside of Vim
  - `?`: Toggle NERD Tree's quick help
- `ale`
- `NERDCommenter`
    - `<count><leader>cc`：在普通模式或可视模式下，将当前行或选中的文本注释掉。使用`NERDCommenterComment`命令。

    - `<count><leader>cn`：与`cc`相同，但强制嵌套注释。使用`NERDCommenterNested`命令。

    - `<count><leader>c<space>`：切换所选行的注释状态。如果顶部选定行已注释，则取消注释所有选定行，反之亦然。使用`NERDCommenterToggle`命令。

    - `<count><leader>cm`：使用一组多部分分隔符对给定行进行注释。使用`NERDCommenterMinimal`命令。

    - `<count><leader>ci`：独立切换所选行的注释状态。使用`NERDCommenterInvert`命令。

    - `<count><leader>cs`：使用漂亮的块格式布局对选定的行进行注释。使用`NERDCommenterSexy`命令。

    - `<count><leader>cy`：与`cc`相同，但首先将注释行复制到剪贴板。使用`NERDCommenterYank`命令。

    - `<leader>c$`：从光标到行尾对当前行进行注释。使用`NERDCommenterToEOL`命令。

    - `<leader>cA`：在行尾添加注释分隔符，并进入插入模式。使用`NERDCommenterAppend`命令。

    - `<leader>ca`：切换到备用的分隔符集。使用`NERDCommenterAltDelims`命令。

    - `<count><leader>cl`：与`NERDCommenterComment`相同，但分隔符沿左侧对齐。使用`NERDCommenterAlignLeft`命令。

    - `<count><leader>cb`：与`NERDCommenterComment`相同，但分隔符同时沿左右两侧对齐。使用`NERDCommenterAlignBoth`命令。

    - `<count><leader>cu`：取消选定行的注释。使用`NERDCommenterUncomment`命令。

除了这些默认映射外，还有一个命令`NERDCommenterInsert`，默认情况下被禁用，它在插入模式下在当前光标位置添加注释分隔符并进入插入模式。


------------------------------------
### 搜索
- `:grep` 和 `:vimgrep`
- `ack`

--------------------------------------
### 寄存器
- 默认寄存器：`""`，存储最近的复制和剪切操作的文本，使用 `y` 复制和 `d` 剪切，然后使用 `p` 粘贴。
- 命名寄存器：由大写字母和数字标识，如 `"a` 或 `"1`，用于存储和检索特定文本。例如，`"ayw` 复制单词到 `"a` 寄存器，然后使用 `"ap` 粘贴。
- 黑洞寄存器：`"_`，用于丢弃文本，特别适用于删除文本但不保存在其他寄存器中。
- 操作寄存器：使用 `"` 后跟寄存器标识符来选择要使用的寄存器，例如 `"a` 表示使用 `"a` 寄存器。
- 查看寄存器内容：使用 `:reg` 命令，如 `:reg a` 查看 `"a` 寄存器的内容。

#### 从外部复制文本到vim中
```vim
set clipboard=unamed                " 复制到系统寄存器(*)
set clipboard=unnamedplus           " 复制到系统寄存器(+)
set clipboard=unnamed,unnamedplus   " 复制到系统寄存器(*, +)
```

----------------------------------------
### vimrc
[rfc](https://www.freecodecamp.org/news/vimrc-configuration-guide-customize-your-vim-editor/)

```vim
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""               
"               
"               ██╗   ██╗██╗███╗   ███╗██████╗  ██████╗
"               ██║   ██║██║████╗ ████║██╔══██╗██╔════╝
"               ██║   ██║██║██╔████╔██║██████╔╝██║     
"               ╚██╗ ██╔╝██║██║╚██╔╝██║██╔══██╗██║     
"                ╚████╔╝ ██║██║ ╚═╝ ██║██║  ██║╚██████╗
"                 ╚═══╝  ╚═╝╚═╝     ╚═╝╚═╝  ╚═╝ ╚═════╝
"               
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""               

" Disable compatibility with vi which can cause unexpected issues.
set nocompatible

" Enable type file detection. Vim will be able to try to detect the type of file in use.
filetype on

" Enable plugins and load plugin for the detected file type.
filetype plugin on

" Load an indent file for the detected file type.
filetype indent on

" Turn syntax highlighting on.
syntax on

" Add numbers to each line on the left-hand side.
set number

" Highlight cursor line underneath the cursor horizontally.
set cursorline

" Highlight cursor line underneath the cursor vertically.
 set cursorcolumn


" --------------------------------------------------


" Set shift width to 4 spaces.
set shiftwidth=2

" Set tab width to 4 columns.
set tabstop=2

" Use space characters instead of tabs.
set expandtab

" Do not save backup files.
set nobackup

" Do not let cursor scroll below or above N number of lines when scrolling.
set scrolloff=10

" Do not wrap lines. Allow long lines to extend as far as the line goes.
set nowrap

" While searching though a file incrementally highlight matching characters as you type.
set incsearch

" Ignore capital letters during search.
set ignorecase

" Override the ignorecase option if searching for capital letters.
" This will allow you to search specifically for capital letters.
set smartcase

" Show partial command you type in the last line of the screen.
set showcmd

" Show the mode you are on the last line.
set showmode

" Show matching words during a search.
set showmatch

" Use highlighting when doing a search.
set hlsearch

" Set the commands to save in history default number is 20.
set history=1000


" -------------------------------------



" Enable auto completion menu after pressing TAB.
set wildmenu

" Make wildmenu behave like similar to Bash completion.
set wildmode=list:longest

" There are certain files that we would never want to edit with Vim.
" Wildmenu will ignore files with these extensions.
set wildignore=*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx






" PLUGINS ---------------------------------------------------------------- {{{

call plug#begin('~/.vim/plugged')

  " 语法检查
  Plug 'dense-analysis/ale'
  
  " Navigation
  Plug 'preservim/nerdtree'

  " 状态栏
  Plug 'itchyny/lightline.vim'

  " 代码注释
  Plug 'preservim/nerdcommenter'

  " 自动补全括号
  Plug 'jiangmiao/auto-pairs'

  " 颜色主题
  Plug 'junegunn/seoul256.vim'

  " 代码对齐线 
  Plug 'Yggdroot/indentLine'

  " sorround
  Plug 'tpope/vim-surround'

  " copilot
  Plug 'github/copilot.vim'

  " cpp syntax highlight
  Plug 'octol/vim-cpp-enhanced-highlight'

  " clang-format
  Plug 'rhysd/vim-clang-format'

  " 模糊搜索
  Plug 'Yggdroot/LeaderF'

  " leetcode
  "Plug 'ianding1/leetcode.vim'

call plug#end()

" }}}


" MAPPINGS --------------------------------------------------------------- {{{

" Set the "," as the leader key.
let mapleader = ","

" Press ,, to jump back to the last cursor position.
nnoremap <leader>, ``

" Press \p to print the current file to the default printer from a Linux operating system.
" View available printers:   lpstat -v
" Set default printer:       lpoptions -d <printer_name>
" <silent> means do not display output.
"nnoremap <silent> <leader>p :%w !lp<CR>

" Type jj to exit insert mode quickly.
inoremap jj <Esc>

" Press the space bar to type the : character in command mode.
nnoremap <space> :

" Pressing the letter o will open a new line below the current one.
" Exit insert mode after creating a new line above or below the current line.
"nnoremap o o<esc>
"nnoremap O O<esc>

" Center the cursor vertically when moving to the next word during a search.
nnoremap n nzz
nnoremap N Nzz

" Yank from cursor to the end of line.
nnoremap Y y$

" Map the F5 key to run a Python script inside Vim.
" I map F5 to a chain of commands here.
" :w saves the file.
" <CR> (carriage return) is like pressing the enter key.
" !clear runs the external clear screen command.
" !python3 % executes the current file with Python.
nnoremap <f5> :w <CR>:!clear <CR>:!python3 % <CR>

" You can split the window in Vim by typing :split or :vsplit.
" Navigate the split view easier by pressing CTRL+j, CTRL+k, CTRL+h, or CTRL+l.
nnoremap <c-j> <c-w>j
nnoremap <c-k> <c-w>k
nnoremap <c-h> <c-w>h
nnoremap <c-l> <c-w>l

" Resize split windows using arrow keys by pressing:
" CTRL+UP, CTRL+DOWN, CTRL+LEFT, or CTRL+RIGHT.
noremap <c-up> <c-w>+
noremap <c-down> <c-w>-
noremap <c-left> <c-w>>
noremap <c-right> <c-w><

" NERDTree specific mappings.
" Map the F3 key to toggle NERDTree open and close.
nnoremap <F3> :NERDTreeToggle<cr>

" Have nerdtree ignore certain files and directories.
let NERDTreeIgnore=['\.git$', '\.jpg$', '\.mp4$', '\.ogg$', '\.iso$', '\.pdf$', '\.pyc$', '\.odt$', '\.png$', '\.gif$', '\.db$']

" }}}


" VIMSCRIPT -------------------------------------------------------------- {{{

" This will enable code folding.
" Use the marker method of folding.
augroup filetype_vim
    autocmd!
    autocmd FileType vim setlocal foldmethod=marker
augroup END

" More Vimscripts code goes here.

" }}}


" STATUS LINE ------------------------------------------------------------ {{{

" Clear status line when vimrc is reloaded.
set statusline=

" Status line left side.
set statusline+=\ %F\ %M\ %Y\ %R

" Use a divider to separate the left side from the right side.
set statusline+=%=

" Status line right side.
set statusline+=\ ascii:\ %b\ hex:\ 0x%B\ row:\ %l\ col:\ %c\ percent:\ %p%%

" Show the status on the second to last line.
set laststatus=2

" }}}


" -----------nerdcommenter--------------

" Create default mappings
let g:NERDCreateDefaultMappings = 1

" Add spaces after comment delimiters by default
let g:NERDSpaceDelims = 1

" Use compact syntax for prettified multi-line comments
let g:NERDCompactSexyComs = 1

" Align line-wise comment delimiters flush left instead of following code indentation
let g:NERDDefaultAlign = 'left'

" Set a language to use its alternate delimiters by default
let g:NERDAltDelims_java = 1

" Add your own custom formats or override the defaults
let g:NERDCustomDelimiters = { 'c': { 'left': '/**','right': '*/' } }

" Allow commenting and inverting empty lines (useful when commenting a region)
let g:NERDCommentEmptyLines = 1

" Enable trimming of trailing whitespace when uncommenting
let g:NERDTrimTrailingWhitespace = 1

" Enable NERDCommenterToggle to check all selected lines is commented or not
let g:NERDToggleCheckAllLines = 1


" -----------seoul256.vim--------------------------
" Unified color scheme (default: dark)
" colo seoul256

" Light color scheme
colo seoul256-light

" Switch
" set background=dark
set background=light


" ----------------LeaderF----------------------------
" Don't display LeaderF help in normal mode
let g:Lf_HideHelp = 1

" Disable caching, regenerate results each time
let g:Lf_UseCache = 0

" Turn off version control tool, disregard version-controlled files
let g:Lf_UseVersionControlTool = 0

" Ignore current buffer's file name, exclude it from search scope
let g:Lf_IgnoreCurrentBufferName = 1

" Set LeaderF window position to popup
let g:Lf_WindowPosition = 'popup'

" Define LeaderF window separators and font
let g:Lf_StlSeparator = { 'left': "\ue0b0", 'right': "\ue0b2", 'font': "DejaVu Sans Mono for Powerline" }

" Disable preview feature, search results won't display preview info
let g:Lf_PreviewResult = {'Function': 0, 'BufTag': 0 }

" Mappings: Define actions for pressing specific keys in LeaderF
" Search for files
let g:Lf_ShortcutF = "<leader>ff"

" Search for buffers
noremap <leader>fb :<C-U><C-R>=printf("Leaderf buffer %s", "")<CR><CR>

" Search for most recently used files
noremap <leader>fm :<C-U><C-R>=printf("Leaderf mru %s", "")<CR><CR>

" Search for tags
noremap <leader>ft :<C-U><C-R>=printf("Leaderf bufTag %s", "")<CR><CR>

" Search for current word in current buffer
noremap <C-B> :<C-U><C-R>=printf("Leaderf! rg --current-buffer -e %s ", expand("<cword>"))<CR>

" Search for current word globally
noremap <C-F> :<C-U><C-R>=printf("Leaderf! rg -e %s ", expand("<cword>"))<CR>

" Search for visually selected text
xnoremap gf :<C-U><C-R>=printf("Leaderf! rg -F -e %s ", leaderf#Rg#visual())<CR>

" Use LeaderF to search recall history
noremap go :<C-U>Leaderf! rg --recall<CR>

" Operations related to gtags
" Must execute `Leaderf gtags --update` first
let g:Lf_GtagsAutoGenerate = 0
let g:Lf_Gtagslabel = 'native-pygments'

" Search for references
noremap <leader>fr :<C-U><C-R>=printf("Leaderf! gtags -r %s --auto-jump", expand("<cword>"))<CR><CR>

" Search for definitions
noremap <leader>fd :<C-U><C-R>=printf("Leaderf! gtags -d %s --auto-jump", expand("<cword>"))<CR><CR>

" Search recall history
noremap <leader>fo :<C-U><C-R>=printf("Leaderf! gtags --recall %s", "")<CR><CR>

" Show next gtags search result
noremap <leader>fn :<C-U><C-R>=printf("Leaderf gtags --next %s", "")<CR><CR>

" Show previous gtags search result
noremap <leader>fp :<C-U><C-R>=printf("Leaderf gtags --previous %s", "")<CR><CR>


" ----------------------clang-format-------------------------------------

let g:clang_format#style_options = {
            \ "AccessModifierOffset" : -4,
            \ "AllowShortIfStatementsOnASingleLine" : "true",
            \ "AlwaysBreakTemplateDeclarations" : "true",
            \ "Standard" : "C++11"}

" map to <Leader>cf in C++ code
autocmd FileType c,cpp,objc nnoremap <buffer><Leader>cf :<C-u>ClangFormat<CR>
autocmd FileType c,cpp,objc vnoremap <buffer><Leader>cf :ClangFormat<CR>
" if you install vim-operator-user
autocmd FileType c,cpp,objc map <buffer><Leader>x <Plug>(operator-clang-format)
" Toggle auto formatting:
nmap <Leader>C :ClangFormatAutoToggle<CR>
```

--------------------------------------------------
## Vim 重编译
```bash
git clone https://github.com/vim/vim.git

cd vim/src
make distclean

sudo apt install python-dev python3-dev libncurses5-dev
./configure --with-features=huge --enable-python3interp --enable-pythoninterp --enable-rubyinterp --enable-luainterp --enable-perlinterp --enable-multibyte --enable-cscope

make
sudo make install
```
