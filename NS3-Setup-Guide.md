# 🚗 智慧停车（华为星闪 + 鸿蒙 + CV2X）需求规格说明书
 
**版本：** V1.0  
**日期：** 2026-03-22  
**状态：** 正式定稿  

---

## 1. 文档概述

### 1.1 项目背景
基于华为星闪 NearLink、鸿蒙车机、CV2X、华为云 / 边缘、国密安全构建新一代无感智慧停车系统，实现：
车辆进场自动发现 → 自动分配最优车位 → 鸿蒙车机实时导航 → 离场自动账单 → 无感支付通行。

### 1.2 文档用途
* 产品、研发、测试、交付统一执行依据
* 项目立项、需求评审、验收标准文件

### 1.3 核心技术栈
* 华为星闪 NearLink（发现 / 定位 / 通信）
* 鸿蒙车机原子化服务 + NearLink Kit
* CV2X 车路协同
* 华为云 IoT + 边缘计算
* 国密 SM2/SM3/SM4

### 1.4 优先级定义
* **P0：必做**（核心流程，必须上线）
* **P1：应做**（体验增强，二期）
* **P2：可选**（高阶扩展，三期）

### 1.5 项目设计思路
本项目严格遵循第一性原理进行顶层设计，摒弃传统智慧停车“设备叠加、流程优化”的改良式思路，回归停车场景最本质的需求、最本质的技术约束、最本质的系统关系，从底层逻辑重构无感停车全链路。
* **回归本质：** 停车的本质是车辆与车位的精准匹配，通行的本质是身份可信与快速响应，体验的本质是减少用户一切主动操作。
* **技术重构：** 放弃传统蓝牙、WiFi、摄像头体系，直接选用华为星闪 NearLink 作为底层连接与定位底座，以鸿蒙车机作为原生入口，以 CV2X 作为车路协同扩展，满足毫秒级发现、亚米级定位、高并发连接等底层能力。
* **架构重塑：** 车辆是主体、车场是服务、云端是中枢，实现机器与机器的直接协同。构建星闪近场发现→鸿蒙可信身份→自动鉴权开闸→智能分配车位→车机无感导航→自动计费支付→自动放行的全闭环。

---

## 2. 总体技术架构

### 2.1 五层架构
1. **感知层：** 星闪锚点、UWB、地磁、视频桩、道闸
2. **边缘层：** 华为边缘网关、定位解算、本地控制
3. **服务层：** 鉴权、车位调度、融合定位、计费、支付
4. **车机层：** 鸿蒙原子服务、导航、账单、华为钱包
5. **云平台：** 华为云 IoT、大数据、对账、运营后台

---

## 3. 核心业务流程

### 3.1 无感停车全流程
1. 车辆驶入车场覆盖范围。
2. 星闪网关实现**毫秒级自动发现**。
3. 身份鉴权：星闪 ID + VIN + 鸿蒙 ID 绑定校验。
4. 鉴权通过，联动边缘计算实现**无感开闸**。
5. 核心服务下发**车位预分配**指令，鸿蒙车机启动室内导航。
6. 车辆入位，感知层确认，开始计费。
7. 车辆驶出，系统自动核算费用并推送账单至鸿蒙车机。
8. 华为钱包自动扣款，道闸放行。

---

## 4. 功能需求

### 4.1 星闪 NearLink（P0）
* 近场自动发现、连接、重连
* 双向身份认证
* SLP 高精定位（ToF/PDoA）
* 低时延数据透传通道

### 4.2 鸿蒙车机（P0）
* 原子化服务免安装
* 车位 / 路线 / 账单展示
* 2D/3D 车场地图导航
* 华为钱包支付

### 4.3 CV2X（P1）
* RSU 消息解析（BSM/RSI/MAP）
* 进场前预分配车位
* VIN/TBox 可信接入

### 4.4 车位调度（P0）
* 实时车位状态采集
* 最优车位推荐
* 新能源 / 无障碍优先
* 车位锁定 3–5 分钟

### 4.5 融合定位引擎（P0）
* 星闪 + UWB + 惯导融合
* 亚米级定位
* 楼层识别、跳变抑制
* 动态路径规划

### 4.6 计费与账单（P0）
* 按时 / 分段 / 封顶 / 节假日规则
* 离场自动推送账单
* 订单、流水、对账

