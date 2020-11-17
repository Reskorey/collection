### **Go-filecoin 节点搭建**

**1.系统要求**

Go-filecoin可以在大多数GNU / Linux和MacOS系统上构建并运行。Windows尚不支持。

验证节点可以在具有至少8GB RAM的大多数系统上运行。采矿节点特别需要大量的RAM和GPU资源，具体取决于要实现的扇区配置。

 

2.**安装依赖项和系统配置**

克隆go-filecoingit仓库并输入：

 mkdir -p /path/to/filecoin-project

 git  clone  https://github.com/filecoin-project/go-filecoin.git    /path/to/filecoin-project/go-filecoin



**3.安装Go**

构建过程go-filecoin要求[Go](https://golang.org/doc/install) > = v1.13。

第一次安装Go吗？我们建议您使用[本教程](https://www.ardanlabs.com/blog/2016/05/installing-go-and-your-workspace.html)，其中包括环境设置。

由于使用cgo  in go-filecoin，无论使用的是预构建的库还是从源代码编译的库，都需要C编译器来构建。要使用gcc（例如export CC=gcc），需要v7.4.0或更高版本。

构建过程将下载一个静态库，其中包含[Filecoin证明实现](https://github.com/filecoin-project/rust-fil-proofs)（用Rust编写）。

**注意：**要从源代码构建证明，（1）必须安装Rust开发环境，并且（2）FFI_BUILD_FROM_SOURCE=1必须设置环境变量。可以在[filecoin-ffi中](https://github.com/filecoin-project/filecoin-ffi)找到更多信息。



**4.安装依赖**

1.加载所有的Git子模块：

git submodule update --init --recursive

2.初始化构建依赖关系：

make deps

**注意：**第一次deps启动可能会**很慢**，因为非常大的参数文件是在中下载或本地生成的/var/tmp/filecoin-proof-parameters。有耐心; 未来的运行会更快。



**5.构建Filecoin并运行测试**

1.构建二进制文件：

make

2.运行单元测试：

go run ./build test

3.可以选择将构建和测试结合在一起：

go run ./build best

4.其他方便的构建命令包括：

\# Check the code for style and correctness issues

go run ./build lint

\# Run different categories of tests by toggling their flags

go run ./build test -unit=false -integration=true -functional=true

\# Test with a coverage report

go run ./build test -cover

\# Test with Go's race-condition instrumentation and warnings (see https://blog.golang.org/race-detector)

go run ./build test -race

\# Deps, Lint, Build, Test (any args will be passed to `test`)

go run ./build all

**注意：**传递给go run ./build test（例如-cover）的任何标志都将传递给go test。

**对于该版本的所有问题**，请查阅本文档的“ [疑难解答”](https://go.filecoin.io/go-filecoin-tutorial/Troubleshooting-&-FAQ.html)部分。



**6.开始运行Filecoin**

1.如果go-filecoin以前在系统上运行过，请删除现有的Filecoin存储库（**这将删除所有以前的Filecoin数据**）：

rm -rf ~/.filecoin

2.初始化go-filecoin：

go-filecoin init --genesisfile=https://ipfs.io/ipfs/QmXZQeezX1x8uRQX9EUaYxnyivUpTfJqQTvszk3c8SnFPN/testnet.car --network=testnet

3.启动go-filecoin守护程序：

go-filecoin daemon

这应该返回“我的对等ID为<peerID>”，其中以“ Qm”开头<peerID>的长[CID](#cid)字符串是。

4.打印引导节点地址列表：

go-filecoin config bootstrap.addresses

5.从您刚刚打印的列表中选择任何地址，然后连接到它（自动对等点发现和连接即将推出。）：

go-filecoin swarm connect <any-filecoin-node-mulitaddr>

**注意：**第一次可能会**很慢**。filecoin节点需要一个大的参数文件作为证明，存储在中/tmp/filecoin-proof-parameters。它通常由deps构建步骤生成。如果缺少这些文件，它们将被重新生成，最多可能需要一个小时。我们正在努力寻找更好的解决方案。

6.检查节点的连接性：

go-filecoin swarm peers                  # lists addresses of peers to which you're connected

7.对等方地址的最后一个段是其peerID。使用以下命令直接测试与对等方的连接：

go-filecoin ping <peerID>      # Pings the peer and displays round-trip latency.



现在，该节点应连接到一些对等节点，并将开始下载和验证区块链。

 