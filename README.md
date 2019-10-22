# phddns for docker

## 项目来源
在archlinuxarm上不方便使用花生壳内网版，所以制作了一个基于debian的docker镜像来运行。事实上基于docker的话其他发行版本基本可以使用了。

## 使用方法

### 安装docker

依据你使用的发行版来安装docker

#### Archlinuxarm

```
# pacman -S docker      
```
#### Debian

- 信任 Docker 的 GPG 公钥

```
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

- 如果是32位系统

```
# echo "deb [arch=armhf] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/debian \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
```

- 如果是64位系统

```
# echo "deb [arch=arm64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/debian \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
```

- 最后是安装

```
# apt-get update
# apt-get install -y docker-ce
```

### 构建phddns镜像

- 如果使用64位系统

```
$ cat Dockerfile.aarch64 > Dockerfile
```

- 如果使用32位系统

```
$ cat Dockerfile.armhf > Dockerfile
```

构建镜像

```
$ docker build -t phddns:v1 .
```

构建后会导入至本地镜像之中

### 创建容器启动镜像

```
$ docker run -d \
      --restart=always \
      --mac-address=<mac-address> \
      --name=phddns \
      phddns:v1 \
      /bin/bash /usr/orayapp/phdaemon
```

上面的`<mac-address>`和花生壳的SN号相关连，如果有付费过的账号，将mac改成对应机器网口的mac地址。

## 注意事项
如果使用aarch架构的archlinuxarm或者部分cpu无法直接运行phddns，可以使用qemu模拟运行。aarch64架构的archlinuxarm可以使用该PKGBUILD构建的qemu-user-static-bin来运行。
```
https://gist.github.com/abelfodil/d0916511bfa3c75bc2e01dc79b045623
```