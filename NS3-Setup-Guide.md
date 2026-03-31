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

### 7.2 核心表结构及 DDL 实现

#### 1. vehicle（车辆信息表）
存储车辆唯一身份、星闪 ID、鸿蒙 ID 绑定关系。包含字段：`id`, `vin`, `starflash_id`, `harmony_id`, `car_type`, `create_time`, `status`。

#### 2. parking_space（车位表）
重点管理车位状态流转，防并发冲突。
```sql
CREATE TABLE `parking_space` (
  `space_id` varchar(64) NOT NULL COMMENT '车位唯一编号',
  `park_id` varchar(64) NOT NULL COMMENT '所属车场ID',
  `floor` varchar(16) NOT NULL COMMENT '所在楼层，如 B1, B2',
  `area` varchar(16) NOT NULL COMMENT '所在区域，如 A区, B区',
  `x_coordinate` decimal(10,6) DEFAULT NULL COMMENT '定位X坐标',
  `y_coordinate` decimal(10,6) DEFAULT NULL COMMENT '定位Y坐标',
  `space_type` tinyint(4) NOT NULL DEFAULT '0' COMMENT '类型：0-普通, 1-充电, 2-无障碍',
  `status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '状态：0-空闲, 1-锁定(调度中), 2-占用, 3-故障',
  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`space_id`),
  KEY `idx_park_status` (`park_id`, `status`) COMMENT '用于快速检索空闲车位'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='车位信息表';
3. park_order（停车订单主表）
记录一次完整停车入场→计费→支付→离场的生命周期。

SQL
CREATE TABLE `park_order` (
  `order_id` varchar(64) NOT NULL COMMENT '订单全局唯一ID',
  `vin` varchar(64) NOT NULL COMMENT '车辆VIN码',
  `park_id` varchar(64) NOT NULL COMMENT '车场ID',
  `space_id` varchar(64) DEFAULT NULL COMMENT '最终停入的车位ID',
  `entry_time` datetime NOT NULL COMMENT '入场开闸时间',
  `park_start_time` datetime DEFAULT NULL COMMENT '实际入位(计费)时间',
  `exit_time` datetime DEFAULT NULL COMMENT '离场时间',
  `total_fee` decimal(10,2) DEFAULT '0.00' COMMENT '总计费金额',
  `pay_status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '支付状态：0-未出账, 1-待支付, 2-已支付',
  `order_status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '订单状态：0-进行中, 1-已完成, 2-异常',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`order_id`),
  KEY `idx_vin_status` (`vin`, `order_status`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='停车订单主表';
4. 其他附属表说明
parking_lot（车场表）： 基础信息、楼层、总车位、地图 JSON。

starflash_device（星闪设备表）： 管理星闪网关、锚点位置与在线状态。

fee_rule（计费规则表）： 按车场配置免费时长、小时费、单日封顶规则。

location_log / gate_log / pay_log： 定位轨迹、道闸操作、支付流水日志表。

8. P0 核心测试用例
星闪自动发现时延 ≤ 100ms

身份认证通过 → 自动开闸

新能源车辆优先分配充电位

室内定位精度误差 ≤ 1 米

车机导航路线正确及跳变抑制测试

入位自动开始计费

离场自动推送账单

支付成功 → 自动开闸

断网边缘本地可用性测试

通信加密无明文泄露

9. 项目里程碑
M1（1–4 周）P0 核心上线： 星闪发现/定位、鸿蒙车机基础、车位分配、计费、无感支付。

M2（5–8 周）P1 优化： 语音交互、发票、监控大屏、CV2X 预分配。

M3（9–12 周）P2 扩展： 多车场联动、预约停车、自动泊车对接。

10. 交付物清单
需求规格说明书

鸿蒙车机原子化服务包

星闪 / 边缘 / 后端服务程序源代码

运营管理后台系统

API 接口文档与数据库字典

测试报告与 NS-3 仿真数据

部署 & 运维手册

11. 附录 A：技术标准与参考链接
星闪 NearLink 标准： 国际星闪联盟标准主页

华为星闪开发文档： HarmonyOS NearLink Kit 开发指南

鸿蒙车机开发规范： 原子化服务开发规范

国密安全： SM2/SM3/SM4 算法标准及等保 2.0。

12. 附录 B：NS-3 仿真与测试环境配置指南
为了验证“毫秒级发现”及高并发等第一性原理设计，本项目采用 NS-3 进行底层通信仿真。以下为团队成员统一的环境配置标准。

🛠️ 环境要求与准备
操作系统： Windows 10/11，需开启 WSL2 (Ubuntu 子系统)。

配置命令： 管理员权限运行 PowerShell 输入 wsl --install，完成后重启电脑。

📦 安装依赖与编译源码
在 Ubuntu 终端中依次执行：

Bash
# 1. 更新源并安装 C++ 构建依赖
sudo apt update && sudo apt upgrade -y
sudo apt install g++ python3 python3-dev pkg-config sqlite3 cmake ninja-build git tar wget -y

# 2. 下载并解压 NS-3.41
wget [https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2](https://www.nsnam.org/releases/ns-allinone-3.41.tar.bz2)
tar xfj ns-allinone-3.41.tar.bz2
cd ns-allinone-3.41/ns-3.41

# 3. 配置与全量编译 (该步骤耗时较长，请保持电脑供电)
./ns3 configure --enable-examples --enable-tests
./ns3 build

# 4. 验证安装结果
./ns3 run hello-simulator
# 若输出 "Hello Simulator" 则代表环境配置成功！
📂 脚本运行规范
项目组专属的 C++ 仿真脚本统一存放在 ns-3.41/scratch/ 目录下。运行脚本时无需加 .cc 后缀（例如：./ns3 run scratch/smart-parking）。
