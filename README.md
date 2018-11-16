# tgic-nodev2.0.1

这TGIC网络2.0是节点部署文件
推荐  2cpu 4ram 40g硬盘 1M带宽  linux ubuntu 16.04 64bit

安装：

登录root账户，部署节点无法直接在root账户上部署 所以您需要创建账户:

```
adduser <您要创建的账户名>

usermod -a -G sudo <您已经创建的账户名>
```

切换到你创建的用户，开始安装必须的东西:

```
sudo apt-get update 

sudo apt-get install -y libpq-dev build-essential python git curl jq libtool autoconf locales automake locate wget zip unzip htop nmon iftop

sudo apt-get install npm

sudo npm install -g n

sudo n 6.9.2
```

安装PostgreSQL:

```
sudo apt-get install -y postgresql postgresql-contrib

```

安装grunt-cli（全局）:

```
sudo npm install grunt-cli -g
```

克隆Tgic-node库

```
git clone https://github.com/xianfeic/tgic-nodev2.0.1
cd tgic-nodev2.0.1
```
  
然后创建数据库 
```
1 sudo -s -u postgres

2 psql

3 CREATE USER xxxx1 WITH PASSWORD 'xxxx';

4 CREATE DATABASE xxxx2;

5 GRANT ALL PRIVILEGES ON DATABASE xxxx2 to xxxx1;

# xxxx1  你要创建是数据库用户名
# xxxx    你要设置的密码
# xxxx2  你要创建的数据库

```

现在您需要修改配置文件了
找到config.Tgic.json这个文件，在第68行的代码 "secret": [""],增加您的密钥

```
  "forging": {
    "coldstart": 6,
    "force": true,
    "secret": ["请在这里输入您的12个单词密钥"],
    "access": {
      "whiteList": [
        "127.0.0.1"
      ]
    }
  },
```
完成后，再次修改config.Tgic.json！
```
 "db": {
    "host": "localhost",
    "port": 5432,
    "database": "xxxx2",
    "user": "XXXX1",
    "password": "XXXX",
    "poolSize": 20,
    "poolIdleTimeout": 30000,
    "reapIntervalMillis": 1000,
    "logEvents": [
      "error"
    ]
  },
```
OK!接下来我们就安装依赖

```
npm install libpq

npm install secp256k1

npm install bindings

npm install 
```
完成后，就可以直接运行了！

```
node app.js --genesis genesisBlock.Tgic.json --config config.Tgic.json
```
LINUX断开shh链接后节点就自动关闭了运行了
我们需要使用screen 来执行最后一步的操作
