# Ansible

[TOC]

## <u>文档标识</u>

| 文档名称 | Ansible  |
| -------- | -------- |
| 版本号   | <V1.0.0> |

## <u>文档修订历史</u>

| 版本   | 日期       | 描述   | 文档所有者 |
| ------ | ---------- | ------ | ---------- |
| V1.0.0 | 2022.11.23 | create | 杨丝雨     |
|        |            |        |            |
|        |            |        |            |

## <u>相关文档参考</u>

[Ansible中文官方文档]: https://cn-ansibledoc.readthedocs.io/zh_CN/latest/



## Ansible简介

Ansible 是一款 IT 自动化工具。主要应用场景有配置系统、软件部署、持续发布及不停服平滑滚动更新的高级任务编排。

Ansible 本身非常简单易用，同时注重安全和可靠性，以最小化变动为特色，使用 OpenSSH 实现数据传输 ( 如果有需要的话也可以使用其它传输模式或者 pull 模式 )，其语言设计非常利于人类阅读，即使是针对不刚接触 Ansible 的新手来讲亦是如此。

我们坚信无论什么范围的环境，简单都是必须的，所以我们的设计尽可能满足各类型的繁忙人群：开发人员、系统管理员、发布工程师、IT 管理员等所有类型的人。同时， Ansible 适用于各种环境，小到几台多到成千上万台的企业实际环境都完全满足。

Ansible 不使用C/S架构管理节点，即没有 Agent 。 这样的架构使得 Ansible 不会存在如何升级远程 Agent 管理进程或者因为没有安装 Agent 而无法管理系统。 因为 OpenSSH 是非常流行的开源组件，安全问题也非常少 。 Ansible 的 去中心化 管理方式深受业内认可， 即它只依赖 OS 的 KEY 认证访问远程主机。 如需， Ansible 可以便捷接入 Kerberos, LDAP 或者其它认证系统。

该文档覆盖了 Ansible 所有版本的文档，可以通过左边栏索引跳转对应页面 「别跳了，就翻译了一个版本，官方更新太快了」。 官方并行管理多个版本 Ansible 及对应版本的文档，所以使用前请确认参考的是对应版本的文档。 最新的功能，我们会在版本中标识新增的功能。

Ansible 主版本大概每年 3-4 个版本。 核心功能应用的发布会很谨慎，每个版本都非常重视代码质量和语言简洁性。 但是， 社区贡献的新模块和组件发展的非常快，每个版本都会合入很多的新模块。

## Playbooks

Playbooks 是 Ansible的配置,部署,编排语言.他们可以被描述为一个需要希望远程主机执行命令的方案,或者一组IT程序运行的命令集合.

如果 Ansible 模块你是工作室中的工具,那么 playbooks 就是你设置的方案计划.

在基础层面, playbooks 可以被用来管理用于部署到远程主机的配置文件.在更高的层面上,playbooks 可以依次对多层式架构上的服务器执行上线包括滚动更新在内的操作并可以将操作委托给其他主机包括在此过程中发生的与监视服务器,负载均衡服务器的交互操作在内.

虽然这里讲发很多,但是不需要立刻一次性全部学完.你可以从小功能开始,当你需要的时候再来这里找对应的功能即可.

Playbooks 被设计的非常简单易懂和基于text language二次开发.有多种办法来组织 playbooks 和其附属的文件,同时我们也会提供一些关于学习 Ansible 的建议.

## Ansible与同类工具的对比

- ansible:精简和快速,无需安装代理
- saltstack:命令行的工具，不同级别主服务器，但系统兼容较差
- puppet:长久，全面。但使用复杂。

## Ansible的安装

Ansible的安装部署非常简单，其仅依赖于 Python 和 SSH，而系统默认均已安装。

Ansible 被 RedHat 红帽官方收购后，其安装源被收录在 epel 中，如已安装 epel 可直接 yum 安装。

通过 pip 和 easy_install 的 Python 第三方包管理器也可以便捷安装 Ansible。

> 推荐使用yum方式安装

### 1.yum安装

```shell
[root@localhost]# yum install epel-release -y   # 安装epel源
[root@localhost]# yum install ansible -y        # yum安装ansible即可，非常简单方便
[root@localhostins]# ansible --version          # 验证安装是否成功
[root@localhost]# ansible help               # help可以查看ansible的命令参数
```

### 2.pip方式安装

```shell
[root@localhost]# yum install gcc glibc-devel zlib-devel rpm-build openssl-devel -y  # 安装前确保服务器的gcc、glibc开发环境均已安装
[root@localhost]# yum install python-pip python-devel -y # 安装 python-pip 及 python-devel 程序包
[root@localhost]# pip install --upgrade pip # 升级本地 pip 至最新版本 
[root@localhost]# pip install --upgrade ansible # 安装ansible
```

### 3.Ansible的配置

|         配置文件         |          作用          |
| :----------------------: | :--------------------: |
| /etc/ansible/ansible.cfg |       主配置文件       |
|    /etc/ansible/hosts    | 机器清单，进行分组管理 |
|   /etc/ansible/roles/    |     存放角色的目录     |

```shell
[root@localhost]# vim /etc/ansible/ansible.cfg   # 修改主配置文件
```

```yaml
[defaults]
#inventory      = /etc/ansible/hosts  					# 默认主机文件
#library        = /usr/share/my_modules/                # 库文件存放目录
#forks          = 5                                     # 默认开启的并发数
#remote_user = root                                     # 默认palybooks用户 
#sudo_user      = root                                  # 默认sudo用户
#ask_sudo_pass = True                                   # 是否需要sudo密码
#ask_pass      = True                                   # 是否需要密码
#remote_port    = 22                                    # 默认远程主机的端口号
#host_key_checking = False                              # 是否检查host_key，推荐关闭
#deprecation_warnings=False      # 是否开启“不建议使用”警告，该类型警告大多属于版本更新方法过时提醒
#command_warnings=False      # 当shell和命令行模块被默认模块简化的时,Ansible 将默认发出警告
#timeout = 10											# 连接主机的时间
#log_path=/var/log/ansible.log                          # 开启ansible日志
#private_key_file = /path/to/file（/root/.ssh/id_rsa）   # 基于密钥认证的文件
[privilege_escalation]                                  # 默认以root身份运行ansible
become=True
become_method=sudo
become_user=root
become_ask_pass=False
```

> 创建配置文件。默认的配置文件是/etc/ansible/ansible.cfg，但是一般不使用它，而是在工作目录下创建自己的配置文件
>
> [root@control ansible]# vim ansible.cfg    # 文件名必须是ansible.cfg
> [defaults]
> inventory = hosts    # 管理的主机，配置在当前目录的hosts文件中，hosts名是自定义的。=号两边空格可有可无。

```yaml
# 创建主机清单文件。写在[]里的是组名，[]下面的是组内的主机名
[root@control ansible]# vim hosts
[test]
node1

[proxy]
node2

[webservers]
node[3:4]     # node3和node4的简化写法，表示从3到4

[database]
node5

# cluster是组名，自定义的；:children是固定写法，表示下面的组名是cluster的子组。
[cluster:children]
webservers
database

# 查看被管理的所有的主机。注意，一定在工作目录下执行命令。
[root@control ansible]# ansible all --list-hosts
  hosts (5):
    node1
    node2
    node3
    node4
    node5
# 查看webservers组中所有的主机
[root@control ansible]# ansible webservers --list-hosts
  hosts (2):
    node3
    node4
```

## Ansible管理

- ansible进行远程管理的两个方法：
  - adhoc临时命令。就是在命令行上执行管理命令。
  - playbook剧本。把管理任务用特定格式写到文件中。
- 无论哪种方式，都是通过模块加参数进行管理。

### adhoc临时命令

- 语法：

```shell
ansible 主机或组列表 -m 模块 -a "参数"    # -a是可选的
```

