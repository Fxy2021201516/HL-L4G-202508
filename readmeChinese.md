这个项目名为 **L4G: Two-hop Label Management for Group Steiner Tree Search on Graphs**，可以翻译为 **L4G: 用于图上分组斯坦纳树搜索的两跳标签管理**。

这是一个关于图算法的研究项目。核心是使用一种叫做“两跳标签 (Two-hop Label)”的技术来高效地解决一个经典的图论难题——“分组斯坦纳树 (Group Steiner Tree)”问题。

-----

## HL-GST\_data (数据文件)

这部分介绍了实验所使用的数据。

首先，所有数据文件都被打包在一个压缩包里，使用前需要先解压。项目提供了六个不同的数据集：**Musae**, **Twitch**, **Github**, **Amazon**, **DBLP**, **Reddit**。

每个数据集都包含 7 个文件。我们以 Musae 数据集为例来说明：

1.  **"exp\_musae\_info.txt"**
      * **作用**：这是一个**可读的文本文件**，包含了该数据集的基本信息，例如虚拟顶点、非虚拟顶点和边的数量。
      * **内容解释**：文件里也展示了图的每一条边和它的权重。
          * 例如 `"Edge 0 1 1000000"` 表示顶点 0 和顶点 1 之间有一条**虚拟边 (mock edge)**，其权重是一个固定的常量 1,000,000。
          * 例如 `"Edge 0 4 51"` 表示顶点 0 和顶点 4 之间有一条**非虚拟边 (non-mock edge)**，其权重是 51。这个权重代表了这两个顶点之间的**Jaccard距离 (Jaccard distance)** 为 0.51。
      * **关键概念**：
          * **虚拟顶点 (mock vertices)** 只通过虚拟边与非虚拟顶点相连。
          * 每条**虚拟边 (mock edge)** 连接一个虚拟顶点和一个非虚拟顶点。
          * 根据上面的例子，我们可以推断出顶点 0 和 4 是**非虚拟顶点**，而顶点 1 是一个**虚拟顶点**。

-----

接下来的 6 个文件是**二进制格式**的。使用二进制格式是为了在进行实验时，程序可以**更快地读取数据**，提高效率。

2.  **"exp\_musae\_new\_is\_mock\_Jacard.binary"**

      * **作用**：用布尔值 (bool) 记录每个顶点是**虚拟顶点**还是**非虚拟顶点**。

3.  **"exp\_musae\_input\_graph\_Mock\_Jacard.binary"**

      * **作用**：包含了转换后图的邻接表，这个图**同时包含**虚拟和非虚拟部分。

4.  **"exp\_musae\_input\_graph\_noMock\_Jacard.binary"**

      * **作用**：包含了原始图的邻接表，这个图**只包含**非虚拟部分。

5.  **"exp\_musae\_select\_groups1\_Jacard.binary"**

      * **作用**：包含了为实验随机选择的顶点组集合，用于查询分组斯坦纳树。每个集合包含 **3 个顶点组**。

6.  **"exp\_musae\_select\_groups2\_Jacard.binary"**

      * **作用**：同上，但每个集合包含 **5 个顶点组**。

7.  **"exp\_musae\_select\_groups3\_Jacard.binary"**

      * **作用**：同上，但每个集合包含 **7 个顶点组**。

-----

## HL-GST\_code (代码文件)

这部分介绍了项目的代码结构。

代码文件分为头文件（`.h`）和源文件（`.cpp`）。

  * 头文件（`.h`）位于 `HL-GST_code\include` 目录下。
  * 源文件（`.cpp`）位于 `HL-GST_code\src` 目录下。

其中，两个核心的实验启动文件是：

  * **"exp\_HL4GST.cpp"** (位于 `HL-GST_code\src\label_generation`)
      * **作用**：用于进行论文中的**标签生成 (label generation)** 实验。
  * **"exp\_HL4GST\_nonHOP\_maintain.cpp"** (位于 `HL-GST_code\src\nonHOP_maintain`)
      * **作用**：用于进行论文中的**标签维护 (label maintenance)** 实验。

其他更具体的算法实现代码可以从上述两个文件追溯到：

  * `HL-GST_code\include\label_generation` 目录包含了**标签生成**实验中实现的算法源码。
  * `HL-GST_code\include\nonHOP_maintain` 目录包含了**标签维护**实验中实现的算法源码。

-----

## Compiling Code example (编译与运行示例)

这部分提供了详细的编译和运行指南。作者是在一个配置较高的 Linux 服务器（Ubuntu 20.04.1 系统）上进行实验的。

### 环境准备

假设你在 Linux 服务器上的用户名是 "root"，需要准备以下环境：

1.  **下载并解压数据集**

      * 假设解压后的数据文件夹路径为 `root/HL-GST_data`。

2.  **下载并解压 Boost 库**

      * Boost 是一个非常流行的 C++ 开源库集。你需要从官网 ([https://www.boost.org](https://www.boost.org)) 下载。
      * 假设解压后的 Boost 库路径为 `home/boost_1_75_0`。

3.  **下载并解压代码**

      * 假设解压后的代码文件夹路径为 `root/HL-GST_code`。

### 编译与运行

环境准备好后，就可以使用 `HL-GST_code\sh` 目录下的脚本文件来编译和运行代码了。

#### **运行标签生成实验**

在 `root` 终端中，执行以下命令：

```bash
sh HL-GST_code/sh/run_label_generation_exp.sh
```

这个脚本文件 (`run_label_generation_exp.sh`) 的内容如下：

```bash
g++ -std=c++17 -I/home/boost_1_75_0 -I/root/HL-GST_code/include /root/HL-GST_code/src/label_generation/exp_HL4GST.cpp -lpthread -O3 -o A
./A
```

**命令解释**:

  * `g++`: 调用 C++ 编译器。
  * `-std=c++17`: 指定使用 C++17 标准。
  * `-I/home/boost_1_75_0`: `-I` 参数用于指定头文件搜索路径，这里是告诉编译器去哪里找 Boost 库的头文件。
  * `-I/root/HL-GST_code/include`: 告诉编译器去哪里找这个项目自身的头文件。
  * `/root/HL-GST_code/src/label_generation/exp_HL4GST.cpp`: 要编译的主程序源文件。
  * `-lpthread`: 链接 `pthread` 库，用于支持**并行计算 (多线程)**。
  * `-O3`: 开启**最高级别的编译优化**，让程序运行得更快。
  * `-o A`: `-o` 参数用于指定输出的可执行文件名，这里是 `A`。
  * `./A`: 编译成功后，运行生成的可执行文件 `A`。

实验结果会自动保存在 **CSV 文件**中。

#### **运行标签维护实验**

这个实验分为两步：

1.  **首先，需要生成初始标签**。执行以下命令：

    ```bash
    sh HL-GST_code/sh/run_nonHOP_maintain_exp_generateLPPR.sh
    ```

2.  **然后，再运行标签维护实验**。执行以下命令：

    ```bash
    sh HL-GST_code/sh/run_nonHOP_maintain_exp.sh
    ```

作者在论文中的所有实验都是通过上述方法进行的。