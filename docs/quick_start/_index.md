+++
title = "Quick Start"
weight = 20
pre = "<b>2. </b>"
+++

# Quick Start

The latest release version is 0.2.0, you can quickly experience Dragonfly in the following simple steps.
<!--more-->

## Start SuperNode on Docker Container

We provide 2 images in different places to speed up your pulling, you can choose one of them according to your location.

* China: registry.cn-hangzhou.aliyuncs.com/alidragonfly/supernode:0.2.0
* USA: registry.us-west-1.aliyuncs.com/alidragonfly/supernode:0.2.0

Here is the commands if you choose the registry `registry.cn-hangzhou.aliyuncs.com/alidragonfly/supernode:0.2.0`:

```bash
imageName="registry.cn-hangzhou.aliyuncs.com/alidragonfly/supernode:0.2.0"
docker pull ${imageName}
docker run -d -p 8001:8001 -p 8002:8002 ${imageName}
```

## Install Dragonfly Client

Download the proper package for your operating system and architecture:

* df-client: linux 64-bit
  * [Download from GitHub](https://github.com/alibaba/Dragonfly/releases/download/v0.2.0/df-client_0.2.0_linux_amd64.tar.gz)
  * [Download from OSS](http://dragonfly-os.oss-cn-beijing.aliyuncs.com/df-client_0.2.0_linux_amd64.tar.gz)
* df-client: macOS 64-bit
  * [Download from GitHub](https://github.com/alibaba/Dragonfly/releases/download/v0.2.0/df-client_0.2.0_darwin_amd64.tar.gz)
  * [Download from OSS](http://dragonfly-os.oss-cn-beijing.aliyuncs.com/df-client_0.2.0_darwin_amd64.tar.gz)

Uncompress the package and add the directory `df-client` to your `PATH` environment variable to make you can directly use `dfget` and `dfdaemon`.

Here is the commands to download and install `df-client` in `$HOME`:

```bash
cd $HOME
# select an URL listed above
wget https://github.com/alibaba/Dragonfly/releases/download/v0.2.0/df-client_0.2.0_linux_amd64.tar.gz
tar -zxf df-client_0.2.0_linux_amd64.tar.gz
# execute or add this line to ~/.bashrc
export PATH=$PATH:$HOME/df-client/
```

## Use Dragonfly to Download a File

It's very simple to use Dragonfly to download a file, just like this:

```bash
dfget -u 'https://github.com/alibaba/Dragonfly/blob/master/docs/images/logo.png' -o /tmp/logo.png
```

## Use Dragonfly to Pull an Image

We have 2 steps to do before we pull an image:

* start `dfdaemon` with a specified registry, such as `https://index.docker.io`:

    ```bash
    nohup dfdaemon --registry https://index.docker.io > /dev/null 2>&1 &
    ```

* configure dockerd and restart:
  * Add this line to dockerd config file [/etc/docker/daemon.json](https://docs.docker.com/registry/recipes/mirror/#configure-the-docker-daemon)

      ```json
      "registry-mirrors": ["http://127.0.0.1:65001"]
      ```

  * restart dockerd

      ```bash
      systemctl restart docker
      ```

> NOTE: make sure the SuperNode is running

That's all we need to do, then we can pull an image by Dragonfly just as usual:

```bash
docker pull nginx:latest
```