- 测试到远程主机的连通性

```shell
[root@control ansible]# ansible all -m ping
```

### ansible模块

- 模块基本信息查看

```shell
# 列出ansible的所有模块数量
[root@control ansible]# ansible-doc -l | wc -l
2834
# 列出ansible的所有模块
[root@control ansible]# ansible-doc -l 
# 查看与yum相关的模块
[root@control ansible]# ansible-doc -l | grep yum

# 查看yum模块的使用说明，主要查看下方的EXAMPLE示例
[root@control ansible]# ansible-doc yum
```

- 学习模块，主要知道实现某种功能，需要哪个模块。
- 模块的使用方式都一样。主要是查看该模块有哪些参数。

#### command模块

- ansible默认模块，用于在远程主机上执行任意命令
- command不支持shell特性，如管道、重定向。

```shell
# 在所有被管主机上创建目录/tmp/demo
[root@control ansible]# ansible all -a "mkdir /tmp/demo"

# 查看node1的ip地址
[root@control ansible]# ansible node1 -a "ip a s"
[root@control ansible]# ansible node1 -a "ip a s | head"   # 报错
```

#### shell模块

- 与command模块类似，但是支持shell特性，如管道、重定向。

```shell
# 查看node1的ip地址，只显示前10行
[root@control ansible]# ansible node1 -m shell -a "ip a s | head"
```

#### script模块

- 用于在远程主机上执行脚本

```shell
# 在控制端创建脚本即可
[root@control ansible]# vim test.sh
#!/bin/bash

yum install -y httpd
systemctl start httpd

# 在test组的主机上执行脚本
[root@control ansible]# ansible test -m script -a "test.sh"
```

#### file模块

- 可以创建文件、目录、链接等，还可以修改权限、属性等
- 常用的选项：
  - path：指定文件路径
  - owner：设置文件所有者
  - group：设置文件所属组
  - state：状态。touch表示创建文件，directory表示创建目录，link表示创建软链接，absent表示删除
  - mode：设置权限
  - src：source的简写，源
  - dest：destination的简写，目标

```shell
# 查看使用帮助
[root@control ansible]# ansible-doc file
... ...
EXAMPLES:

- name: Change file ownership, group and permissions  # 忽略
  file:                           # 模块名。以下是它的各种参数
    path: /etc/foo.conf           # 要修改的文件的路径
    owner: foo                    # 文件所有者
    group: foo                    # 文件的所有组
    mode: '0644'                  # 权限
... ...
# 根据上面的example，-m file -a的内容就是doc中把各参数的冒号换成=号

# 在test主机上创建/tmp/file.txt
[root@control ansible]# ansible test -m file -a "path=/tmp/file.txt state=touch"   # touch是指如果文件不存在，则创建

# 在test主机上创建/tmp/demo目录
[root@control ansible]# ansible test -m file -a "path=/tmp/demo state=directory"

# 将test主机上/tmp/file.txt的属主改为sshd，属组改为adm，权限改为0777
[root@control ansible]# ansible test -m file -a "path=/tmp/file.txt owner=sshd group=adm mode='0777'"
[root@control ansible]# ansible test -a "ls -l /tmp/file.txt"

# 删除test主机上/tmp/file.txt
[root@control ansible]# ansible test -m file -a "path=/tmp/file.txt state=absent"    # absent英文缺席的、不存在的

# 删除test主机上/tmp/demo
[root@control ansible]# ansible test -m file -a "path=/tmp/demo state=absent"

# 在test主机上创建/etc/hosts的软链接，目标是/tmp/hosts.txt
[root@control ansible]# ansible test -m file -a "src=/etc/hosts dest=/tmp/hosts.txt state=link"
```

#### copy模块

- 用于将文件从控制端拷贝到被控端
- 常用选项：
  - src：源。控制端的文件路径
  - dest：目标。被控制端的文件路径
  - content：内容。需要写到文件中的内容

```shell
[root@control ansible]# echo "AAA" > a3.txt
# 将a3.txt拷贝到test主机的/root/
[root@control ansible]# ansible test -m copy -a "src=a3.txt dest=/root/"

# 在目标主机上创建/tmp/mytest.txt，内容是Hello World
[root@control ansible]# ansible test -m copy -a "content='Hello World' dest=/tmp/mytest.txt"
```

#### fetch模块

- 与copy模块相反，copy是上传，fetch是下载
- 常用选项：
  - src：源。被控制端的文件路径
  - dest：目标。控制端的文件路径

```shell
# 将test主机上的/etc/hostname下载到本地用户的家目录下
[root@control ansible]# ansible test -m fetch -a "src=/etc/hostname dest=~/"
[root@control ansible]# ls ~/node1/etc/   # node1是test组中的主机
hostname
```

#### lineinfile模块

- 用于确保存目标文件中有某一行内容
- 常用选项：
  - path：待修改的文件路径
  - line：写入文件的一行内容
  - regexp：正则表达式，用于查找文件中的内容

```shell
# test组中的主机，/etc/issue中一定要有一行Hello World。如果该行不存在，则默认添加到文件结尾
[root@control ansible]# ansible test -m lineinfile -a "path=/etc/issue line='Hello World'"

# test组中的主机，把/etc/issue中有Hello的行，替换成chi le ma
[root@control ansible]# ansible test -m lineinfile -a "path=/etc/issue line='chi le ma' regexp='Hello'"
```

#### replace模块

- lineinfile会替换一行，replace可以替换关键词
- 常用选项：
  - path：待修改的文件路径
  - replace：将正则表达式查到的内容，替换成replace的内容
  - regexp：正则表达式，用于查找文件中的内容

```shell
# 把test组中主机上/etc/issue文件中的chi，替换成he
[root@control ansible]# ansible test -m replace -a "path=/etc/issue regexp='chi' replace='he'"
```

#### user模块

- 实现linux用户管理
- 常用选项：
  - name：待创建的用户名
  - uid：用户ID
  - group：设置主组
  - groups：设置附加组
  - home：设置家目录
  - password：设置用户密码
  - state：状态。present表示创建，它是默认选项。absent表示删除
  - remove：删除家目录、邮箱等。值为yes或true都可以。

```shell
# 在test组中的主机上，创建tom用户
[root@control ansible]# ansible test -m user -a "name=tom"

# 在test组中的主机上，创建jerry用户。设置其uid为1010，主组是adm，附加组是daemon和root，家目录是/home/jerry
[root@control ansible]# ansible test -m user -a "name=jerry uid=1010 group=adm groups=daemon,root home=/home/jerry"

# 设置tom的密码是123456
# {{}}是固定格式，表示执行命令。password_hash是函数，sha512是加密算法，则password_hash函数将会把123456通过sha512加密变成tom的密码
[root@control ansible]# ansible test -m user -a "name=tom password={{'123456'|password_hash('sha512')}}"

# 删除tom用户，不删除家目录
[root@control ansible]# ansible test -m user -a "name=tom state=absent"

# 删除jerry用户，同时删除家目录
[root@control ansible]# ansible test -m user -a "name=jerry state=absent remove=yes"
```

#### group模块

- 创建、删除组
- 常用选项：
  - name：待创建的组名
  - gid：组的ID号
  - state：present表示创建，它是默认选项。absent表示删除

```shell
# 在test组中的主机上创建名为devops的组
[root@control ansible]# ansible test -m group -a "name=devops"

# 在test组中的主机上删除名为devops的组
[root@control ansible]# ansible test -m group -a "name=devops state=absent"
```

#### yum_repository

- 用于配置yum
- 常用选项：
  - file： 指定文件名
  - 其他选项，请与文件内容对照

