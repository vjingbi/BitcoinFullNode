# BitcoinFullNode

打铭文专用比特币全节点搭建教程及工具

---

# 一. 比特币全节点

---

# 二. Ordinals全节点

---

# 三. Atomicals全节点
## 1. Atomicals-electrumx (Electrum X Server) Github : https://github.com/atomicals/atomicals-electrumx

    a. 配置环境
    
    ```
    sudo apt install python3-pip build-essential libc6-dev libncurses5-dev libncursesw5-dev libreadline-dev libleveldb-dev
    ```
   
    b. 安装plyvel (需要提前安装好python3，我是用的版本是3.10)
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
   
## 2. Electrumx-proxy (ElectrumX-Proxy)
    Github : https://github.com/atomicals/electrumx-proxy
