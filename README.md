# 安装git，这个 SSR 脚本会使用git自动将SSR下载到本地，先安装git
    sudo apt-get update 
    sudo apt-get install git 
# 安装ShadowsocksR 或者 SSR 脚本
    git clone https://github.com/IArvinServer/linux-ssr.git    
    cd linux-ssr 
# 这个脚本算是写的比较完善了，里面封装了 SSR 的安装、配置、启动、关闭等功能。为了方便操作，我们将脚本放进 /usr/local/bin 中
    sudo mv ssr /usr/local/bin 
    sudo chmod 766 /usr/local/bin/ssr 
# 接下来我们通过脚本安装SSR
    ssr install 
    ssr config 
    配置文件路径 /usr/local/share/shadowsocksr/config.json
# 配置ShadowsocksR ，第一次安装完成之后回自动打开配置文件，如下所示：
    {
    "server": "服务器地址",
    "server_ipv6": "::",
    "server_port": 端口,
    "local_address": "127.0.0.1",
    "local_port": 8118,

    "password": "密码",
    "method": "aes-256-cfb",
    "protocol": "origin",
    "protocol_param": "",
    "obfs": "plain",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, 
    "additional_ports_only" : false, 
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",
    "fast_open": false
}
# 注意替换配置中的相关节点信息，配置好之后会自动启动SSR，如果输出的日志没有提示错误的话，那么 ShadowsocksR 的配置就完成了。
# 将SOCKS代理转换为HTTP代理，
# 安装配置privoxy

    sudo apt-get install -y privoxy        #安装privoxy
    sudo vim /etc/privoxy/config        #配置privoxy
    通过 / 搜索 listen-address 192.168.0.1:8118 第773行
    config配置，然后将前面的 # 去掉，然后修改为 listen-address  127.0.0.1:8118
    （端口号可以自行更改，后面必须一致），取消 listen-address [::1]:8118 前面的注释，不用修改。
    搜索 forward-socks5t ，第1388行，（没找到可以新建一行），然后将内容修改为如下内容：
    forward-socks5t / 127.0.0.1:8118
注意最后的那个点是必须写的。
# 重启privoxy
    sudo service privoxy start
# 剩下的就看你想怎么使用了，现在我的HTTP代理的端口是 8118 ，如果我想让 apt 走代理，那么执行下面的
# 操作配置环境变量
# 设置http 和 https 全局代理
    export http_proxy='http://localhost:8118' 
    export https_proxy='http://localhost:8118' 
# 取消代理
    unset http_proxy 
    unset https_proxy 
# 测试代理是否可用：
    curl www.google.com 
 如果有输出证明代理成功。
# 如果是像我一样拉LineageOS的源码编译，那么就给 git 配置代理，不用 HTTP 代理，直接使用 SOCKS 代理：
    git config --global http.proxy 'socks5://127.0.0.1:8118' 
    git config --global https.proxy 'socks5://127.0.0.1:8118' 
# 取消代理
    git config --global --unset http.proxy 
    git config --global --unset https.proxy 
至此，本文完~