```shell
# 在test组中的主机上，配置yum
[root@control ansible]# ansible test -m yum_repository -a "file=myrepo name=myApp description='My App' baseurl=ftp://192.168.88.240/rhel8/AppStream gpgcheck=no enabled=yes"

[root@node1 ~]# cat /etc/yum.repos.d/myrepo.repo 
[myApp]
baseurl = ftp://192.168.88.240/rhel8/AppStream
enabled = 1
gpgcheck = 0
name = My App

[root@control ansible]# ansible test -m yum_repository -a "file=myrepo name=BaseOS description='Base OS' baseurl=ftp://192.168.88.240/rhel8/BaseOS gpgcheck=no enabled=yes"

[root@node1 ~]# cat /etc/yum.repos.d/myrepo.repo 
[myApp]
baseurl = ftp://192.168.88.240/rhel8/AppStream
enabled = 1
gpgcheck = 0
name = My App

[BaseOS]
baseurl = ftp://192.168.88.240/rhel8/BaseOS
enabled = 1
gpgcheck = 0
name = Base OS
```

#### yum模块

- 用于rpm软件包管理，如安装、升级、卸载
- 常用选项：
  - name：包名
  - state：状态。present表示安装，如果已安装则忽略；latest表示安装或升级到最新版本；absent表示卸载。

```shell
# 在test组中的主机上安装tar
[root@control ansible]# ansible test -m yum -a "name=tar state=present"

# 在test组中的主机上安装wget、net-tools
[root@control ansible]# ansible test -m yum -a "name=wget,net-tools"

# 在test组中的主机上卸载wget
[root@control ansible]# ansible test -m yum -a "name=wget state=absent"
```

#### service模块

- 用于控制服务。启动、关闭、重启、开机自启。
- 常用选项：
  - name：控制的服务名
  - state：started表示启动；stopped表示关闭；restarted表示重启
  - enabled：yes表示设置开机自启；no表示设置开机不要自启。

```shell
# 在test主机上安装httpd
[root@control ansible]# ansible test -m yum -a "name=httpd state=latest"

#  在test主机上启动httpd，并设置它开机自启
[root@control ansible]# ansible test -m service -a "name=httpd state=started enabled=yes"
```

#### 逻辑卷相关模块

- 逻辑卷可以动态管理存储空间。可以对逻辑卷进行扩容或缩减。
- 可以把硬盘或分区转换成物理卷PV；再把1到多个PV组合成卷组VG；然后在VG上划分逻辑卷LV。LV可以像普通分区一样，进行格式化、挂载。
- 关闭虚拟机node1，为其添加2块20GB的硬盘
- LINUX下KVM虚拟机新加的硬盘，名称是`/dev/vdb`和`/dev/vdc`
- vmware虚拟机新加的硬盘，名称是`/dev/sdb`和`/dev/sdc`
- 如果选nvme硬盘，名称可能是`/dev/nvme0n1`和`/dev/nvme0n2`

```shell
[root@node1 ~]# lsblk    # 可以查看到新加的硬盘vdb和vdc
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0     11:0    1 1024M  0 rom  
vda    253:0    0   30G  0 disk 
`-vda1 253:1    0   20G  0 part /
vdb    253:16   0   20G  0 disk 
vdc    253:32   0   20G  0 disk 
```

##### lvg模块

- 创建、删除卷组，修改卷组大小
- 常用选项：
  - vg：定义卷组名。vg：volume group
  - pvs：由哪些物理卷构成。pvs：physical volumes

```shell
# 在test组中的主机上安装lvm2，state不写，默认是present
[root@control ansible]# ansible test -m yum -a "name=lvm2"

# 手工在node1上对vdb进行分区
[root@node1 ~]# fdisk /dev/vdb
Command (m for help): g    # 创建GPT分区表
Command (m for help): n    # 新建分区
Partition number (1-128, default 1):    # 回车，使用1号分区
First sector (2048-41943006, default 2048):   # 起始位置，回车
Last sector, +sectors or +size{K,M,G,T,P} (2048-41943006, default 41943006): +5G   # 结束位置+5G

Command (m for help): n   # 新建分区
Partition number (2-128, default 2):   # 回车，使用2号分区
First sector (10487808-41943006, default 10487808): # 起始位置，回车
Last sector, +sectors or +size{K,M,G,T,P} (10487808-41943006, default 41943006): # 结束位置，回车，分区到结尾
Command (m for help): w   # 存盘

[root@node1 ~]# lsblk    # vdb被分出来了两个分区
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0     11:0    1 1024M  0 rom  
vda    253:0    0   30G  0 disk 
`-vda1 253:1    0   20G  0 part /
vdb    253:16   0   20G  0 disk 
|-vdb1 253:17   0    5G  0 part 
`-vdb2 253:18   0   15G  0 part 
vdc    253:32   0   20G  0 disk 

# 在test组中的主机上创建名为myvg的卷组，该卷组由/dev/vdb1组成
[root@control ansible]# ansible test -m lvg -a "vg=myvg pvs=/dev/vdb1"

# 在node1上查看卷组
[root@node1 ~]# vgs
  VG   #PV #LV #SN Attr   VSize  VFree 
  myvg   1   0   0 wz--n- <5.00g <5.00g

# 扩容卷组。卷组由PV构成，只要向卷组中加入新的PV，即可实现扩容
[root@control ansible]# ansible test -m lvg -a "vg=myvg pvs=/dev/vdb1,/dev/vdb2"

[root@node1 ~]# vgs  # 在node1上查看卷组
  VG   #PV #LV #SN Attr   VSize  VFree 
  myvg   2   0   0 wz--n- 19.99g 19.99g
```

##### lvol模块

- 创建、删除逻辑卷，修改逻辑卷大小
- 常用选项：
  - vg：指定在哪个卷组上创建逻辑卷
  - lv：创建的逻辑卷名。lv：logical volume
  - size：逻辑卷的大小，不写单位，以M为单位

```shell
# 在test组中的主机上创建名为mylv的逻辑卷，大小为2GB
[root@control ansible]# ansible test -m lvol -a "vg=myvg lv=mylv size=2G"

# 在node1上查看逻辑卷
[root@node1 ~]# lvs
  LV   VG   Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mylv myvg -wi-a----- 2.00g   
  
# mylv扩容至4GB
[root@control ansible]# ansible test -m lvol -a "vg=myvg lv=mylv size=4G"

[root@node1 ~]# lvs  # 在node1上查看逻辑卷
  LV   VG   Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mylv myvg -wi-a----- 4.00g   
```

#### filesystem模块

- 用于格式化，也就是创建文件系统
- 常用选项：
  - fstype：指定文件系统类型
  - dev：指定要格式化的设备，可以是分区，可以是逻辑卷

```shell
#  在test组中的主机上，把/dev/myvg/mylv格式化为xfs
[root@control ansible]# ansible test -m filesystem -a "fstype=xfs dev=/dev/myvg/mylv"

# 在node1上查看格式化结果
[root@node1 ~]# blkid /dev/myvg/mylv
/dev/myvg/mylv: UUID="46c0af72-e517-4b15-9e53-ec72fbe1d96e" TYPE="xfs"
```

#### mount模块

- 用于挂载文件系统
- 常用选项：
  - path：挂载点。如果挂载点不存在，自动创建。
  - src：待挂载的设备
  - fstype：文件系统类型
  - state：mounted，表示永久挂载

```shell
# 在test组中的主机上，把/dev/myvg/mylv永久挂载到/data
[root@control ansible]# ansible test -m mount -a "path=/data src=/dev/myvg/mylv state=mounted fstype=xfs"

# 在node1上查看
[root@node1 ~]# tail -1 /etc/fstab 
/dev/myvg/mylv /data xfs defaults 0 0
[root@node1 ~]# df -h /data/
Filesystem             Size  Used Avail Use% Mounted on
/dev/mapper/myvg-mylv  4.0G   61M  4.0G   2% /data


