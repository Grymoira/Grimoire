
This is my Linux system of Ubuntu 24.04 LTS version's Vim setting

```json
let mapleader = ","

set number                       " 显示行号
set relativenumber               " 开启相对行号
set cursorline                   " 高光光标所在行
set mouse=a                      " 开启鼠标支持
set clipboard=unnamedplus        " 同步系统剪切板
set scrolloff=5                  " 光标距离顶部/底部始终保持 5 行

set tabstop=4                    " Tab 宽度 4
set shiftwidth=4                 " 缩进宽度 4
set expandtab                    " 自动将 Tab 转换为空格
set autoindent                   " 自动缩进
set smartindent                  " C 语言智能缩进
set showmatch                    " 括号匹配高亮
set backspace=indent,eol,start   " 允许退格建删除

syntax on                        " 开启语法高亮
filetype plugin indent on        " 自动识别文件类型

nnoremap <leader>r :call CompileRunGcc()<CR>
inoremap <leader>r <ESC>:call CompileRunGcc()<CR>

func! CompileRunGcc()
    exec "w"
    let l:filename = expand('%')
    let l:output = expand('%<')

    if &filetype == 'c'
        exec "!gcc -Wall % -o %< && ./%<"
    elseif &filetype == 'cpp'
        exec "!g++ -Wall % -o %< && ./%<"
    endif
endfunc

inoremap ( ()<ESC>i
inoremap [ []<ESC>i
inoremap " ""<ESC>i
inoremap ' ''<ESC>i

inoremap { {}<ESC>i<CR><ESC>O
```