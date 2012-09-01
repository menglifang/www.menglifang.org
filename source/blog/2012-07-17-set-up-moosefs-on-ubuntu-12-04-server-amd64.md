---
title: 在Ubuntu Server 12.04 LTS AMD64下安装Moosefs
date: 2012/07/17
tags: [ubuntu, linux, dfs]
---

###mfs主控节点(master server)

####安装说明

* 安装Ubuntu Server 12.04 LTS AMD64
* 下载[mfs-1.6.25.tar.gz](http://www.moosefs.org/download.html)
* 安装编译工具和依赖库

READMORE

```bash
apt-get install build-essential

apt-get install zlib1g-dev
```

* 添加系统用户

```bash
groupadd mfs
useradd -g mfs mfs
```

* 编译并安装

```bash
cd /usr/src
tar zxvf mfs-1.6.25.tar.gz
cd mfs-1.6.25

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib
--with-default-user=mfs --with-default-group=mfs
--disable-mfschunkserver --disable-mfsmount
make && make install
```

* 配置服务器

  * 通过配置模板创建缺省配置文件

    ```bash
    cd /etc 
    cp mfsmaster.cfg.dist mfsmaster.cfg
    cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
    cp mfsexports.cfg.dist mfsexports.cfg
    ```

  * 创建metadata文件

    ```bash
    cd /var/lib/mfs
    cp metadata.mfs.empty metadata.mfs
    ```

  * 创建服务器名mfsmaster到IP地址的映射

    ```
    vi /etc/hosts

    # 例如：
    192.168.1.81  mfsmaster
    ```

* 启动mfsmaster服务、

```bash
/usr/sbin/mfsmaster start
```

* 启动Web监控服务

```bash
/usr/sbin/mfscgiserv
```

* 通过浏览器访问监控服务，[http://mfsmaster:9425](http://mfsmaster:9425)

###mfs备份节点(metalogger)

####安装说明

* 安装Ubuntu Server 12.04 LTS AMD64
* 下载[mfs-1.6.25.tar.gz](http://www.moosefs.org/download.html)
* 安装编译工具和依赖库

```bash
apt-get install build-essential

apt-get install zlib1g-dev
```

* 添加系统用户

```bash
groupadd mfs
useradd -g mfs mfs
```

* 编译并安装

```bash
cd /usr/src
tar zxvf mfs-1.6.25.tar.gz
cd mfs-1.6.25

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib
--with-default-user=mfs --with-default-group=mfs
--disable-mfschunkserver --disable-mfsmount
make && make install
```

* 配置服务器

  * 通过模板创建缺省的metalogger配置文件

    ```bash
    cp /etc
    cp mfsmetalogger.cfg.dist mfsmetalogger.cfg
    ```

  * 配置mfsmaster的IP地址

    ```bash
    vi /etc/hosts

    # 例如
    192.168.1.81  mfsmaster
    ```

* 启动mfsmetalogger服务

```bash
/usr/sbin/mfsmetalogger start
```

###mfs数据节点(chunk server)

####安装说明

* 安装Ubuntu Server 12.04 LTS AMD64
* 下载[mfs-1.6.25.tar.gz](http://www.moosefs.org/download.html)
* 安装编译工具和依赖库

```bash
apt-get install build-essential

apt-get install zlib1g-dev
```

* 添加系统用户

```bash
groupadd mfs
useradd -g mfs mfs
```

* 编译并安装

```bash
cd /usr/src
tar zxvf mfs-1.6.25.tar.gz
cd mfs-1.6.25

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib
--with-default-user=mfs --with-default-group=mfs --disable-mfsmaster
make && make install
```

* 配置服务器

  * 通过模板创建缺省的配置文件

    ```bash
    cp /etc
    cp mfschunkserver.cfg.dist mfschunkserver.cfg
    cp mfshdd.cfg.dist mfshdd.cfg
    ```

  * 配置挂载的硬盘

    ```bash
    vi /etc/mfshdd.conf

    # 例如
    /mnt/mfschunks1
    /mnt/mfschunks2
    ```

  * 配置挂载目录使用户mfs有写权限

    ```bash
    chown -R mfs:mfs /mnt/mfschunks1
    chown -R mfs:mfs /mnt/mfschunks2
    ```

  * 配置mfsmaster的IP地址

    ```bash
    vi /etc/hosts

    # 例如
    192.168.1.81  mfsmaster
    ```

* 启动mfschunkserver服务

```bash
/usr/sbin/mfschunkserver start
```

###客户端安装

####安装说明

* 安装Ubuntu Server 12.04 LTS AMD64
* 下载[fuse](http://sourceforge.net/projects/fuse/files/latest/download)
* 下载[mfs-1.6.25.tar.gz](http://www.moosefs.org/download.html)
* 安装编译工具和依赖库

```bash
apt-get install build-essential

apt-get install zlib1g-dev
```

* 编译并安装fuse

```bash
cd /usr/src
tar zxvf fuse-2.9.0.tar.gz
./configure
make && make install
```

* 编译并安装mfs

```bash
cd /usr/src
tar zxvf mfs-1.6.25.tar.gz
cd mfs-1.6.25

./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var/lib
--with-default-user=mfs --with-default-group=mfs --disable-mfsmaster
--disable-mfschunkserver
make && make install
```

* 配置mfsmaster的IP地址

```bash
vi /etc/hosts

# 例如
192.168.1.81  mfsmaster
```

* 挂载MFS文件系统

```bash
mkdir -p /mnt/mfs
/usr/bin/mfsmount /mnt/mfs -H mfsmaster
```

* 查看是否正确挂载

```bash
df -h | grep mfs
```