# 在test组中的主机上，卸载/dev/myvg/mylv
[root@control ansible]# ansible test -m mount -a "path=/data state=absent"

# 在test组中的主机上，强制删除/dev/myvg/mylv
[root@control ansible]# ansible test -m lvol -a "lv=mylv state=absent vg=myvg force=yes"   # force是强制

# 在test组中的主机上，删除myvg卷组
[root@control ansible]# ansible test -m lvg -a "vg=myvg state=absent"
```

## Playbook剧本

- 常用于复杂任务的管理，以及管理经常要完成的任务
- playbook也是通过模块和它的参数，在特定主机上执行任务
- playbook是一个文件，该文件中需要通过yaml格式进行书写

### YAML

- YAML Ain't a Markup Language：YAML不是一个标记语言

#### yaml语法规范

1. yaml文件的文件名，一般以yml或yaml作为扩展名
2. 文件一般以`---`作为第一行，不是必须的，但是常用
3. 键值对使用冒号`:`表示，冒号后面必须有空格。
4. 数组使用`-`表示，`-`后面必须有空格。
5. 相同的层级必须有相同的缩进。如果缩进不对，则有语法错误。每一级缩进，建议2个空格。
6. 全文不能使用tab，必须使用空格。

#### 配置vim适应yaml语法

```shell
# 文件位置和名字是固定的，用于设置vim的格式
[root@control ansible]# vim ~/.vimrc
set ai        # 设置自动缩进
set ts=2      # 设置按tab键，缩进2个空格
set et        # 将tab转换成相应个数的空格
```

### 编写playbook

- 一个剧本（即playbook），可以包含多个play
- 每个play用于在指定的主机上，通过模块和参数执行相应的任务
- 每个play可以包含多个任务。
- 任务有模块和参数构成。

```yaml
---
- 名字: 猴王初问世
  职员表: 猴哥, 大马猴
  场景:
      - 名字: 石头裂开了
      
      - 名字: 天宫震颤了

- 名字: 官封弼马温
  职员表: 猴哥, 玉皇大帝
  场景:
      - 名字: 太白金星骗猴哥
      
      - 名字: 猴哥天宫放马
```

```shell
# 编写用于测试连通性的playbook，相当于执行ansible all -m ping
[root@control ansible]# vim test.yml
---
- hosts: all
  tasks:
    - ping:

[root@control ansible]# ansible-playbook test.yml  # 执行playbook

# 以上更规范的写法如下：
[root@control ansible]# vim test.yml
---
- name: test network    # play的名字，可选项
  hosts: all            # 作用于所有的主机
  tasks:                # 任务
    - name: task 1      # 第1个任务的名字，可选项
      ping:             # 第1个任务使用的模块

[root@control ansible]# ansible-playbook test.yml  # 执行playbook


# 在test组的主机和node2上创建/tmp/demo目录，权限是0755。将控制端/etc/hosts拷贝到目标主机的/tmp/demo中
[root@control ansible]# vim fileop.yml
---
- name: create dir and copy file
  hosts: test,node2    # 这里的名称，必须出现在主机清单文件中
  tasks:
    - name: create dir
      file:
        path: /tmp/demo
        state: directory
        mode: '0755'
    
    - name: copy file
      copy:
        src: /etc/hosts
        dest: /tmp/demo/hosts

# 执行playbook
[root@control ansible]# ansible-playbook fileop.yml


# 在test组中的主机上，创建用户bob，附加组是adm；在node2主机上，创建/tmp/hi.txt，其内容为Hello World.
[root@control ansible]# vim two.yml
---
- name: create user
  hosts: test
  tasks:
    - name: create bob
      user:
        name: bob
        groups: adm

- name: create file
  hosts: node2
  tasks:
    - name: make file
      copy:
        dest: /tmp/hi.txt
        content: "Hello World"

[root@control ansible]# ansible-playbook two.yml
```

- `|`和`>`的区别：`|`它保留换行符，`>`把多行合并为一行

```shell
# 通过copy模块创建/tmp/1.txt，文件中有两行内容，分别是Hello World和ni hao
[root@control ansible]# vim f1.yml
---
- name: play 1
  hosts: test
  tasks:
    - name: mkfile 1.txt
      copy:
        dest: /tmp/1.txt
        content: |
          Hello World!
          ni hao.

[root@control ansible]# ansible-playbook f1.yml
# 查看结果
[root@node1 ~]# cat /tmp/1.txt 
Hello World!
ni hao.


# 通过copy模块创建/tmp/2.txt，文件中有一行内容，分别是Hello World! ni hao
[root@control ansible]# vim f2.yml 
---
- name: play 1
  hosts: test
  tasks:
    - name: mkfile 2.txt
      copy:
        dest: /tmp/2.txt
        content: >
          Hello World!
          ni hao.

[root@control ansible]# ansible-playbook f2.yml
[root@node1 ~]# cat /tmp/2.txt 
Hello World! ni hao.
```

- playbook示例

```shell
# 在test组中的主机上创建john用户，它的uid是1040，主组是daemon，密码为123
[root@control ansible]# vim user_john.yml
---
- name: create user
  hosts: test
  tasks:
    - name: create user john
      user:
        name: john
        uid: 1040
        group: daemon
        password: "{{'123'|password_hash('sha512')}}"
[root@control ansible]# ansible-playbook user_john.yml

# 在test组中的主机上删除用户john
[root@control ansible]# vim del_john.yml
---
- name: delete user
  hosts: test
  tasks:
    - name: delete user john
      user:
        name: john
        state: absent
[root@control ansible]# ansible-playbook del_john.yml
```

#### 硬盘管理

- 常用的分区表类型有：MBR（主引导记录）、GPT（GUID分区表）
- MBR最多支持4个主分区，或3个主分区加1个扩展分区。最大支持2.2TB左右的硬盘
- GPT最多支持128个主分区。支持大硬盘

##### parted模块

- 用于硬盘分区管理
- 常用选项：
  - device：待分区的设备
  - number：分区编号
  - state：present表示创建，absent表示删除
  - part_start：分区的起始位置，不写表示从开头
  - part_end：表示分区的结束位置，不写表示到结尾

```shell
# 在test组中的主机上，对/dev/vdc进行分区，创建1个1GB的主分区
[root@control ansible]# vim disk.yml
---
- name: disk manage
  hosts: test
  tasks:
    - name: create a partition
      parted:
        device: /dev/vdc
        number: 1
        state: present
        part_end: 1GiB

[root@control ansible]# ansible-playbook disk.yml

# 在目标主机上查看结果
[root@node1 ~]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
.. ...
vdc    253:32   0   20G  0 disk 
`-vdc1 253:33   0 1023M  0 part 

# 继续编辑disk.yml，对/dev/vdc进行分区，创建1个新的5GB的主分区
[root@control ansible]# vim disk.yml 
... ...
    - name: add a new partition
      parted:
        device: /dev/vdc
        number: 2
        state: present
        part_start: 1GiB
        part_end: 6GiB

[root@control ansible]# ansible-playbook disk.yml 
[root@node1 ~]# lsblk 
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
... ...
vdc    253:32   0   20G  0 disk 
|-vdc1 253:33   0 1023M  0 part 
`-vdc2 253:34   0    5G  0 part 

# 继续编辑disk.yml，创建名为my_vg的卷组，它由上面创建的vdc1和vdc2构成
[root@control ansible]# vim disk.yml 
... ...
    - name: create my_vg
      lvg:
        vg: my_vg
        pvs: /dev/vdc1,/dev/vdc2

# 继续编辑disk.yml，在my_vg卷组上创建名为my_lv的逻辑卷，大小1G
[root@control ansible]# vim disk.yml 
... ...
    - name: create my_lv
      lvol:
        vg: my_vg
        lv: my_lv
        size: 1G


