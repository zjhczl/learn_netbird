# learn netbird
要求
基础设施要求：

具有至少1CPU和2GB内存的 Linux VM。
该虚拟机应该可通过 TCP 端口80、443、33073、10000和33080公开访问；还可通过 UDP 端口3478、49152-65535公开访问。
指向虚拟机的公有域名。
软件要求：

使用 docker compose 插件（ Docker 安装指南）在 VM 上安装 Docker ，或者使用 docker 2 或更高版本的 docker-compose。
jq已安装。在大多数发行版中，通常在官方存储库中可用，可以使用sudo apt install jq或安装sudo yum install jq
curlsudo apt install curl已安装。通常在官方存储库中可用，可以使用或安装sudo yum install curl

## 1. 安装

### 1.1. docker环境
#### 1.1.1. 移除原来的docker环境
```
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
#### 1.1.2. 添加docker源
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
#### 1.1.3. 安装docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### 1.2. 安装jq
```
sudo apt install jq
```
### 1.3. 安装netbird
```
export NETBIRD_DOMAIN=netbird.example.com; curl -fsSL https://github.com/netbirdio/netbird/releases/latest/download/getting-started-with-zitadel.sh | bash

```
也可以分布安装
```
curl -sSLO https://github.com/netbirdio/netbird/releases/latest/download/getting-started-with-zitadel.sh
# check the script
cat getting-started-with-zitadel.sh
# run the script
export NETBIRD_DOMAIN=netbird.example.com
bash getting-started-with-zitadel.sh

```

## 问题
### docker镜像拉取出错
创建或修改 /etc/docker/daemon.json
```
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io/",
        "https://huecker.io/",
        "https://dockerhub.timeweb.cloud",
        "https://noohub.ru/",
        "https://dockerproxy.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://docker.nju.edu.cn",
        "https://xx4bwyg2.mirror.aliyuncs.com",
        "http://f1361db2.m.daocloud.io",
        "https://registry.docker-cn.com",
        "http://hub-mirror.c.163.com"
    ]
}
```
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```