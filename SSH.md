# SSH（登录）

#### 基本用法

远程登录服务器：

```shell
ssh user@hostname
```

- ##### <font color='red'>user</font>:用户名 zzk

- ##### <font color='red'>hostname</font>:IP地址或域名 

- ##### <font color='red'>Password</font>:xxxxxxxxx



第一次登录时会提示：

```shell
The authenticity of host '123.57.47.211 (123.57.47.211)' can't be established.
ECDSA key fingerprint is SHA256:iy237yysfCe013/l+kpDGfEG9xxHxm0dnxnAbJTPpG8.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

输入**<font color='red'>yes</font>**，然后回车即可。

这样会将该服务器的信息记录在**<font color='red'>~/.ssh/known_hosts</font>** 文件中。

然后输入密码即可登录到远程服务器中。

默认登录端口号为22。如果想登录某一特定端口：

```shell
ssh user@hostname -p 22
```

#### 配置文件

创建文件 **<font color='red'>~/.ssh/config。</font>** 

然后在文件中输入：

```shell
Host myserver1
    HostName IP地址或域名
    User 用户名

Host myserver2
    HostName IP地址或域名
    User 用户名
```

之后再使用服务器时，可以直接使用别名<font color='red'>**myserver1**</font>、<font color='red'>**myserver2**</font>。

#### 密钥登录

创建密钥：

```shell
ssh-keygen
```

然后一直回车即可。

执行结束后，<font color='red'>**~/.ssh/**</font>目录下会多两个文件：

<font color='red'>**id_rsa**</font>：私钥
<font color='red'>**id_rsa.pub**</font>：公钥

之后想免密码登录哪个服务器，就将公钥传给哪个服务器即可。

例如，想免密登录<font color='oran'>**myserver**</font>服务器。则将公钥中的内容，复制到<font color='oran'>**myserver**</font>中的<font color='red'>**~/.ssh/authorized_keys**</font>文件里即可。

也可以使用如下命令一键添加公钥：

```shell
ssh-copy-id myserver
```

#### 执行命令

命令格式：

```shell
ssh user@hostname command
```

例如：

```shell
ssh user@hostname ls -a
```

或者

```shell
# 单引号中的$i可以求值
ssh myserver 'for ((i = 0; i < 10; i ++ )) do echo $i; done'
```

亦或者（错误示范）

```shell
# 双引号中的$i不可以求值
# 双引号是在本地服务器进行转义了，所以传过去命令不是echo $a，而是echo 1
# 单引号传过去的是echo $a
ssh myserver "for ((i = 0; i < 10; i ++ )) do echo $i; done"
```

# SSH（传文件）

#### 基本用法

命令格式：

```shell
scp source destination
```

将<font color='red'>**source**</font>路径下的文件复制到<font color='red'>**destination**</font>中

一次复制多个文件：

```shell
scp source1 source2 destination
```

复制文件夹：

```shell
scp -r ~/tmp myserver:/home/acs/
```

将本地家目录中的<font color='red'>**tmp**</font>文件夹复制到<font color='red'>**myserver**</font>服务器中的<font color='red'>**/home/acs/**</font>目录下。

```shell
scp -r ~/tmp myserver:homework/
```

将本地家目录中的<font color='red'>**tmp**</font>文件夹复制到<font color='red'>**myserver**</font>服务器中的<font color='red'>**~/homework/**</font>目录下。

```shell
scp -r myserver:homework .
```

将<font color='red'>**myserver**</font>服务器中的<font color='red'>**~/homework/**</font>文件夹复制到本地的当前路径下。

指定服务器的端口号：

```shell
scp -P 22 source1 source2 destination
```

注意： <font color='red'>**scp**</font>的<font color='red'>**myserver**</font>等参数尽量加在<font color='red'>**-r -P**</font>source和<font color='red'>**destination**</font>之前。

使用<font color='red'>**scp**</font>配置其他服务器的<font color='red'>**vim**</font>和<font color='red'>**tmux**</font>

```shell
scp ~/.vimrc ~/.tmux.conf myserver:
```

