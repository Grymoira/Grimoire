
> This instruction is based on Ubuntu 24.04 LTS System Bash Terminal

# 配置

## 安装

```bash
sudo apt install zoxide
```

## 建议同时安装 fzf 

> fxf 是一个开启交互式搜索工具

```bash
sudo apt install fzf
```

## 将其添加到 shell 配置文件中

这里有两种方式

1.

```bash
echo 'eval "$(zoxide init bash)"' >> ~/.bashrc
```

2.

```bash
nano ~/.bashrc
ecval "$(zoxide init bash)"
```

## 使配置生效

```bash
source ~/.bashrc
```

# 使用

```bash
z 任意路径
```

```bash
z pro awe # 进入同时包含pro 和awe 的路径
```

```bash
# 如果无法确定输入哪个关键词，或者有多个候选结果
zi # 这时候会有一个类似搜索框的界面(fxf)
```

```bash
z - # 或到上一个目录
zoxide query -l # 查看数据库中记录的路径和权重
```

