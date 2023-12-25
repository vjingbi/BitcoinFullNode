# BitcoinFullNode

打铭文专用比特币全节点搭建教程及工具

---

**一. 比特币全节点**

---

**二. Ordinals全节点**

---

**三. Atomicals全节点**
1. Atomicals-electrumx (Electrum X Server)
    Github : https://github.com/atomicals/atomicals-electrumx

    a. 配置环境
   `sudo apt install python3-pip build-essential libc6-dev libncurses5-dev libncursesw5-dev libreadline-dev libleveldb-dev`
   
    b. 安装plyvel (需要提前安装好python3，我是用的版本是3.10) `sudo pip3 install plyvel` 

    c. 克隆Atomicals-ElectrumX  `git clone https://github.com/atomicals/atomicals-electrumx.git`

    d. 克隆完成后进入ElectrumX文件夹，执行安装命令 `sudo python3 http://setup.py install`

   
3. Electrumx-proxy (ElectrumX-Proxy)
    Github : https://github.com/atomicals/electrumx-proxy
