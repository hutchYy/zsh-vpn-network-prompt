# zsh-vpn-network-prompt
Guide to add the VPN's network config in the prompt
![](prompt.png)

## Installation
The prompt works for both ZSH "vanilla" and Oh My Zsh,on the other hand the configuration is not the same.

### Oh My Zsh
In Oh My Zsh, it is fairly simple to add the network prompt by editing your current theme file.

First, we need to find what's your current theme:
```Bash
cat ~/.zshrc | grep -E 'ZSH_THEME=".*?"'
```

Then we need to edit it to add our custom prompt code:
```Bash
# Please replace [THEME_NAME] by your theme's name
code ~/.oh-my-zsh/themes/[THEME_NAME].zsh-theme
```
Once open, you can jump to the [`interesting part`](#Adding-the-VPN's-network-config).
### ZSH vanilla
Just open the .zshrc file with your favorite code editor and jump to the [`interesting part`](#Adding-the-VPN's-network-config).

### Adding the VPN's network config
To add the VPN's network config to your prompt you will need to:
1. Add the following function to your file:
    ```Bash
    function _tun_ips() {
    local ipaddr
    ipaddr=$(ip a | grep -E 'tun[0-9]+' | grep -E '([0-9]{1,3}\.){3}[0-9]{1,3}' | awk '{split($0,a," "); print a[5]"("a[2]")"}')
    if [[ $(echo $ipaddr | wc -l) > 1 ]];then
        ipaddr=$(echo -e $ipaddr | sed ':a;N;$!ba;s/\n/,/g')
    fi
    if [[ -n $ipaddr ]]; then
        echo "%{$fg[cyan]%}$ipaddr%{$reset_color%}:"
    fi
    }
    ```
2. Call the previously added function in the prompt by adding \"**\$(\_tun\_ips)**\" in the "PROMPT" variable.

    For e.g., here is my PROMPT variable with the \"**\$(\_tun\_ips)**\":
    ```
    PROMPT='
    $(_user_host)${_current_dir} $(git_prompt_info) $(ruby_prompt_info) $(_tun_ips)
    %{%(!.${fg[red]}.${fg[white]})%}➔%{$reset_color%} '
    ```
4. Reload you zsh by doing:
    ```
    source ~/.zshrc
    ```
3. Enjoy !
    ![](prompt.png)
