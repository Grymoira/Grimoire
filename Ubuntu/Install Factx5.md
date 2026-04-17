
> also, the instruction is based on the Ubuntu 24.04 LTS


Download the `fcitx5` and related components:

```bash
sudo apt update
sudo apt install fcitx5 fcitx5-chinese-addons fcitx5-frontend-gtk4 fcitx5-frontend-qt5 fcitx5-config-qt
```

After that, open the keyboard settings and find the input sources. Make sure only English (US) is listed, and delete any unnecessary Pinyin input sources.

Then, input the command `im-config` in the terminal and click OK and Yes.

Select fcitx5 from the list and click OK. There are some additional configuration options to explore on your own.

# Configure type skin

Can search for the theme file in your browser or on GitHub, then use the command: 

```bash
sudo cp -r ~/Downloads /usr/share/fcitx5/themes
```

Afterwards, need to change the permissions: 

```bash
sudo chmod -R 755 /usr/share/fcitx5/themes
```

Then it should work normally.

For specific applications (VS Code/Chrome),

And need to check your environment variables.
Add the following to the end of your `~/.profile` file:

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
```

After adding this, run:

```bash
source ~/.profile
```

Here's a recommended font: [maple mono](https://github.com/subframe7536/maple-font/releases/download/v7.9/MapleMonoNormal-NF-CN.zip), or you can download it from this page: [github](https://github.com/subframe7536/maple-font/)