# 继续编辑disk.yml，格式化my_lv为ext4
[root@control ansible]# vim disk.yml 
... ...
    - name: mkfs my_lv
      filesystem:
        dev: /dev/my_vg/my_lv
        fstype: ext4


# 继续编辑disk.yml，将my_lv挂载到/data
[root@control ansible]# vim disk.yml 
... ...
    - name: mount my_lv
      mount:
        path: /data
        src: /dev/my_vg/my_lv
        fstype: ext4
        state: mounted

# 完整的disk.yml如下
---
- name: disk manage
  hosts: test
  tasks:
    - name: create a partition
      parted:
        device: /dev/vdc
        number: 1
        state: present
        part_end: 1GiB

    - name: add a new partition
      parted:
        device: /dev/vdc
        number: 2
        state: present
        part_start: 1GiB
        part_end: 6GiB

    - name: create my_vg
      lvg:
        vg: my_vg
        pvs: /dev/vdc1,/dev/vdc2

    - name: create my_lv
      lvol:
        vg: my_vg
        lv: my_lv
        size: 1G
        
    - name: mkfs my_lv
      filesystem:
        dev: /dev/my_vg/my_lv
        fstype: ext4

    - name: mount my_lv
      mount:
        path: /data
        src: /dev/my_vg/my_lv
        fstype: ext4
        state: mounted

```

#### 模块

```shell
# 在test组中的主机上，安装httpd、php、php-mysqlnd
[root@control ansible]# vim pkg.yml
---
- name: install pkgs
  hosts: test
  tasks:
    - name: install web pkgs  # 此任务通过yum安装三个包
      yum:
        name: httpd,php,php-mysqlnd
        state: present
# 安装多个软件包，还可以写为：
---
- name: install pkgs
  hosts: test
  tasks:
    - name: install web pkgs
      yum:
        name: [httpd,php,php-mysqlnd]
        state: present

# 安装多个软件包，还可以写为：
---
- name: install pkgs
  hosts: test
  tasks:
    - name: install web pkgs
      yum:
        name: 
          - httpd
          - php
          - php-mysqlnd
        state: present

# 根据功能等，可以将一系列软件放到一个组中，安装软件包组，将会把很多软件一起安装上。比如gcc、java等都是开发工具，安装开发工具包组，将会把它们一起安装。
[root@node1 ~]# yum grouplist   # 列出所有的软件包组
[root@node1 ~]# yum groupinstall "Development Tools"
# 如果列出的组名为中文，可以这样进行：
[root@node1 ~]# LANG=C yum grouplist

# 继续编辑pkg.yml，在test组中的主机上安装Development tools组
[root@control ansible]# vim pkg.yml
---
- name: install pkgs
  hosts: test
  tasks:
    - name: install web pkgs    # 此任务通过yum安装三个包
      yum:
        name:
          - httpd
          - php
          - php-mysqlnd
        state: present

    - name: install dev group    # 此任务通过yum安装一组包
      yum:
        name: "@Development Tools"   # @表示后面的名字是组名
        state: present

[root@control ansible]# ansible-playbook pkg.yml

# 系统升级命令
[root@control ansible]# yum update
# 继续编辑pkg.yml，在test组中的主机上升级所有的包到最新版本
---
- name: install pkgs
  hosts: test
  tasks:
    - name: install web pkgs
      yum:
        name:
          - httpd
          - php
          - php-mysqlnd
        state: present

    - name: install dev group
      yum:
        name: "@Development Tools"
        state: present

    - name: update system    # 相当于yum update命令
      yum:
        name: "*"       # 表示系统已经安装的所有包
        state: latest

[root@control ansible]# ansible-playbook pkg.yml
```

## Ansible变量

### facts变量

- facts翻译过来就是事实。
- facts变量是ansible自带的预定义变量，用于描述被控端软硬件信息。
- facts变量通过setup模块获得。

```shell
# 通过setup模块查看所有facts变量
[root@control ansible]# ansible test -m setup
```

- facts变量是一个大的由`{}`构成的键值对字典。在`{}`中，有很多层级的嵌套。可以通过参数过滤出第一个层级的内容。

```shell
# 查看所有的IPV4地址，filter是过滤的意思
[root@control ansible]# ansible test -m setup -a "filter=ansible_all_ipv4_addresses"

# 查看可用内存
[root@control ansible]# ansible test -m setup -a "filter=ansible_memfree_mb"
```

- 常用的facts变量
  - ansible_all_ipv4_addresses：所有的IPV4地址
  - ansible_bios_version：BIOS版本信息
  - ansible_memtotal_mb：总内存大小
  - ansible_hostname：主机名

- 在playbook中使用变量

```shell
# 显示远程主机的主机名和内存大小。在ansible中，变量使用{{}}表示
# debug模块用于输出信息，常用的参数是msg，用于输出指定内容
[root@control ansible]# vim debug.yml
---
- name: display host info
  hosts: test
  tasks:
    - name: display hostname and memory
      debug:    # debug是模块，它的选项msg可以输出指定信息
        msg: "hostname: {{ansible_hostname}}; mem: {{ansible_memtotal_mb}} MB"

[root@control ansible]# ansible-playbook debug.yml
```

### 自定义变量

- 引入变量，可以方便Playbook重用。比如装包的playbook，包名使用变量。多次执行playbook，只要改变变量名即可，不用编写新的playbook。
- ansible支持10种以上的变量定义方式。常用的变量来源如下：
  - inventory变量。变量来自于主机清单文件
  - facts变量。
  - playbook变量。变量在playbook中定义。
  - 变量文件。专门创建用于保存变量的文件。推荐变量写入单独的文件。

```shell
# 使用inventory变量。
[root@control ansible]# vim hosts
[test]
node1 iname="nb"     # 主机变量定义的方法。iname是自定义名称

[proxy]
node2

[webservers]
node[3:4]

[database]
node5

[cluster:children]
webservers
database

[webservers:vars]       # 组变量定义方法。:vars是固定格式
iname="dachui"

# 通过变量创建用户
[root@control ansible]# vim var1.yml
---
- name: test create user
  hosts: test
  tasks:
    - name: create user
      user:
        name: "{{iname}}"
        state: present
        
- name: create user in webservers
  hosts: webservers
  tasks:
    - name: create some users
      user:
        name: "{{iname}}"
        state: present

[root@control ansible]# ansible-playbook var1.yml

# 上述两个play也可以合并为一个，如下：
[root@control ansible]# vim var1.yml
---
- name: test create user
  hosts: test,webservers    # 指定执行的目标是test组和webservers组
  tasks:
    - name: create user
      user:
        name: "{{iname}}"
        state: present
        

# 在playbook中定义变量
# 在test组中的主机上创建用户jack，他的密码是123456
[root@control ansible]# vim user_jack.yml
---
- name: create user
  hosts: test
  vars:    # 固定格式，用于声明变量
    username: "jack"    # 此处引号可有可无
    mima: "123456"      # 此处引号是需要的，表示数字字符
  tasks:
    - name: create some users
      user:
        name: "{{username}}"   # {}出现在开头，必须有引号
        state: present
        password: "{{mima|password_hash('sha512')}}"

[root@control ansible]# ansible-playbook user_jack.yml


# 将变量定义在文件中
[root@control ansible]# vim vars.yml   # 文件名自定义
---
yonghu: rose
mima: abcd
[root@control ansible]# vim user_rose.yml 
---
- name: create user
  hosts: test
  vars_files: vars.yml   # vars_files用于声明变量文件
  tasks:
    - name: create some users
      user:
        name: "{{yonghu}}"   # 这里的变量来自于vars.yml
        state: present
        password: "{{mima|password_hash('sha512')}}"

