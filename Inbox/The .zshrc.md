2026.4.17

This is my Linux Ubuntu 24.04 LTS Terminal zsh setting

```json
export LANG="en_US.UTF-8"
export LANGUAGE="en_US:en"
HISTSIZE=10000
SAVEHIST=10000
HISTFILE=~/.zsh_history

# 自动补全系统初始化
# autoload -Uz compinit && compinit
fpath=(~/.zsh/completion $fpath) # 如果你有自定义补全，在这里添加
autoload -Uz compinit
if [[ -n ~/.zcompdump(#qN.mh-24) ]]; then
    compinit -C
else
    compinit
fi

# 环境变量 & PATH
# export PATH=$PATH:/home/chaser/.local/bin
# 1. 以下部分为zsh特有，如果使用的是非zsh，不使用这部分
typeset -U path
path=($HOME/.local/bin $path)
export PATH
# 2. 如果使用的是非zsh，使用这一部分
# local new_path="/home/chaser/.local/bin"
# case ":$PATH:" in
#     *":$new_path:"*) ;;
#     *) export PATH="$PATH:$new_path" ;;
# esac

# 别名设置 (Aliases)
alias ls='ls --color=auto'
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias grep='grep --color=auto'
alias egrep='grep -E --color=auto'
alias fgrep='grep -F --color=auto'

notify() {
    (( $+commands[notify-send] )) || return
    [[ -n "$DISPLAY" ]] || return
    notify-send "$@"
}

# 代理函数 (Proxy)
function proxy() {

	if ! command -v curl >/dev/null 2>&1; then
	    printf "\033[31m[Proxy]\033[0m curl not installed\n"
	    return 1
	fi

    local proxy_addr="127.0.0.1:7897"
    
    export http_proxy="http://$proxy_addr"
    export https_proxy="http://$proxy_addr"
    export ftp_proxy="http://$proxy_addr"
    export all_proxy="socks5h://$proxy_addr"
	export HTTP_PROXY="$http_proxy"
	export HTTPS_PROXY="$http_proxy"
    export FTP_PROXY="$ftp_proxy"
    export ALL_PROXY="$all_proxy"
    export no_proxy="localhost,127.0.0.1,127.0.1.1,::1,*.local,node,workgroup"
    export NO_PROXY="localhost,127.0.0.1,127.0.1.1,::1,*.local,node,workgroup"

	local git_status="disabled"
	
	if command -v git >/dev/null 2>&1; then
	    git config --global http.proxy "http://$proxy_addr"
	    git config --global https.proxy "http://$proxy_addr"
	    git_status="configured"
	fi
	
	printf "\n\033[1;36m[Proxy]\033[0m Enabled\n"
	# printf " ├─ [*] Engine  -> %s\n" "$engine"
	printf " ├─ [✔] Address -> %s\n" "$proxy_addr"  
	printf " ├─ [✔] Git     -> %s\n" "$git_status"  
	printf " └─ [!] Status  -> testing connection...\n"

    (
	    local ip_info
        ip_info=$(curl -s --connect-timeout 2 --max-time 3 "http://ip-api.com/line?fields=country,city,query" | tr '\n' ' ')
        local http_check
        http_check=$(curl -Is --connect-timeout 3 https://www.google.com | head -n 1)
        
       if [[ $http_check =~ 'HTTP/[0-9.]+ (2|3)[0-9][0-9]' ]]; then
            printf "\033[32m[Proxy]\033[0m ✔ Connected\n"
            [[ -n "$ip_info" ]] && printf " └─ [&] Location : %s\n" "$ip_info"
            # printf " ├─ [&] Location -> %s %s\n" "$country" "$city"
            # printf " ├─ [&] IP       -> %s\n" "$ip"
            # printf " └─ [&] Latency  -> %sms\n" "$latency"
	        notify "Proxy Check" "Google connection successful: $ip_info"
        else
            printf "\033[31m[Proxy]\033[0m [✘] Connection failed\n"
	        notify "Proxy Error" "Unable to access external network."
        fi
        zle && zle reset-prompt
    )
    return 0
}

function proxyoff() {
    unset http_proxy https_proxy ftp_proxy all_proxy
    unset HTTP_PROXY HTTPS_PROXY FTP_PROXY ALL_PROXY
    unset no_proxy NO_PROXY
    
#     local current_git_http=$(git config --global --get http.proxy)
#     local current_git_https=$(git config --global --get https.proxy)
#     local proxy_addr="127.0.0.1:7897"
    
    if command -v git >/dev/null 2>&1; then
        git config --global --unset http.proxy 2>/dev/null
        git config --global --unset https.proxy 2>/dev/null
    fi
    printf "\033[31m[Proxy]\033[0m [✘] Disabled\n"
}

# 5. 工具初始化
# 设置缓存目录
# typeset -U XDG_CACHE_HOME="${XDG_CACHE_HOME:-$HOME/.cache}"
export XDG_CACHE_HOME="${XDG_CACHE_HOME:-$HOME/.cache}"
# local _cache_dir="$XDG_CACHE_HOME/zsh_init_cache"
typeset _cache_dir="$XDG_CACHE_HOME/zsh_init_cache"
[[ -d "$_cache_dir" ]] || mkdir -p "$_cache_dir"

# 运行 fastfetch
if [[ "$SHLVL" == 1 && $- == *i* ]]; then
    command -v fastfetch >/dev/null 2>&1 && fastfetch
fi

# 初始化 zoxide
if (( $+commands[zoxide] )); then
    local _zoxide_cache="$_cache_dir/zoxide.zsh"
    if [[ ! -f "$_zoxide_cache" || "$_zoxide_cache" -ot "$(whence -p zoxide)" ]]; then
        zoxide init zsh >! "$_zoxide_cache"
    fi
    source "$_zoxide_cache"
fi

# 初始化 oh-my-posh
local _omp_config="$HOME/.cache/oh-my-posh/themes/zash.omp.json"
local _omp_cache="$_cache_dir/omp.zsh"

if (( $+commands[oh-my-posh] )); then
	if [[ -f "$_omp_config" ]]; then
	    if [[ ! -f "$_omp_cache" || "$_omp_cache" -ot "$(whence -p oh-my-posh)" || "$_omp_cache" -ot "$_omp_config" ]]; then
	        oh-my-posh init zsh --config "$_omp_config" > "$_omp_cache"
	    fi
	    source "$_omp_cache"
	else
	    echo "[!] oh-my-posh config not found at $_omp_config, skipping."
	fi
else
	echo "[!] oh-my-posh not found, skipping initialization."
fi

unset _cache_dir _zoxide_cache _omp_config _omp_cache

# 6. 插件

# 自动补全
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh

# 语法高亮
source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# 生成随机密码
# Generate random password
loom() {
    local c_info="\033[1;34m"    # Info - blue
    local c_succ="\033[1;32m"    # Success - green
    local c_err="\033[1;31m"     # Error - red
    local c_warn="\033[1;33m"    # Warning - yellow
    local c_reset="\033[0m"      # Reset color

    echo "${c_info}[INFO] Generating random passwords...${c_reset}"
    
    if ! command -v python3 >/dev/null 2>&1; then
	    echo "${c_err}[ERROR] Python3 is not installed. ${c_reset}" >&2
	    return 1
	fi
    
    local pass
    if ! pass=$(python3 -c "import string, secrets, random; charset=''.join(c for c in string.ascii_letters+string.digits if c not in 'l10oO'); chars=[secrets.choice(charset) for _ in range(12)]+[secrets.choice(string.ascii_lowercase),secrets.choice(string.ascii_uppercase),secrets.choice(string.digits),secrets.choice('23456789')]; random.SystemRandom().shuffle(chars); print(''.join(chars))" 2>/dev/null); then
        echo "${c_err}[ERROR] Password generation failed! Please check if Python is installed on your system.${c_reset}" >&2
        return 1
    fi
    
    if [[ -z "$pass" ]]; then
        echo "${c_err}[ERROR] Password generation error, empty result!${c_reset}" >&2
        return 1
    fi
    
    echo "${c_succ}[SUCCESS] Password generated.${c_reset}"
    printf "Password is : ${c_succ}%s${c_reset}\n\n" "$pass"

    if [[ -n "$1" ]]; then
        local file_name="$1"
        local save_path="${2:-$HOME/Documents}"
        
        save_path="${save_path%/}"
        local full_file_path="$save_path/${file_name}.txt"
        
        echo "${c_info}[INFO] Received file save command.${c_reset}"
        echo "${c_info}[INFO] Target file name : ${file_name}.txt${c_reset}"
        echo "${c_info}[INFO] Target directory : ${save_path}${c_reset}"
        
        if [[ ! -d "$save_path" ]]; then
            echo "${c_info}[INFO] The directory does not exist; attempting to create it...${c_reset}"
            if ! { mkdir -p "$save_path"; } 2>/dev/null; then
                echo "${c_err}[ERROR] Directory creation failed! Please check if the path is valid or if you have sufficient permissions (${save_path}).${c_reset}" >&2
                return 1
            fi
            echo "${c_succ}[SUCCESS] The directory was created successfully.${c_reset}"
        fi
        
        echo "${c_info}[INFO] Appending password to file...${c_reset}"
        
        if [[ ! -f "$full_file_path" ]]; then
            touch "$full_file_path" 2>/dev/null
            chmod 600 "$full_file_path" 2>/dev/null
        fi
        
        local current_time=$(date "+%Y-%m-%d %H:%M:%S")
        if ! { printf "[%s] %s\n" "$current_time" "$pass" >> "$full_file_path"; } 2>/dev/null; then
            echo "${c_err}[ERROR] Failed to write to file! Please check file read/write permissions or disk space (${full_file_path}).${c_reset}" >&2
            return 1
        fi
        
        chmod 600 "$full_file_path" 2>/dev/null
        
        echo "${c_succ}[SUCCESS] The password has been securely saved to : ${full_file_path}${c_reset}"
    else
        echo "${c_warn}[WARN] No filename specified. The generated password will not be saved to any file.${c_reset}"
    fi
}
```