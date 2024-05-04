# 下载docker
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

# 添加当前用户到docker用户组
sudo usermod -aG docker $USER
# 更新docker用户组
newgrp docker
# 配置docker镜像加速
sudo mkdir -p /etc/docker

sudo tee /etc/docker/daemon.json <<-'EOF'

{

"registry-mirrors": ["https://punulfd2.mirror.aliyuncs.com"]

}

EOF
# 重启docker
sudo systemctl daemon-reload

sudo systemctl restart docker
# 下载二进制包

wget https://golang.google.cn/dl/go1.19.linux-amd64.tar.gz
# 将下载的二进制包解压至 /usr/local目录
sudo tar -C /usr/local -xzf go1.19.linux-amd64.tar.gz
mkdir $HOME/go
# 将以下内容添加至环境变量
vim .bashrc

export GOPATH=$HOME/go

export GOROOT=/usr/local/go

export PATH=$GOROOT/bin:$PATH

export PATH=$GOPATH/bin:$PATH
# 更新环境变量
source  ~/.bashrc
# 设置代理
go env -w GO111MODULE=on

go env -w GOPROXY=https://goproxy.cn,direct

# 下载nvm安装脚本

wget https://gitee.com/real__cool/fabric_install/raw/main/nvminstall.sh
# 安装nvm；屏幕输出内容添加环境变量
chmod +x nvminstall.sh
./nvminstall.sh


# 将环境变量写入.bashrc
vim .basgrc

export NVM_DIR="$HOME/.nvm"

[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm

[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion


# 更新环境变量
source  ~/.bashrc
# 安装node16
nvm install 16
# 换源
npm config set registry https://registry.npmmirror.com
# 安装jq
sudo apt install jq
# 克隆本项目
git clone https://github.com/TiAmo-9000/fabric-trace-two.git
# 启动区块链部分
cd fabric-trace-two/blockchain/network
# 仅在首次使用执行：下载Fabric Docker镜像。
./install-fabric.sh -f 2.5.6 d
# 启动区块链网络
./start.sh
# 如果在启动区块链网络时遇到报错可以尝试:执行清理所有的容器指令：
docker rm -f $(docker ps -aq)
# 启动后端
cd fabric-trace-two/application/backend

go run main.go

# 修改ip：两个文件中的ip地址设置为自己的服务器的公网IP地址
vim fabric-trace-two/application/web/.env.development

vim fabric-trace-two/application/web/src/router/index.js
# 启动前端
cd fabric-trace-two/application/web
# 仅在首次运行执行：安装依赖
npm install 
# 启动前端
npm run dev
# 服务器防火墙修改
允许TCP端口8080,9090,9528
# 在浏览器中打开：
服务器IP:9528 即可看到前端页面。
# 关闭项目
前端（npm run dev界面）与后端（go run main.go）界面： 使用键盘组合键：ctrl+c
# 关闭区块链部分：
在fabric-trace-two/blockchain/network目录./stop.sh
