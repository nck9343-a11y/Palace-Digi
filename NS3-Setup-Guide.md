# 🚗 智慧停车系统 (Palace-Digi) - NS-3 仿真环境配置指南

本文档旨在指导团队成员在 Windows 11 环境下，通过 WSL2 (Ubuntu) 从零搭建完整的 NS-3 (v3.41) 网络仿真环境，用于后续的星闪 (NearLink) 与 C-V2X 通信仿真测试。

## ⚠️ 核心要求
* **操作系统：** Windows 10 (2004及以上版本) 或 Windows 11
* **磁盘空间：** C盘或系统盘至少预留 20GB 空闲空间
* **网络环境：** 保持网络畅通（下载源码需要较好网络）

---

## 🛠️ 第一步：安装 Windows 的 Linux 子系统 (WSL2)
*注：如果你电脑上已经配置好了 WSL2 和 Ubuntu 环境，可直接跳过此步。*

1. 在 Windows 搜索栏搜索 **PowerShell**，右键选择 **“以管理员身份运行”**。
2. 在终端中执行以下命令安装默认的 Ubuntu 系统：
   ```powershell
   wsl --install
   等待安装进度条跑完后，重启电脑。

重启后会自动弹出 Ubuntu 终端窗口，按照提示设置你的 UNIX 用户名和密码（输入密码时不可见，正常输入完回车即可），常见密码为123。
打开安装好的 Ubuntu 终端，依次执行以下命令，为 NS-3 准备 C++ 编译环境：
更新软件源（如果遇到卡顿，建议提前配置好 Ubuntu 的国内镜像源）：
sudo apt update && sudo apt upgrade -y

安装核心构建工具与依赖：


sudo apt install g++ python3 python3-dev pkg-config sqlite3 cmake ninja-build git tar wget -y
第三步：下载与解压 NS-3 源码
我们使用 NS-3.41 官方全家桶稳定版：
1.下载源码包：
wget [https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2](https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2)
2.解压
tar xfj ns-allinone-3.41.tar.bz2
进入工作目录（后续所有的仿真代码都在这个目录下执行）：


cd ns-allinone-3.41/ns-3.41
第四步：配置与编译 NS-3
这是最关键的一步，系统会将源码编译为可执行的仿真引擎。
1.配置工程（开启官方示例和测试代码）：
./ns3 configure --enable-examples --enable-tests
2.开始编译：
./ns3 build
这一步会调用 CPU 全速进行编译，需要编译近 2000 个 C++ 文件，耗时约 5~30 分钟不等。期间电脑风扇狂转、CPU 满载属于正常现象，请耐心等待终端返回至输入提示符。
第五步：验证安装
编译彻底完成后，在 ns-3.41 目录下执行以下命令，运行系统自带的测试脚本：


./ns3 run hello-simulator
如果终端成功输出 Hello Simulator，恭喜你！NS-3 仿真环境已完美安装成功。
如何运行我们自己的仿真脚本？
团队自己的 C++ 仿真脚本统一存放在 ns-3.41/scratch/ 目录下。

运行脚本时，不需要加 .cc 后缀。例如运行 scratch/smart-parking.cc：


./ns3 run scratch/smart-parking



