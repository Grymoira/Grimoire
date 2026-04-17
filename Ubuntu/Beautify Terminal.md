
> This instruction is based on Ubuntu 24.04 LTS System

[`fastfetch configuration file content`](https://github.com/Efterklang/dotfiles/blob/main/tui_cli/fastfetch/config.jsonc)

[`oh-my-post github repo`](https://github.com/JanDeDobbeleer/oh-my-posh) 

[`oh-my-post official docs`](https://ohmyposh.dev/docs/themes)

Install the `fastfetch`:

```bash
sudo add-apt-repository ppa:zhangsongcui3371/fastfetch
sudo apt update
sudo apt install fastfetch
```

And create configuration file:

```bash
mkdir -p ~/.config/fastfetch/
```

Edit configuration file:

```bash
nano ~/.config/fastfetch/config.jsonc
```

Here is an configuration method：

```jsonc
{
  "$schema": "https://github.com/fastfetch-cli/fastfetch/raw/dev/doc/json_schema.json",
  "logo": {
    "source": "~/.config/fastfetch/preset/anime.txt",
    "color": {
      "1": "white"
    },
    "padding": {
      "right": 1,
      "left": 1,
      "top": 3
    }
  },
  // "logo": {
  //   "source": "./preset/nyarch.png",
  //   "type": "iterm",
  //   "height": 25,
  //   "width": 30
  // },
  "display": {
    "separator": "➜  "
  },
  "modules": [
    "break",
    {
      "type": "title",
      "format": "Hello{7}{6}"
    },
    "break",
    {
      "type": "os",
      "key": " OS           ",
      "keyColor": "yellow",
      "format": "{8}"      
    },
    {
      "type": "kernel",
      "key": "│ ├ Kernel    ",
      "keyColor": "yellow"
    },
    {
      "type": "packages",
      "key": "│ ├󰏖 Packages  ",
      "keyColor": "yellow"
    },
    {
      "type": "shell",
      "key": "│ └ Shell     ",
      "keyColor": "yellow"
    },
    {
      "type": "de",
      "key": " DE           ",
      "keyColor": "green"
    },
    {
      "type": "wm",
      "key": "│ ├󰂫 WM        ",
      "keyColor": "green"
    },
    {
      "type": "lm",
      "key": "│ ├󰧨 LM        ",
      "keyColor": "green"
    },
    {
      "type": "terminal",
      "key": "│ ├ Terminal  ",
      "keyColor": "green"
    },
    {
      "type": "host",
      "key": " PC           ",
      "keyColor": "red"
    },
//    {
//      "type": "cpu",
//      "key": "│ ├ CPU       ",
//      "keyColor": "red"
//    },
//    {
//      "type": "disk",
//      "key": "│ ├󰋊 Disk      ",
//      "keyColor": "red"
//    },
    {
      "type": "memory",
      "key": "│ ├ Memory    ",
      "keyColor": "red"
    },
    {
      "type": "swap",
      "key": "│ ├󰓡 Swap      ",
      "keyColor": "red"
    },
    {
      "type": "uptime",
      "key": "│ ├󰅐 Uptime    ",
      "keyColor": "red"
    },
    {
      "type": "display",
      "key": "│ └󰍹 Display   ",
      "keyColor": "red",
      "format": "{1}x{2} @ {3}Hz"
    },
//    {
//      "type": "sound",
//      "key": " SND          ",
//      "keyColor": "magenta"
//    },
//    {
//      "key": " Wifi         ",
//      "keyColor": "magenta",
//      "type": "wifi"
//    },
    {
      "key": "󰩟 Local IP     ",
      "keyColor": "magenta",
      "type": "localip",
      "compact": true
    },
    "break",
    {
      "type": "colors",
//      "paddingLeft": 1,
      "symbol": "circle",
      "block": {
        "width": 9
      }
    }
  ]
}
```

Edit the `anime.txt`:

```bash
mkdir -p ~/.config/fastfetch/preset/
nano ~/.config/fastfetch/preset/anime.txt
```

```text
​ ⣇⣿⠘⣿⣿⣿⡿⡿⣟⣟⢟⢟⢝⠵⡝⣿⡿⢂⣼⣿⣷⣌⠩⡫⡻⣝⠹⢿⣿⣷ 
​ ⡆⣿⣆⠱⣝⡵⣝⢅⠙⣿⢕⢕⢕⢕⢝⣥⢒⠅⣿⣿⣿⡿⣳⣌⠪⡪⣡⢑⢝⣇ 
​ ⡆⣿⣿⣦⠹⣳⣳⣕⢅⠈⢗⢕⢕⢕⢕⢕⢈⢆⠟⠋⠉⠁⠉⠉⠁⠈⠼⢐⢕⢽ 
​ ⡗⢰⣶⣶⣦⣝⢝⢕⢕⠅⡆⢕⢕⢕⢕⢕⣴⠏⣠⡶⠛⡉⡉⡛⢶⣦⡀⠐⣕⢕ 
​ ⡝⡄⢻⢟⣿⣿⣷⣕⣕⣅⣿⣔⣕⣵⣵⣿⣿⢠⣿⢠⣮⡈⣌⠨⠅⠹⣷⡀⢱⢕ 
​ ⡝⡵⠟⠈⢀⣀⣀⡀⠉⢿⣿⣿⣿⣿⣿⣿⣿⣼⣿⢈⡋⠴⢿⡟⣡⡇⣿⡇⡀⢕ 
​ ⡝⠁⣠⣾⠟⡉⡉⡉⠻⣦⣻⣿⣿⣿⣿⣿⣿⣿⣿⣧⠸⣿⣦⣥⣿⡇⡿⣰⢗⢄ 
​ ⠁⢰⣿⡏⣴⣌⠈⣌⠡⠈⢻⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣬⣉⣉⣁⣄⢖⢕⢕⢕ 
 ⡀⢻⣿⡇⢙⠁⠴⢿⡟⣡⡆⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣷⣵⣵⣿ 
​ ⡻⣄⣻⣿⣌⠘⢿⣷⣥⣿⠇⣿⣿⣿⣿⣿⣿⠛⠻⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿ 
​ ⣷⢄⠻⣿⣟⠿⠦⠍⠉⣡⣾⣿⣿⣿⣿⣿⣿⢸⣿⣦⠙⣿⣿⣿⣿⣿⣿⣿⣿⠟ 
​ ⡕⡑⣑⣈⣻⢗⢟⢞⢝⣻⣿⣿⣿⣿⣿⣿⣿⠸⣿⠿⠃⣿⣿⣿⣿⣿⣿⡿⠁⣠ 
 ⡝⡵⡈⢟⢕⢕⢕⢕⣵⣿⣿⣿⣿⣿⣿⣿⣿⣿⣶⣶⣿⣿⣿⣿⣿⠿⠋⣀⣈⠙ 
​ ⡝⡵⡕⡀⠑⠳⠿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⠿⠛⢉⡠⡲⡫⡪⡪⡣
```

```bash
sudo apt update 
sudo apt install zsh -y
```

Set zsh as the default:

```bash
chsh -s $(which zsh)
```

Install oh-my-post:

```bash
curl -s https://ohmyposh.dev/install.sh | bash -s
```

Edit zsh configuration file:

```bash
nano ~/.zshrc
```

This is the content of my own `.zshrc` file, which includes individual plugins. If these plugins are not downloaded separately, the terminal will automatically report an error.

```.zshrc
export LANG=en_US.UTF-8
HISTSIZE=10000
SAVEHIST=10000
HISTFILE=~/.zsh_history

# 自动补全系统初始化
autoload -Uz compinit && compinit

# 环境变量 & PATH
export PATH=$PATH:/home/chaser/.local/bin


# 别名设置 (Aliases)
alias ls='ls --color=auto'
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'

# 代理函数 (Proxy)
function proxy() {
    local proxy_addr="127.0.0.1:7897"

    export http_proxy="http://$proxy_addr"
    export https_proxy="http://$proxy_addr"
    export ftp_proxy="http://$proxy_addr"
    export all_proxy="socks5h://$proxy_addr"
    export HTTP_PROXY="http://$proxy_addr"
    export HTTPS_PROXY="http://$proxy_addr"
    export FTP_PROXY="http://$proxy_addr"
    export ALL_PROXY="socks5h://$proxy_addr"
    export no_proxy="localhost,127.0.0.1,127.0.1.1,::1,*.local,node,workgroup"
    export NO_PROXY="localhost,127.0.0.1,127.0.1.1,::1,*.local,node,workgroup"

    git config --global http.proxy "http://$proxy_addr"
    git config --global https.proxy "http://$proxy_addr"

    echo -e "\033[32m[✔] Terminal agent is enabled (Port: $proxy_addr)\033[0m"
    echo -e "\033[34m[✔] TIP 1: Use 'sudo -E apt update' to keep proxy for sudo commands.\033[0m"
    echo -e "\033[34m[✔] TIP 2: 'ping' uses ICMP and WILL NOT go through proxy. Use 'curl' to test.\033[0m"
    echo -e "\033[34m[✔] TIP 3: Git proxy has been set to global. Remember to 'proxyoff'.\033[0m"

    echo -e "\033[33mTesting connection & identifying location ...\033[0m"

    local ip_info=$(curl -s --connect-timeout 3 "http://ip-api.com/line?fields=country,city,query" | tr '\n' ' ')

    local http_check=$(curl -Is --connect-timeout 3 https://www.google.com | head -n 1)
    if [[ $http_check == *"200"* ]] || [[ $http_check == *"301"* ]] || [[ $http_check == *"302"* ]]; then
        echo -e "\033[32m[OK] Google connection successful \033[0m"
        if [[ -z "$ip_info" ]]; then
            echo -e "\033[31m[!] Proxy is on but failed to fetch location info.\033[0m"
        else
            echo -e "\033[32m[OK] Location: $ip_info\033[0m"
        fi
    else
        echo -e "\033[31m[ERROR] Unable to access the external network \033[0m"
        echo -e "\033[31m[ERROR] Please check your proxy software \033[0m"
    fi
}

function proxyoff() {
    unset http_proxy https_proxy ftp_proxy all_proxy
    unset HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY
    unset no_proxy NO_PROXY
    git config --global --unset http.proxy
    git config --global --unset https.proxy
    echo -e "\033[31m[✘] Terminal agent and Git proxy have been closed.\033[0m"
}

# 5. 工具初始化

# 运行 fastfetch
fastfetch

# 初始化 zoxide
eval "$(zoxide init zsh)"

# 初始化 oh-my-posh
eval "$(oh-my-posh init zsh --config ~/.cache/oh-my-posh/themes/zash.omp.json)"

# 6. 插件

# 自动补全
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh

# 语法高亮
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

Make the configuration file take effect immediately:

```bash
source ~/.zshrc
```