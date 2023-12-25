# BitcoinFullNode

打铭文专用比特币全节点搭建教程及工具

---

# 一. 比特币全节点

---

# 二. Ordinals全节点

---

# 三. Atomicals全节点
## 1. Atomicals-electrumx (Electrum X Server) Github : https://github.com/atomicals/atomicals-electrumx
>Linux下运行（Ubuntu22.04）
>
>需要python环境

a. 配置环境
```
sudo apt install python3-pip build-essential libc6-dev libncurses5-dev libncursesw5-dev libreadline-dev libleveldb-dev
```
   
b. 安装plyvel (需要提前安装好python3，我用的版本是3.10)
```
sudo pip3 install plyvel
``` 

c. 克隆Atomicals-ElectrumX
```
git clone https://github.com/atomicals/atomicals-electrumx.git
```

d. 克隆完成后进入ElectrumX文件夹，执行安装命令
```
sudo python3 setup.py install
```

e. 创建文件夹
```
mkdir ~/.electrumx/db
```

f. 进入刚创建的文件夹，执行下面三条命令
```
openssl genrsa -out server.key 2048

openssl req -new -key server.key -out server.csr

openssl x509 -req -days 1825 -in server.csr -signkey server.key -out server.crt
```

g. 创建EletrumX配置文件，添加下面内容
```
nano ~/.electrumx/electrumx.conf

NET=mainnet
COIN=Bitcoin
DB_ENGINE=leveldb
DB_DIRECTORY=/home/atom/.electrumx/db
DAEMON_URL=http://rpc_username:rpc_password@IP:8332
SSL_CERTFILE=/home/atom/.electrumx/server.crt
SSL_KEYFILE=/home/atom/.electrumx/server.key
SERVICES=tcp://:50001,ssl://:50002,wss://:50004,rpc://
```

h. 防火墙放行下面端口
```
sudo ufw allow 50001

sudo ufw allow 50002
   
sudo ufw allow 50004

sudo ufw allow 50010
```

i. 设置EletrumX服务
```
sudo nano /etc/systemd/system/electrumx.service

[Unit]
Description=ElectrumXServer
After=network.target

[Service]
User=atom
EnvironmentFile=/home/atom/.electrumx/electrumx.conf
ExecStart=/home/atom/atomicals-electrumx/electrumx_server
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

j. 服务相关命令
```
sudo systemctl enable electrumx

sudo systemctl start electrumx

sudo systemctl stop electrumx

sudo systemctl status electrumx
```

k. 查看状态
```
journalctl -u electrumx -f
```

l. 成功运行截图。等同步完成就可以用自己的节点了，过程超级超级长，耐心等（看电脑配置和网速，2天左右）
![1](https://github.com/vjingbi/BitcoinFullNode/assets/41134585/56ebc20e-38e4-4e8e-8771-c49ec748c423)
![2](https://github.com/vjingbi/BitcoinFullNode/assets/41134585/10c07ec4-e842-484a-bec4-2d9c4e686d7c)



## 2. Electrumx-proxy (ElectrumX-Proxy)  Github : https://github.com/atomicals/electrumx-proxy

>Linux、Windows下均可运行
>
>需要nodejs

a. 安装nodejs，过程有点长，耐心等
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

b. 克隆Electrumx-proxy
```
git clone https://github.com/atomicals/electrumx-proxy.git
```

c. 克隆完成后进入electrumx-proxy文件夹，创建配置文件
```
cd electrumx-proxy
nano .env

NODE_ENV=production
PORT=8080
ELECTRUMX_PORT=50010
ELECTRUMX_HOST=127.0.0.1
TRUST_PROXY=true
RATE_LIMIT_WINDOW_SECONDS=90
RATE_LIMIT_DELAY_AFTER=20
RATE_LIMIT_DELAY_MS=200
```

e. 安装，过程有点长，耐心等
```
npm install
```

f. 运行
```
npm run dev
```

g. 去除速度限制
```
speedLimiter
```

h. 访问http接口，等第1步中超级超级慢的同步完成后，会显示两个true
```
http://IP:8080/proxy/health
```
![image](https://github.com/vjingbi/BitcoinFullNode/assets/41134585/02b6cfb6-a35a-4155-89f8-fbaac480f4ab)

## 3. 使用自建节点挖矿 atomicals-js https://github.com/atomicals/atomicals-js

>Linux、Windows下均可运行
>
>需要nodejs，Linux安装过程看上一步Electrumx-proxy中命令，Windows直接点下一步即可

a. 克隆atomicals-js
```
git clone https://github.com/atomicals/atomicals-js.git
```

b. 安装，过程有点长，耐心等（ 如果没有提前安装yarn，需要执行命令`npm install -g yarn` ）
```
yarn install
yarn run build
```

c. 安装完成后修改**.env**文件，ELECTRUMX_PROXY_BASE_URL改成自建节点
```
ELECTRUMX_PROXY_BASE_URL=https://IP/proxy
```

e. 创建钱包
```
yarn cli wallet-init
```

f. 开始愉快的挖矿旅程吧
```
yarn cli mint-dft quark --satsbyte=88
```
![image](https://github.com/vjingbi/BitcoinFullNode/assets/41134585/101c5a6c-7bbd-491a-af81-5776dda9ef33)