### 4.7 道闸联动（P0）
* 认证通过自动开闸
* 支付完成自动开闸
* 断网本地可用

### 4.8 安全合规（P0）
* 国密加密通信
* 双向可信认证
* 数据脱敏、日志审计≥6 个月

### 4.9 运营后台（P0/P1）
* 车场 / 车位 / 收费配置
* 实时监控大屏
* 订单 / 对账 / 报表
* 设备状态监控

---

## 5. 开发任务拆解

### 5.1 星闪模块（P0）
1. 星闪网关 / 锚点驱动适配
2. 扫描、连接、认证管理
3. SLP 定位解算（ToF/PDoA）
4. 车场数据透传服务

### 5.2 鸿蒙车机（P0）
1. 鸿蒙原子化服务开发
2. NearLink Kit 封装
3. 导航 / 账单 UI 开发
4. 华为钱包支付对接

### 5.3 CV2X（P1）
1. RSU 数据接入
2. 进场前预分配逻辑
3. TBox 数据融合

### 5.4 后端核心服务（P0）
1. 车辆身份鉴权服务
2. 车位分配与调度服务
3. 融合定位引擎
4. 计费、订单、支付服务
5. 道闸联动服务
6. 国密安全服务

### 5.5 华为云 / 边缘（P0）
1. 边缘本地控制
2. 断网续传
3. IoT 设备接入与管理
4. 数据存储与统计

### 5.6 运营后台（P0）
1. 基础配置
2. 实时监控大屏
3. 订单 / 对账 / 报表
4. 设备管理

---

## 6. 接口规范

### 6.1 星闪 NearLink 接口（P0）
* `starflash:scan` 扫描网关
* `starflash:connect` 建立连接
* `starflash:auth` 身份认证
* `starflash:loc:result` 定位结果
* `starflash:push:parkSpace` 车位推送
* `starflash:push:bill` 账单推送

### 6.2 鸿蒙车机接口（P0）
* `nearlink:init` 初始化
* `nearlink:callback:found` 发现网关
* `harmony:atomic:launch` 拉起服务
* `harmony:ui:navi` 导航界面
* `harmony:pay:createOrder` 创建订单
* `harmony:pay:result` 支付结果

### 6.3 业务 HTTP 接口（P0）
* `/api/v1/vehicle/auth` 车辆鉴权
* `/api/v1/park/assign` 分配车位
* `/api/v1/vehicle/location` 定位上报
* `/api/v1/park/start` 开始停车
* `/api/v1/bill/get` 获取账单
* `/api/v1/pay/notify` 支付通知
* `/api/v1/gate/open` 开闸指令

---

## 7. 数据库设计

### 7.1 设计原则
* 以车辆 - 车场 - 车位 - 订单为主线，结构清晰、易于扩展
* 边缘侧使用 Redis 做实时状态存储，云端使用 MySQL 做持久化
* 日志独立分表，不影响核心业务性能
* 支持国密标识、星闪 ID、鸿蒙 ID、VIN 统一关联

环境配置：

---

## Installation

### 1. Installing on the host machine (NS-3 仿真环境)

**Step 1. Install prerequisites**
在 Windows 11 的 WSL2 (Ubuntu) 环境下，首先安装必要的 C++ 编译环境：

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install g++ python3 python3-dev pkg-config sqlite3 cmake ninja-build git tar wget -y
```
Step 2. Download and Build NS-3
下载官方源码并进行全量编译：
```bash
wget [https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2](https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2)
tar xfj ns-allinone-3.41.tar.bz2
cd ns-allinone-3.41/ns-3.41
./ns3 configure --enable-examples --enable-tests
./ns3 build
```
Step 3. Backend Services Build (核心服务端构建)
使用 Docker 快速拉起本地边缘网关与核心数据库（包含 MySQL 与 Redis）：
```
docker-compose -f docker/docker-compose.yml up -d
```
Step 4.Quick Start
Run the Communication Simulation (运行通信仿真)
进入 NS-3 编译目录，一键执行智慧停车场景下的星闪连接与车位预分配仿真：
```
cd ns-allinone-3.41/ns-3.41
./ns3 run scratch/smart-parking
```
<img width="2559" height="862" alt="image" src="https://github.com/user-attachments/assets/3c1f60b7-b119-4b38-9155-0e4bc434be08" /> 
环境配置成功实例



