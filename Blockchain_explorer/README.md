<center>长期的一致性胜过短期的高强度</center>

------

### 搭建区块链浏览器——萌新友好

[项目链接](https://gitee.com/acrowise/blockchain-explorer)

#### 0. 前言

你是否因为只能通过命令终端观察fabric的网络结构而倍感苦恼？你是否因为不能便捷的查看区块链状态而感到迷茫。。。。。。。。。来学区块链浏览器吧！！！！来一起享受螺旋升天的畅快✈

但是当你打开上面项目链接，你可能会大喊：淦，大霖！大霖！为何有一种什么都看不懂的赶脚🤣！！！

一般遇到这种情况，正确的做法是抽根儿烟冷静一下🚬，然后放弃考研（张宇乱入），这像极了追姑娘的你🐶。90后的叔叔阿姨怎么能轻言放弃，虽然我跟大家一样，也很懵逼，但是奥利给精神激励法让我充满了动力，区块链浏览器从0到1，让我给大家踩踩坑吧！！！

来点儿正经的，我会把关于项目查到的一些链接放在合适的位置供大家查看，我也是小白，所以做的过程中的遇到的问题和解决的方法都会记录下来，当然，可大家遇到的问题会不同，不过也希望能对大家有所帮助。

来吧，和你的小可爱大霖一起，相位猛冲！！！

#### 1. 区块链浏览器是什么

当谈到区块链浏览器的时候，你可能会大吃一惊——难道是谷歌、百度这样的浏览器和区块链技术的结合体吗？其实并不是，我们需要注意断句，不是区块链浏览器，而是区块链浏览、器，它真的就只是浏览，而并没有传统浏览器解析前端代码的作用。

所以，区块链浏览器就是我们平时在浏览器中登录的网站，它建立在普通的中心化网络上（这个不难理解，意思就是网站的拥有者权威发布区块链信息），专门为用户提供**浏览和查询区块链上信息。**

因为区块链公开透明的特质，它需要有一个媒介能够让矿工、监管者、开发者、交易者等等用户看到链上的情况，比如某笔交易、某块区块、当前链高。区块链浏览器就是这个媒介，在这里，我们可以看到链上的所有信息（除了隐私信息，比如真实身份），只需要输入某钱包地址或者某笔交易的ID，即可查询它们的详细信息。

[上述解释的参考信息](https://baijiahao.baidu.com/s?id=1656940963144135646&wfr=spider&for=pc)

如果你对以太坊有所了解的话，应该知道以太坊浏览器[etherscan](https://etherscan.io/)，在这个网站上，网站内容实时更新，你可以搜索查看以太坊区块链的全部信息。

现在我们对区块链浏览器已经有了一个初步的认识，那现在我们就一起去搭建这样的一个浏览器，从而帮我们加深认识。

#### 2. 前期准备

##### 2.1 新建虚拟机

为了避免之前做的项目搭建的环境的影响，我们最好新建一个虚拟机，这样会减少本项目搭建过程中遇到的问题（老linux操作系统玩家请回避），虚拟机的创建过程不在赘述，并且你需要在虚拟机中安装Ubuntu系统，另外，操作虚拟机的远程工具当然必不可少（比如XShell）。

##### 2.2 环境搭建（一）

开启虚拟机之后，我们为项目运行搭建所需环境

```shell
$ sudo apt install vim
$ sudo apt install git 
$ sudo apt install curl

# 安装 docker 和 docker-compose
$ sudo apt install docker.io
$ sudo apt install docker-compose
# docker 换源
$ 

# 安装 nvm
$ mkdir .nvm 
$ git clone https://github.com/creationix/nvm.git ~/.nvm
$ export NVM_DIR="$HOME/.nvm"
$[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# 安装 golang
$ wget https://dl.google.com/go/go1.14.4.linux-amd64.tar.gz
$ sudo tar -zxvf go1.14.4.linux-amd64.tar.gz -C /usr/local/
$ sudo vim /etc/profile
# 添加以下内容
export GOPATH=$HOME/go
export GOROOT=/usr/local/go
export PATH=$GOROOT/bin:$PATH
$ source /etc/profile
$ go version
```

这些内容我们在以往的项目中也经常会进行安装，我们也相对熟悉。

##### 2.3 环境搭建（二）

看教程，本项目对一些运行环境提出了一些之前没有见过的要求，还需要再搭建一下

| Hyperledger Explorer Version                                 | Fabric Version Supported                                     | NodeJS Version Supported                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------- |
| **[v1.1.1](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.1.1.md)** (Jul 17, 2020) | [v1.4.0 to v2.1.1](https://hyperledger-fabric.readthedocs.io/en/release-2.1) | [12.16.x](https://nodejs.org/en/download/releases) |
| **[v1.1.0](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.1.0.md)** (Jul 01, 2020) | [v1.4.0 to v2.1.1](https://hyperledger-fabric.readthedocs.io/en/release-2.1) | [12.16.x](https://nodejs.org/en/download/releases) |
| **[v1.0.0](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.0.0.md)** (Apr 09, 2020) | [v1.4.0 to v1.4.8](https://hyperledger-fabric.readthedocs.io/en/release-1.4) | [10.19.x](https://nodejs.org/en/download/releases) |
| **[v1.0.0-rc3](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.0.0-rc3.md)** (Apr 01, 2020) | [v1.4.0 to v1.4.6](https://hyperledger-fabric.readthedocs.io/en/release-1.4) | [10.19.x](https://nodejs.org/en/download/releases) |
| **[v1.0.0-rc2](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.0.0-rc2.md)** (Dec 10, 2019) | [v1.4.0 to v1.4.4](https://hyperledger-fabric.readthedocs.io/en/release-1.4) | [8.11.x](https://nodejs.org/en/download/releases)  |
| **[v1.0.0-rc1](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v1.0.0-rc1.md)** (Nov 18, 2019) | [v1.4.2](https://hyperledger-fabric.readthedocs.io/en/release-1.4) | [8.11.x](https://nodejs.org/en/download/releases)  |
| **[v0.3.9.5](https://gitee.com/acrowise/blockchain-explorer/blob/master/release_notes/v0.3.9.5.md)** (Sep 8, 2019) | [v1.4.2](https://hyperledger-fabric.readthedocs.io/en/release-1.4) | [8.11.x](https://nodejs.org/en/download/releases)  |

- Nodejs 10 and 12 (10.19 and 12.16 tested)
- PostgreSQL 9.5 or greater
- [jq](https://stedolan.github.io/jq)
- Linux-based operating system, such as Ubuntu or MacOS
- golang (optional)
  - For e2e testing

先不管 fabric 的要求，稍后再关注，我本来打算搞最新本版本的浏览器，最后没搞成，最后是用了 nodejs 8.11.4 ，对应于浏览器的 v1.0.0-rc2 ，fabric 下载 v1.4.0 版本

```shell
# 查看远程库中 nodejs 的版本
$ nvm ls-remote
# 换个下载源提升速度
$ export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
# 下载 nodejs
$ nvm install v8.11.4
$ node -v && npm -v
# 把 npm 升级一下
$ npm -g install npm@6.9.0
# 两个插件，我觉的可能装不装没有影响
$ sudo apt install nodejs-legacy
$ sudo apt install libssl1.0-dev nodejs-dev node-gyp npm
```

==安装启动数据库 PostgreSQL，修改数据库配置文件==

+ 安装启动

PostgreSQL 是一种特性非常齐全的自由软件的对象-关系型数据库管理系统

```shell
$ sudo apt-get install postgresql
$ psql --version
psql (PostgreSQL) 10.12 (Ubuntu 10.12-0ubuntu0.18.04.1)
# 启动数据库
$ sudo /etc/init.d/postgresql start
# 查看是否启动
$ netstat -tlnp
# 如果启动成功的话，会出现 5432 的端口号
```

按照要求的话，项目支持 PostgreSQL 9.5 及更高版本，所以直接安装就可以了，下载之后查看一下，可以看到版本为 10.12。

我在启动项目的时候还遇到了无法连接数据库的问题，日志文件 `blockchain-explorer/logs/console/console.log` 文件显示错误 <font color="red">` password authentication failed for user：postgres ` </font>， 这个时候需要修改数据库的配置文件。

+ 修改数据库配置文件（可以防止我在上面描述的遇到的问题）

```shell
$ cd /etc/postgresql/10/main
$ sudo vim postgresql.conf # 修改监听地址为 listen_address = '*'

$ sudo vim pg_hba.conf # 基本拉到文件末尾，将所有的第四个字段改为trust

$ sudo /etc/init.d/postgresql reload # 重新加载配置文件
$ sudo /etc/init.d/postgresql restart # 重新启动数据库服务器
```

==安装jq==

jq 可以对 json 数据进行分片、过滤、映射和转换，和 sed、awk、grep 等命令一样，都可以让你轻松地把玩文本。它能轻松地把你拥有的数据转换成你期望的格式，而且需要写的程序通常也比你期望的更加简短。

```shell
$ sudo apt install jq
```

##### 2.4 下载项目文件

我查了一些其他的教程，发现项目代码的存放位置没有进行具体的说明，所以我直接将项目放在 home 目录下

```shell
$ git clone https://gitee.com/acrowise/blockchain-explorer.git
$ sudo chown -R $USER:$USER ./blockchain-explorer # 变更一下文件的归属
# 切换浏览器版本
$ cd ~/blockchain-explorer
$ git tag 
$ git checkout v1.0.0-rc2
```

##### 2.5 fabric 下载安装

现在我们还面临一个问题，就是 fabric 要下载在哪个目录下面，初步确定下载什么位置都可以，只不过后面要改浏览器项目链接 fabric 网络的路径。

```shell
$ mkdir hyfa && cd hyfa
$ $ vim bootstrap.sh #创建并进入文件编辑界面
### 通过以下网址https://github.com/hyperledger/fabric/blob/master/scripts/bootstrap.sh 进入GitHub仓库，其中的有bootstrap.sh的代码文件，将其复制下来，返回到虚拟机编辑，点击insert编辑文件，shift+insert粘贴复制的代码到bootstrap.sh文件中，点击ECS,:wq后点击ENTER,保存退出
$ chmod +x bootstrap.sh # 赋予此文件可执行权限
$ sudo ./bootstrap.sh 1.4.0 # 需要哪个版本就安装哪个版本
```

#### 3. 项目搭建

##### 3.1 创建数据库（有关数据库的知识，会再补充）

+ 创建数据库

```shell
$ cd ~/blockchain-explorer/app/persistence/fabric/postgreSQL/db
$ sudo -u postgres ./createdb.sh # 创建数据库
```

使用下面两个命令检查是否创建成功

```shell
$ sudo -u postgres psql -c '\l'
$ sudo -u postgres psql fabricexplorer -c '\d'
```

如果你得到了和教程中的相同的表格，说明是创建成功了到的问题

+ ==执行数据库创建文件不成功==

报错是 `node: command not found` , 如果你也遇到这个问题，首先检查 `nvm` 是否安装了，因为上面所说的安装命令并不是永久的，只要虚拟机时重新开机或者远程控制工具短线重连，你都要重新执行下方的命令

```··shell
$ export NVM_DIR="$HOME/.nvm"
$ [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

如果确定 `node` 可以使用，可以尝试蛇皮走位，曲线救国 

```shell
$ sudo vim createdb.sh
# 将命令的第一行拷贝并删除
node processenv.js
# 在原本的文件夹单独执行
$ node processenv.js
$ sudo -u postgres ./createdb.sh
$ sudo -u postgres psql -c '\l'
$ sudo -u postgres psql fabricexplorer -c '\d'
```

通过上面的操作，我得到了与教程中相同的两个表格。

![](photo\0001.png)

+ ==执行数据库创建文件不成功==

报错 <font color="red">`psql: could not connect to server: No such file or directory`</font>

```shell
# 查看是否有数据库服务器及端口5034
$ netstat -tlnp
# 如果没有的话，说明数据库没有启动，可以启动一下
$ sudo /etc/init.d/postgresql start
```

##### 3.2 修改配置文件

根据教程，需要修改配置文件，以使得浏览器可以连接上 fabric 网络，到现在为止，需要修改的文件为 `blockchain-explorer/app/platform/fabric/connection-profile/first-network.json` ，改动为三个 fabric 证书文件的路径

```shell
$ cd hyfa/fabric-samples/first-network
$ sudo ./byfn.sh -m generate
$ sudo ./byfn.sh -m up
```

开启 fabric 的测试网络，生成 `crypto-config` 证书文件，并将对应的文件路径写入 `first-network.json` 文件，文件修改如下![](photo\0010.png)

可以看到，有三处路径被修改，并且使用的是绝对路径。

==注意==：不同的浏览器版本需要修改的路径数量有所不同，比如主分支最新版，需要改动四个地方

##### 3.3 安装依赖包

为了项目的运行，需要拉取一些依赖包，为此，我们可以执行 `blockchain-explorer` 目录下的 `main.sh` 文件，并且安装依赖需要提前安装两个包，一个时是 `make` ，另一个时 `g++` ，可以下载两个包之后执行命令

```shell
$ sudo apt install make 
$ suao apt install g++ 
$ cd blockchain-exporer
$ ./main.sh install
```

我在安装了这两个包之后是执行成功了的。

##### 3.4 运行程序

做完上述所有的工作之后，可以回到项目根目录，启动服务

```shell
$ cd ~/blockchain-explorer
$ sudo ./start.sh
```

加管理员权限执行，是为了防止文件权限导致的启动错误，如果正常启动的话，终端显示如下图所示

![](photo\0011.png)

==注意==

如果没有启动成功，可以在 `blockchain-explorer/logs/console` 目录下查看日志文件，找出出错的原因，当然，很多时候看了也不知道问题到底出在哪里，只会让人更加懵逼。

#### 4. 前端效果

##### 4.1 修改IIS

在登录浏览器之前，记得在 iis 服务器管理上修改一下虚拟机的 IP 和访问端口，端口为 8080

##### 4.2 访问

在浏览器访问 `http://localhost:8080` ，可以看到下面的页面

<img src="photo\0100.png" style="zoom:67%;" />

账号密码都已经在 `first-network` 里面设置过，账号是 `admin`，密码是 `adminpw` 。

登录之后，可以看到下面的页面

![](photo\0101.png)

##### 4.3 结束

```shell
$ sudo ./stop.sh
$ sudo ./byfn.sh -m down
```

记得下次开启的时候，把路径中的私钥换一下，不然就出错咯