[root@control ansible]# ansible-playbook user_rose.yml 
```

## 补充模块

### firewalld模块

- 用于配置防火墙的模块
- 常用选项：
  - port：声明端口
  - permanent：永久生效，但不会立即生效
  - immediate：立即生效，临时生效
  - state：enabled，放行；disabled拒绝
- 防火墙一般默认拒绝，明确写入允许的服务。
- 有一些服务有名字，有些服务没有名字。但是最终都是基于TCP或UDP的某些端口。比如http服务基于TCP80端口。服务名和端口号对应关系的说明文件是：`/etc/services`
- 配置服务器的防火墙，一般来说只要配置开放哪些服务或端口即可。没有明确开放的，都默认拒绝。

- 应用
  - 在test组中的主机上安装并启动httpd
  - 客户端访问服务器的http服务
  - 在test组中的主机上安装并启动firewalld
  - 客户端访问服务器的http服务
  - 在test组中的主机上开放http服务

```shell
# 配置httpd服务
[root@control ansible]# vim firewall.yml
---
- name: configure test
  hosts: test
  tasks:
    - name: install httpd pkg   # 这里通过yum模块装httpd
      yum:
        name: httpd
        state: present

    - name: start httpd service   # 这里通过service模块启httpd服务
      service:
        name: httpd
        state: started
        enabled: yes
        
[root@control ansible]# ansible-playbook firewall.yml
[root@control ansible]# curl http://192.168.88.11/  # 可访问

# 安装并启动firewalld
[root@control ansible]# vim firewall.yml
---
- name: configure test
  hosts: test
  tasks:
    - name: install httpd pkg   # 这里通过yum模块装httpd
      yum:
        name: httpd
        state: present

    - name: start httpd service   # 这里通过service模块启httpd服务
      service:
        name: httpd
        state: started
        enabled: yes
  
    - name: install firewalld pkg   # 这里通过yum模块装firewalld
      yum:
        name: firewalld
        state: present

    - name: start firewalld service   # 这里通过service模块启firewalld服务
      service:
        name: firewalld
        state: started
        enabled: yes
  
[root@control ansible]# ansible-playbook firewall.yml
[root@control ansible]# curl http://192.168.88.11/  # 被拒绝
curl: (7) Failed to connect to 192.168.88.11 port 80: 没有到主机的路由

# 配置防火墙规则，放行http协议
[root@control ansible]# vim firewall.yml
---
- name: configure test
  hosts: test
  tasks:
    - name: install httpd pkg   # 这里通过yum模块装httpd
      yum:
        name: httpd
        state: present

    - name: start httpd service   # 这里通过service模块启httpd服务
      service:
        name: httpd
        state: started
        enabled: yes
  
    - name: install firewalld pkg   # 这里通过yum模块安装firewalld
      yum:
        name: firewalld
        state: present

    - name: start firewalld service   # 这里通过service模块启service服务
      service:
        name: firewalld
        state: started
        enabled: yes
  
    - name: set firewalld rules   # 通过firewalld模块开放80端口
      firewalld:
        port: 80/tcp
        permanent: yes
        immediate: yes
        state: enabled

[root@control ansible]# ansible-playbook firewall.yml 
[root@control ansible]# curl http://192.168.88.11/  # 可访问
```

### template模块

- copy模块可以上传文件，但是文件内容固定
- template模块可以上传具有特定格式的文件（如文件中包含变量）
- 当远程主机接收到文件之后，文件中的变量将会变成具体的值 
- template模块上传的文件，使用的语法叫Jinja2。
- 常用选项：
  - src：要上传的文件
  - dest：目标文件路径

```shell
# 使用template模块将含有变量的文件上传到test组中的主机
[root@control ansible]# vim index.j2
Welcome to {{ansible_hostname}} on {{ansible_eth0.ipv4.address}}

[root@control ansible]# vim templ.yml
---
- name: upload index
  hosts: test
  tasks:
    - name: create web index
      template:
        src: index.j2
        dest: /var/www/html/index.html

[root@control ansible]# ansible-playbook templ.yml
[root@control ansible]# curl http://192.168.88.11/
Welcome to node1 on 192.168.88.11
[root@node1 ~]# cat /var/www/html/index.html 
Welcome to node1 on 192.168.88.11
```

## 进阶语法

### 错误处理

- 当Playbook中包含很多任务时，当某一个任务遇到错误，它将崩溃，终止执行

```shell
# 在test组中的主机上启动mysqld服务，然后创建/tmp/service.txt
# 因为目标主机上没有mysqld服务，所以它将崩溃，终止执行。即，不会创建/tmp/service.txt文件
[root@control ansible]# vim myerr.yml
---
- name: my errors
  hosts: test
  tasks:
    - name: start mysqld service  # 通过service模块启动mysqld服务
      service:
        name: mysqld
        state: started
        enabled: yes
        
    - name: touch a file   # 通过file模块创建文件
      file:
        path: /tmp/service.txt
        state: touch

# 执行playbook，第1个任务就会失败
[root@control ansible]# ansible-playbook myerr.yml
# 到node1上查看，因为第2个任务没有执行，所以文件不会创建
[root@node1 ~]# ls /tmp/service.txt
ls: cannot access '/tmp/service.txt': No such file or directory
```

- 可以指定某一个任务如果出现错误，则忽略它

```shell
# 编辑myerr.yml，如果myslqd服务无法启动，则忽略它
[root@control ansible]# vim myerr.yml
---
- name: my errors
  hosts: test
  tasks:
    - name: start mysqld service
      service:
        name: mysqld
        state: started
        enabled: yes
      ignore_errors: yes    # 即使这个任务失败了，也要继续执行下去

    - name: touch a file
      file:
        path: /tmp/service.txt
        state: touch

[root@control ansible]# ansible-playbook myerr.yml
[root@node1 ~]# ls /tmp/service.txt   # 第2个任务已执行
/tmp/service.txt
```

- 通过全局设置，无论哪个任务出现问题，都要忽略

```shell
[root@control ansible]# vim myerr.yml
---
- name: my errors
  hosts: test
  ignore_errors: yes
  tasks:
    - name: start mysqld service
      service:
        name: mysqld
        state: started
        enabled: yes

    - name: touch a file
      file:
        path: /tmp/mysql.txt
        state: touch

[root@control ansible]# ansible-playbook myerr.yml
[root@node1 ~]# ls /tmp/mysql.txt 
/tmp/mysql.txt
```

### 触发执行任务

- 通过handlers定义触发执行的任务
- handlers中定义的任务，不是一定会执行的
- 在tasks中定义的任务，通过notify关键通知handlers中的哪个任务要执行
- 只有tasks中的任务状态是changed才会进行通知。

```shell
# 下载node1上的/etc/httpd/conf/httpd.conf
[root@control ansible]# vim get_conf.yml
---
- name: download httpd.conf
  hosts: test
  tasks:
    - name: get httpd.conf
      fetch:
        src: /etc/httpd/conf/httpd.conf
        dest: ./
        flat: yes    # 直接下载文件，不要目录
[root@control ansible]# ansible-playbook get_conf.yml

# 修改httpd.conf的端口为变量
[root@control ansible]# vim +45 httpd.conf
... ...
Listen {{http_port}}
... ...

# 修改httpd服务的端口为8000，重启httpd
[root@control ansible]# vim trigger.yml
---
- name: configure httpd
  hosts: test
  vars:
    http_port: "8000"   # 定义httpd.conf中的变量和值
  tasks:
    - name: upload httpd.conf   # 上传httpd.conf
      template:
        src: ./httpd.conf
        dest: /etc/httpd/conf/httpd.conf

    - name: restart httpd    # 重启服务
      service:
        name: httpd
        state: restarted
# 第一次执行trigger.yml，上传文件和重启服务两个任务的状态都是黄色changed
[root@control ansible]# ansible-playbook trigger.yml
# 第二次执行trigger.yml，上传文件的任务状态是绿色的ok，重启服务任务的状态是黄色changed
[root@control ansible]# ansible-playbook trigger.yml

