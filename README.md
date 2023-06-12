# Docker 简明教程

![Docker Architecture diagram](https://docs.docker.com/assets/images/architecture.svg)

## 概述

Docker是一个开源的容器引擎，改变了开发、分发和运行软件的方式，为开发者和运维人员提供了一种更简洁、更高效的方法来处理应用的打包、分发和部署。

1. **简化了应用部署**：在Docker出现之前，开发人员和运维人员经常面临一个难题，即"在我的机器上可以运行，为什么在你的机器上就不行?"。Docker通过将应用及其所有依赖项打包到一个容器中，从而确保应用在任何支持Docker的平台上都可以一致地运行，解决了这个问题。
2. **推动了微服务架构的发展**：微服务架构是将一个大型应用拆分为多个独立运行、各自独立开发和部署的小服务。Docker提供了一种理想的方式来运行、管理和协调这些微服务。每个服务可以被封装在一个Docker容器中，各自独立运行和扩展。
3. **促进了DevOps文化的发展**：DevOps是一种强调开发（Dev）和运维（Ops）紧密合作的文化和实践。Docker通过提供一种应用的打包、分发和运行方式，使得开发和运维的工作更加紧密地结合在一起，推动了DevOps文化的发展。
4. **加速了云计算的发展**：云服务提供商，如Amazon AWS、Google Cloud、Microsoft Azure等，都提供了对Docker容器的原生支持，使得在云环境中运行和管理Docker容器变得非常简单。同时，新的云原生技术，如Kubernetes等，也都与Docker紧密集成，进一步推动了云计算的发展。

## Docker Hub

在正式开始使用 Docker 前，请注册一个 [Docker Hub](https://hub.docker.com/) 账号。Docker Hub 是一个用于存储和分发 Docker 镜像的公共注册服务，类似于编程语言的包管理器（如 Python 的 PyPI 或 Node.js 的 npm）中的仓库，但用于存储 Docker 镜像。它的模式类似于 GitHub，你可以下载公共镜像文件，以及上传自己的镜像文件。

## 安装

### [Docker Engine](https://docs.docker.com/engine/)

Docker 引擎是一个典型的 C/S（客户端/服务器）应用，包括三部分：

- 服务器（也叫做Docker daemon，即Docker守护进程）；
- REST API（定义程序用来与Docker daemon交互的接口）；
- 客户端（命令行接口（CLI），Docker 命令））。

**如果你在服务器上或者虚拟机里使用 Docker，请安装 Docker Engine。**

安装指导：

- [CentOS](https://docs.docker.com/engine/install/centos/)
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Debian](https://docs.docker.com/engine/install/debian/)

另外，官方提供了一个安装脚本，此脚本不适合生产环境使用，具体注意事项请参考[脚本说明](https://docs.docker.com/engine/install/debian/)。

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```



### [Docker Desktop](https://docs.docker.com/desktop/)

Docker Desktop 是一个一键安装 Docker 的应用程序,并且提供了图形化界面。

安装程序包含了：

- Docker Engine（引擎）；
- Docker CLI（客户端）；
- Docker Compose（一个用于定义和运行多容器Docker应用的工具）；
- 其他特性（如Kubernetes的集成，文件分享和网络设置等）。

**如果你在自己的电脑上学习和操作，请安装 Docker Desktop**

安装指导：

- [Mac 安装](https://docs.docker.com/desktop/install/mac-install/)
- [Windows 安装](https://docs.docker.com/desktop/install/windows-install/)
- [Linux 安装](https://docs.docker.com/desktop/install/linux-install/)

> **注意**
>
> Docker Desktop 图形化界面不支持创建容器，Windows需要借助 CMD、PowerShell（Mac需要借助终端）使用 Docker 命令交互。
>
> - Windows 推荐 Windows Terminal终端工具
> - Mac 推荐使用 item2
> - 另外，推荐使用 VSCode 编辑器，微软为其开发了很多好用的容器插件，例如：Docker、remote-containers、Kubernetes等等。

### 验证

打开终端执行`docker version` 显示版本信息，若看到版本信息即表示安装成功。另外还可以输入：

- `docker info` ：显示整个 Docker 的系统信息；
- `docker run hello-world`：运行 hello-world 容器（IT 祖传技能）。

关于 docker 命令使用，可以使用 docker --help or `docker command --help` 获取提示信息。

## Docker 命令结构

旧的：docker <command> (options)

例如:

```shell
docker run hello-world
```

新的：docker <command> <sub-command>(options)

例如：

```
docker container run hello-world
```

他们的输出结果一样，原来的`docker run `命令后续版本也是兼容的，新版本的`docker container run hello-world`命令意思更直观。

## 容器 VS 虚拟机

容器是一种更小更轻便的虚拟化技术。

### 主要区别：

|          | 容器                                                         | 虚拟化                                                       |
| :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 系统级别 | 容器共享主机的操作系统内核，并在用户空间运行。每个容器都有自己的进程空间，文件系统，网络栈等。 | 虚拟机每个都有自己的完整操作系统，包括内核和用户空间，运行在硬件虚拟化层之上。 |
| 资源使用 | 容器只需要加载运行应用所需的库和二进制文件，因此启动更快，占用的资源更少。 | 虚拟机需要运行完整的操作系统，因此会占用更多的系统资源，如CPU，内存和磁盘空间。 |
| 隔离性   | 容器的隔离主要依赖于 Linux 的 Namespace 和 Cgroups，它们在一定程度上隔离了进程空间，网络，文件系统和资源使用等。 | 虚拟机提供了更强的隔离性，因为每个虚拟机都运行在自己的操作系统中，彼此之间完全隔离。 |
| 便携性   | 容器可以在任何运行 Docker 或兼容 OCI 标准的平台上运行，提供了“一次打包，到处运行”的能力。 | 虚拟机也具有便携性，但由于其包含完整的操作系统，因此在迁移时可能会遇到更多的兼容性问题。 |
| 应用场景 | 容器适合微服务架构，CI/CD，DevOps 等快速迭代和高扩展性的场景。 | 虚拟机适合需要完整操作系统环境的应用，或需要更强隔离性的场景。 |

### 主要共同点：

| 共同点           | 容器和虚拟化                                                 |
| :--------------- | :----------------------------------------------------------- |
| **隔离性**       | 容器和虚拟化都提供了运行环境的隔离，可以在隔离的环境中运行应用，互不干扰。 |
| **提高系统效率** | 两者都可以更高效地利用物理硬件资源，通过在同一台物理机上运行多个虚拟环境来提高资源使用效率。 |
| **提高可移植性** | 两者都能实现应用的封装和打包，增强应用在不同环境中的可移植性。 |
| **方便管理**     | 容器和虚拟化都有配套的管理工具，可以方便地创建、启动、停止和删除环境。 |
| **用于多种场景** | 两者都广泛应用于各种场景，如应用部署、测试、开发环境搭建等。 |

## 镜像

Docker镜像（Docker Image）是构建Docker容器的基础。它包含了运行一个应用所需的所有内容：应用程序本身、依赖的库、运行环境和系统工具等。

- 镜像是我们想要运行的应用程序

- 容器是该镜像的一个实例，作为一个进程运行。
- 你可以在同一个镜像上运行许多容器
- Docker 的默认镜像 "注册处 "叫做 [Docker Hub](hub.docker.com)

## 运行一个 nginx 容器

```shell
# 下载镜像
docker pull nginx

# 使用 nginx 镜像运行一个容器实例
docker container run --publish 80:80 nginx

# 检查 nginx 是否正常工作
curl localhost:80
```



