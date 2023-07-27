+++
title = "offline python library install[failure]"
description = "Primera publicación del sitio web."
# slug = "que-hacer-despues-de-instalar-fedora-36"
draft = true

[taxonomies]
    categories = [ "record" ]
    tags = [ "nix", "devenv" ]
+++

## 背景

尝试在开发版上完成对 ROS1 节点的支持，打算先从依赖下手（因为 `catkin` 等工具按照说明是安装 `ROS1` 的必要条件），但是由于开发版上虽然存在 `python` 包，但是无法正常通过联网将对应的 `python` 依赖安装到本板上面，因此尝试通过一些离线安装的方式实现对应的操作。


## 简单依赖下载以及离线安装说明

### pip 以 `tarball` 形式下载 `python` 包

运行下列指令或者脚本，将 `python` 的依赖包下载到当前路径下，这种方法虽然能够根据其依赖申明下载对应的依赖，但是在实测中存在一定的问题，部分用 C 编写的包会因为后面 `install` 时候本机没有对应的 `gnu` 工具链而报错，因此不能算是通用的解决办法。

*--no-binary: 指定的是不下载二进制分发形式的数据(如果目标机和当前机是同一个体系结构也可以正常下载)，目前实测来看只会下载 `tarball`* 
*搞了个 loop 主要是为了防止某些包的依赖没有实现递归下载，在我这边出现了这种情况*

```bash
pip download {package name} -d . --no-binary ":all:"
```

```python
import os
import subprocess

for _ in range(0, 200):
  tar_files = os.listdir(".")

  tar_files = [tar_file for tar_file in tar_files if tar_file.endswith(".tar.gz")]

  for tar_file in tar_files:
    filename = tar_file.split('-')[0]
    command = ["pip", "download", f"{filename}", "-d", ".", "--no-binary", '":all:"']
    print(command)
    subprocess.call(command)
```

### pip 安装 `tarball python` 包

使用一定的工具将下载的一系列 `tarball` 通过传输工具传输到开发板上，将上文中出现的 command 换成下面这条，这样就可以在没有办法连接到 pypi 的时候下载依赖了，但是就如同上文所说的一样，这种方法存在一定的局限性，即在开发版上没有 gcc 等工具的情况下无法安装一些特殊包。

```python
command = ["pip", "install", f"{tar_file}", "--no-index", "--no-deps"]
```

## 通过工具实现下载依赖包的效果

可以尝试通过 [pipdepwalker](https://github.com/jackyliu16/pipdepwalker) 等工具实现下载依赖包的效果。
*但是目前我使用这个工具的简单尝试并不成功，可能需要代码分析以及修改，但是目前没有时间*

## 在本机上编译 `python` 并且移植到目标机器上面去

#TODO