# 既然配置文件没有改变，那么服务就不应该重启
# 修改Playbook，只有配置文件变化了，才重启服务
[root@control ansible]# vim trigger.yml
---
- name: configure httpd
  hosts: test
  vars:
    http_port: "80"
  tasks:
    - name: upload httpd.conf
      template:
        src: ./httpd.conf
        dest: /etc/httpd/conf/httpd.conf
      notify: restart httpd   # 通知restart httpd需要执行

  handlers:
    - name: restart httpd
      service:
        name: httpd
        state: restarted
# 第一次运行Playbook，因为第1个任务是黄色的changed，所以handlers中的任务也被触发执行
[root@control ansible]# ansible-playbook trigger.yml 
# 第二次运行Playbook，因为第1个任务是绿色的OK，也就不会再触发执行其他任务了
[root@control ansible]# ansible-playbook trigger.yml 

```

### when条件

- 只有满足某一条件时，才执行任务
- 常用的操作符：
  - ==：相等
  - !=：不等
  - `>`：大于
  - `<`：小于
  - `<=`：小于等于
  - `>=`：大于等于

- 多个条件或以使用and或or进行连接
- when表达式中的变量，可以不使用`{{}}`

```shell
# 当test组中的主机内存大于2G的时候，才安装mariadb-server
[root@control ansible]# vim when1.yml
---
- name: install mariadb
  hosts: test
  tasks:
    - name: install mariadb pkg
      yum:
        name: mariadb-server
        state: present
      when: ansible_memtotal_mb>2048

# 如果目标主机没有2GB内存，则不会安装mariadb-server
[root@control ansible]# ansible-playbook when1.yml



# 多条件。系统发行版是RedHat8才执行任务
# /etc/motd中的内容，将会在用户登陆时显示在屏幕上
[root@control ansible]# vim motd
 _____________
< hello world >
 -------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

[root@control ansible]# vim when2.yml
---
- name: when condition
  hosts: test
  tasks:
    - name: modify /etc/motd
      copy:
        dest: /etc/motd
        src: motd
      when: >     # 以下三行合并成一行
        ansible_distribution == "RedHat"
        and
        ansible_distribution_major_version == "8"

[root@control ansible]# ansible-playbook when2.yml
```

cowsay:

```shell
[root@node1 ~]# yum install -y cowsay-3.04-4.el7.noarch.rpm
[root@node1 ~]# cowsay hello world    # 默认是奶牛形象
[root@node1 ~]# cowsay -l   # 查看可用形象
[root@node1 ~]# cowsay -f sheep hello world
[root@node1 ~]# cowsay -l > ss.txt
[root@node1 ~]# vim ss.txt    # 删除第1行的说明
[root@node1 ~]# for i in $(cat ss.txt)
> do
> echo "--------------$i----------------"
> cowsay -f $i hello
> sleep 3
> echo "--------------------------------"
> done
```

## 任务块

- 可以通过block关键字，将多个任务组合到一起
- 可以将整个block任务组，一起控制是否要执行

```shell
# 如果test组中的主机系统发行版是RedHat，则安装并启动httpd
[root@control ansible]# vim block1.yml
---
- name: block tasks
  hosts: test
  tasks:
    - name: define a group of tasks
      block:
        - name: install httpd   # 通过yum安装httpd
          yum:
            name: httpd
            state: present

        - name: start httpd     # 通过service启动httpd服务
          service:
            name: httpd
            state: started
            enabled: yes

      when: ansible_distribution=="RedHat"   # 条件为真才会执行上面的任务

[root@control ansible]# ansible-playbook block1.yml
```

### rescue和always

- block和rescue、always联合使用：
  - block中的任务都成功，rescue中的任务不执行
  - block中的任务出现失败（failed），rescue中的任务执行
  - block中的任务不管怎么样，always中的任务总是执行

```shell
[root@control ansible]# vim block2.yml
---
- name: block test
  hosts: test
  tasks:
    - name: block / rescue / always test1
      block:
        - name: touch a file
          file:
            path: /tmp/test1.txt
            state: touch
      rescue:
        - name: touch file test2.txt
          file:
            path: /tmp/test2.txt
            state: touch
      always:
        - name: touch file test3.txt
          file:
            path: /tmp/test3.txt
            state: touch

# 执行playbook node1上将会出现/tmp/test1.txt和/tmp/test3.txt
[root@control ansible]# ansible-playbook block2.yml
[root@node1 ~]# ls /tmp/test*.txt
/tmp/test1.txt  /tmp/test3.txt

# 修改上面的playbook，使block中的任务出错
[root@node1 ~]# rm -f /tmp/test*.txt
[root@control ansible]# vim block2.yml
---
- name: block test
  hosts: test
  tasks:
    - name: block / rescue / always test1
      block:
        - name: touch a file
          file:
            path: /tmp/abcd/test11.txt
            state: touch
      rescue:
        - name: touch file test22.txt
          file:
            path: /tmp/test22.txt
            state: touch
      always:
        - name: touch file test33.txt
          file:
            path: /tmp/test33.txt
            state: touch
# 因为node1上没有/tmp/abcd目录，所以block中的任务失败。但是playbook不再崩溃，而是执行rescue中的任务。always中的任务总是执行
[root@control ansible]# ansible-playbook block2.yml
[root@node1 ~]# ls /tmp/test*.txt
/tmp/test22.txt  /tmp/test33.txt
```

## loop循环

- 相当于shell中for循环
- ansible中循环用到的变量名是固定的，叫item

```shell
# 在test组中的主机上创建5个目录/tmp/{aaa,bbb,ccc,ddd,eee}
[root@control ansible]# vim loop1.yml
---
- name: use loop
  hosts: test
  tasks:
    - name: create directory
      file:
        path: /tmp/{{item}}
        state: directory
      loop: [aaa,bbb,ccc,ddd,eee]

# 上面写法，也可以改为：
---
- name: use loop
  hosts: test
  tasks:
    - name: create directory
      file:
        path: /tmp/{{item}}
        state: directory
      loop: 
        - aaa
        - bbb
        - ccc
        - ddd
        - eee

[root@control ansible]# ansible-playbook loop1.yml


# 使用复杂变量。创建zhangsan用户，密码是123；创建lisi用户，密码是456
# item是固定的，用于表示循环中的变量
# 循环时，loop中每个-后面的内容作为一个整体赋值给item。
# loop中{}中的内容是自己定义的，写法为key:val
# 取值时使用句点表示。如下例中取出用户名就是{{item.uname}}
[root@control ansible]# vim loop_user.yml
---
- name: create users
  hosts: test
  tasks:
    - name: create multiple users
      user:
        name: "{{item.uname}}"
        password: "{{item.upass|password_hash('sha512')}}"
      loop:
        - {"uname": "zhangsan", "upass": "123"}
        - {"uname": "lisi", "upass": "456"}
[root@control ansible]# ansible-playbook  loop_user.yml
```

## role角色

- 为了实现playbook重用，可以使用role角色
- 角色role相当于把任务打散，放到不同的目录中
- 再把一些固定的值，如用户名、软件包、服务等，用变量来表示
- role角色定义好之后，可以在其他playbook中直接调用

```shell
# 使用常规playbook，修改/etc/motd的内容
# 1. 修改默认配置
[root@control ansible]# vim ansible.cfg 
[defaults]
inventory = hosts

# 2. 创建motd模板文件
[root@control ansible]# vim motd.j2
Hostname: {{ansible_hostname}}     # facts变量，主机名
Date: {{ansible_date_time.date}}   #  facts变量，日期
Contact to: {{admin}}              # 自定义变量

# 3. 编写playbook
[root@control ansible]# vim motd.yml
---
- name: modifty /etc/motd
  hosts: test
  vars:
    admin: root@tedu.cn            # 自定义名为admin的变量
  tasks:
    - name: modify motd
      template:
        src: motd.j2
        dest: /etc/motd

