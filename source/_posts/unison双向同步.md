---
title: unison双向同步
abbrlink: 5bfd8747
date: 2019-02-22 16:17:41
categories: 编程
tags: linux
---
### unison双向同步

---
#### 安装
Linux下通过源码包编译安装Unison时,需要用到Objective Caml compiler
```
wget http://caml.inria.fr/pub/distrib/ocaml-4.06/ocaml-4.06.0.tar.gz
tar -zxvf ocaml-4.06.0.tar.gz
cd ocaml-4.06.0
./configure 
make world opt
make install
```
编译安装Unison
```
wget https://github.com/bcpierce00/unison/archive/v2.51.2.tar.gz
tar -zxvf v2.51.2.tar.gz
cd unison-2.51.2/src
make UISTYLE=text
cp ./unison /usr/local/bin
make install
```

#### 设置免密登陆
2台服务器操作相同
```
useradd -m unison
su - unison 
ssh-keygen -t rsa
```
将生成的`/home/unison/.ssh/id_rsa.pub`复制到另一台服务器，然后写入认证文件中
```
cat  ~/id_rsa.pub >> ~/.ssh/authorized_keys
```
如果没有`.ssh`文件夹
```
mkdir ~/.ssh
chmod 700 ~/.ssh
```
重启`ssh`服务
```
systemctl restart sshd
```
测试连接
```
ssh unison@xxx.com -i ~/.ssh/id_rsa
```
#### 同步配置
修改`~/.unison/default.prf`文件
```
root = /unison/test

root = ssh://unison@10.124.5.215//unison/test

#force =/unison/test

servercmd=/usr/local/bin/unison

#ignore =/tmp/test_1/a

batch = true

#repeat = 1

#retry = 3

owner = true

group = true

perms = -1

#fastcheck = false

#rsync = false

sshargs = -C

#xferbycopying = true

confirmbigdel=false

log = true

logfile = /home/unison/.unison/unison.log
```
#### 配置解读
```
两个root表示需要同步的文件夹。

force表示以本地的/unison/test文件夹为标准，将该目录同步到远端,开启后则变成单项同步

ignore = Path表示忽略/unison/test/a目录，即同步时不同步它。

batch=true 表示全自动模式，接受并执行默认动作

log = true表示在终端输出运行信息。

logfile则指定了同时将输出写入log文件。

owner = true //保持同步过来的文件属主 

group = true //保持同步过来的文件组信息 

perms = -1 //保持同步过来的文件读写权限 

repeat = 1 //间隔1秒后,开始新的一次同步检查 

retry = 3 //失败重试 

sshargs = -C //使用ssh的压缩传输方式 

fastcheck true 表示同步时仅通过文件的创建时间来比较，如果选项为false，Unison则将比较两地文件的内容。 

auto //接受缺省的动作，然后等待用户确认是否执行。 

ignore xxx //增加 xxx 到忽略列表中  ：经测试此参数不能用。

ignorecase [true|false|default] //是否忽略文件名大小写 

follow xxx //是否支持对符号连接指向内容的同步 

xferbycopying = true

immutable xxx //不变目录，扫描时可以忽略 

silent //安静模式 

times=true //同步修改时间 

path xxx 参数 //只同步 -path 参数指定的子目录以及文件，而非整个目录，-path 可以多次出现。

confirmbigdel=false//默认值为true，表示当需要同步的两个目录一个为空时，unison将停止，这里设置为false，即便为空unison也不会停止运转
```

#### 测试
```
su unison
unison
```

#### 加入crontab定时任务
```
su unison
crontab -e


0 5 * * * /usr/local/bin/unison
```