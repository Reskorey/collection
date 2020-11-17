### **Lotus 节点搭建**

Ubuntu 系统安装lotus节点

1.lotus需要安装以下依赖或者工具

go (1.14 or higher)

gcc (7.4.0 or higher)

git (version 2 or higher)

bzr (some go dependency needs this)

jq

pkg-config

opencl-icd-loader

opencl driver (like nvidia-opencl on arch) (for GPU acceleration)

opencl-headers (build)

rustup (proofs build)

llvm (proofs build)

clang (proofs build)



2.安装依赖命令

sudo apt update

sudo apt install mesa-opencl-icd ocl-icd-opencl-dev gcc git bzr jq pkg-config curl 

sudo apt upgrade

 

3.安装Go 1.14版本或者更高版本

安装链接参考官方文档：https://golang.org/doc/install

 

4.克隆Lotus 仓库

git clone https://github.com/filecoin-project/lotus.git

cd lotus/

 

5.编译代码并安装

make clean && make all

sudo make install

 

6.安装后设置lotus 为全局命令，可以直接运行

lotus daemon

 

如果国内运行lotus节点，需要先设置代理：

直接执行以下命令或者将此设置为全局。

IPFS_GATEWAY="https://proof-parameters.s3.cn-south-1.jdcloud-oss.com/ipfs/"

 

7.运行lotus 后。 

lotus net peers  命令查看节点连接数。

lotus help 查看命令帮助

 

注：CID是一种用于分布式信息系统中的引用内容格式，它是一种存储信息的方式，因此可以根据其内容而不是其位置来检索它，DataCID 专门用于表示FileCoin分布式网络中存储的文件。