[root@control ansible]# ansible-playbook motd.yml
[root@node1 ~]# cat /etc/motd 
Hostname: node1
Date: 2021-11-01
Contact to: root@tedu.cn


# 创建角色
# 1. 声明角色存放的位置
[root@control ansible]# vim ansible.cfg 
[defaults]
inventory = hosts
roles_path = roles    # 定义角色存在当前目录的roles子目录中

# 2. 创建角色目录
[root@control ansible]# mkdir roles

# 3. 创建名为motd的角色
[root@control ansible]# ansible-galaxy init roles/motd
[root@control ansible]# ls roles/
motd     # 生成了motd角色目录
[root@control ansible]# yum install -y tree
[root@control ansible]# tree roles/motd/
roles/motd/
├── defaults         # 定义变量的目录，优先级最低
│    └── main.yml
├── files            # 保存上传的文件（如copy模块用到的文件）
├── handlers         # handlers任务写到这个目录的main.yml中
│    └── main.yml
├── meta             # 保存说明数据，如角色作者、版本等
│    └── main.yml
├── README.md        # 保存角色如何使用之类的说明
├── tasks            # 保存任务
│    └── main.yml
├── templates        # 保存template模块上传的模板文件
├── tests            # 保存测试用的playbook。可选
│    ├── inventory
│    └── test.yml
└── vars             # 定义变量的位置，推荐使用的位置
     └── main.yml

# 4. 将不同的内容分别写到对应目录的main.yml中
# 4.1 创建motd.j2模板文件
[root@control ansible]# vim roles/motd/templates/motd.j2
Hostname: {{ansible_hostname}}
Date: {{ansible_date_time.date}}
Contact to: {{admin}}

# 4.2 创建变量
[root@control ansible]# vim roles/motd/vars/main.yml  # 追加一行
admin: zzg@tedu.cn

# 4.3 创建任务
[root@control ansible]# vim roles/motd/tasks/main.yml  # 追加
- name: modify motd
  template:
    src: motd.j2      # 这里的文件，自动到templates目录下查找
    dest: /etc/motd

# 5. 创建playbook，调用motd角色
[root@control ansible]# vim role_motd.yml
---
- name: modify motd with role
  hosts: test
  roles:
    - motd

# 6. 执行playbook
[root@control ansible]# ansible-playbook role_motd.yml 
```

- ansible的公共角色仓库：https://galaxy.ansible.com/

```shell
# 在公共仓库中搜索与httpd相关的角色
[root@zzgrhel8 ~]# ansible-galaxy search httpd
# 如果找到相应的角色，如名字为myhttpd，可以下载它到roles目录
[root@zzgrhel8 ~]# ansible-galaxy install myhttpd -p roles/
```

## ansible加解密文件

- ansible加解密文件使用ansible-vault命令

```shell
[root@control ansible]# echo "Hi ni hao" > hello.txt 
[root@control ansible]# cat hello.txt
Hi ni hao

# 加密文件
[root@control ansible]# ansible-vault encrypt hello.txt
New Vault password: 123456
Confirm New Vault password: 123456
Encryption successful
[root@control ansible]# cat hello.txt
$ANSIBLE_VAULT;1.1;AES256
37373366353566346235613731396566646533393361386131313632306563633336333963373465
6164323461356130303863633964393339363738653036310a666564313832316263393061616330
32373133323162353864316435366439386266616661373936363563373634356365326637336165
6336636230366564650a383239636230623633356565623461326431393634656666306330663533
6235

# 解密
[root@control ansible]# ansible-vault decrypt hello.txt
Vault password: 123456
Decryption successful
[root@control ansible]# cat hello.txt 
Hi ni hao


# 加密后更改密码
[root@control ansible]# ansible-vault encrypt hello.txt 
New Vault password: 123456
Confirm New Vault password: 123456
Encryption successful

[root@control ansible]# ansible-vault rekey hello.txt   # 改密码
Vault password: 123456    # 旧密码
New Vault password: abcd  # 新密码
Confirm New Vault password: abcd
Rekey successful

# 不解密文件，查看内容
[root@control ansible]# ansible-vault view hello.txt 
Vault password: abcd
Hi ni hao


# 使用密码文件进行加解密
# 1. 将密码写入文件
[root@control ansible]# echo 'tedu.cn' > pass.txt
# 2. 创建明文文件
[root@control ansible]# echo 'hello world' > data.txt
# 3. 使用pass.txt中的内容作为密码加密文件
[root@control ansible]# ansible-vault encrypt --vault-id=pass.txt data.txt
Encryption successful
[root@control ansible]# cat data.txt    # 文件已加密
# 4. 使用pass.txt中的内容作为密码解密文件
[root@control ansible]# ansible-vault decrypt --vault-id=pass.txt data.txt
Decryption successful
[root@control ansible]# cat data.txt 
hello world
```

## 特殊的主机清单变量

- 如果远程主机没有使用免密登陆，如果远程主机ssh不是标准的22端口，可以设置特殊的主机清单变量
- `ansible_ssh_user`：指定登陆远程主机的用户名
- `ansible_ssh_pass`：指定登陆远程主机的密码
- `ansible_ssh_port`：指定登陆远程主机的端口号

```shell
# 删除远程主机的/root/.ssh/authorized_keys，以便恢复通过密码登陆
[root@control ansible]# ansible all -m file -a "path=/root/.ssh/authorized_keys state=absent"

# 创建新的工作目录
[root@control ~]# mkdir myansible
[root@control ~]# cd myansible
[root@control myansible]# vim ansible.cfg
[defaults]
inventory = hosts
[root@control myansible]# vim hosts
[group1]
node1
node2
node3
[root@control myansible]# ansible all -m ping  # 报错，因为无法免密执行

# 修改node1 ssh服务的端口为220
[root@node1 ~]# systemctl stop firewalld
[root@node1 ~]# vim +17 /etc/ssh/sshd_config 
Port 220
[root@node1 ~]# systemctl restart sshd
# 退出再登陆时，需要指定端口号
[root@zzgrhel8 ~]# ssh -p220 192.168.88.11 



# 配置ssh通过用户名、密码管理远程主机，通过220端口连接node1
[root@control myansible]# vim hosts 
[group1]
node1 ansible_ssh_user=root ansible_ssh_pass=a ansible_ssh_port=220
node2 ansible_ssh_user=root ansible_ssh_pass=a
node3 ansible_ssh_user=root ansible_ssh_pass=a

[root@control myansible]# ansible all -m ping
```

## <u>附录： Ansible的命令参数</u>

- -m：要执行的模块，默认为command
- -a：模块的参数
- -u：ssh连接的用户名，默认用root，ansible.cfg中可以配置
- -b(-become): 变成某个用户来执行操作，默认变的方式是sudo，默认变成root
- --become-user： 指定变成某个用户
- -k(--ask-pass)：提示输入ssh登录密码，当使用密码验证的时候用
- -s(--sudo)：sudo运行
- -U(--sudo-user)：sudo到哪个用户，默认为root
- -K：提示输入sudo密码，当不是NOPASSWD模式时使用
- -C(--check)：只是测试一下会改变什么内容，不会真正去执行
- -c：连接类型(default=smart)
- -f(--forks)：fork多少进程并发处理，默认为5个
- -i：指定hosts文件路径，默认default=/etc/ansible/hosts
- -I：指定pattern，对已匹配的主机中再过滤一次
- --list-host：只打印有哪些主机会执行这个命令，不会实际执行
- -M：要执行的模块路径，默认为/usr/share/ansible
- -o：压缩输出，摘要输出
- --private-key：私钥路径
- -T：ssh连接超时时间，默认是10秒
- -t：日志输出到该目录，日志文件名以主机命名
- -v：显示详细日志