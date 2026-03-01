# miniMNIST-C
本项目是对 Konrad Gajdus 的 [miniMNIST-c](https://github.com/konrad-gajdus/miniMNIST-c) 的改进与增强。它保留了原有的简洁结构和学习价值，同时针对错误处理和文件格式进行了稳健性的提升，使程序更稳定、更易调试。

## 🎯 改进说明（相比原版）
原版代码非常简洁，但在错误处理方面比较薄弱（如文件打开失败直接 `exit(1)`，无任何提示）。我们进行了以下增强，完全不影响原有逻辑，但让程序更健壮：

1. **详细的错误信息**
   - 文件打开失败、内存分配失败、读取数据不完整时，会打印具体原因（如文件名、期望字节数等），方便定位问题。

2. **魔数验证**
   - 读取图片文件时检查魔数是否为 `2051`，标签文件检查是否为 `2049`，确保文件确实是 MNIST 格式，避免后续计算错乱。

3. **图片尺寸验证**
   - 验证文件头中的行数和列数是否均为 28 (`IMAGE_SIZE` 宏定义)，防止因文件损坏或错误导致数据错位。

4. **内存分配检查**
   - `malloc` 失败后不会直接崩溃，而是打印错误信息并安全退出。

5. **读取完整性检查**
   - `fread` 后检查实际读取的字节数是否等于期望值，避免因文件截断导致不完整的数据被用于训练。

这些改进让程序在遇到问题时能给出清晰的提示，而不是默默出错或崩溃，非常适合学习调试。

## 📁 项目结构

```
gitignore.txt
mnist
nn
nn.c
data/
    train-images.idx3-ubyte
    train-labels.idx1-ubyte
```

- `nn.c`：主要程序文件，包含训练与读取逻辑。
- `mnist/`：包含 MNIST 加载代码及工具。
- `data/`：存放 MNIST 数据文件。

## 📥 数据下载说明

本仓库不包含 MNIST 数据集，请手动下载并放入 `data/` 文件夹：

下载数据
访问 MNIST 官方网站：
http://yann.lecun.com/exdb/mnist/
下载以下两个文件（只需训练集即可，程序会自动划分训练/测试集）：

- `train-images-idx3-ubyte.gz`（训练图片）
- `train-labels-idx1-ubyte.gz`（训练标签）

解压文件
在终端进入下载目录，执行：

```bash
gunzip train-images-idx3-ubyte.gz train-labels-idx1-ubyte.gz
```

或者在图形界面用解压软件解压。

放置数据
在项目根目录下创建 `data/` 文件夹（如果不存在），将解压后的两个文件放入其中：

```
项目根目录/
├── data/
│   ├── train-images.idx3-ubyte
│   └── train-labels.idx1-ubyte
├── nn.c
├── README.md
├── LICENSE
└── .gitignore
```

验证
编译并运行程序，如果正常开始训练，说明数据放置正确。

## 📄 许可证
本项目基于 MIT 许可证开源，详情见 LICENSE 文件。
原代码版权归 Konrad Gajdus 所有（Copyright (c) 2024），改进部分遵循相同许可证。

---

欢迎使用本代码学习神经网络与 MNIST 数据集！如有问题请查阅源代码中的注释，或者自行增加更多错误检查。祝编码愉快！
