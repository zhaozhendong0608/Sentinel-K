# 项目全景图 (Project Overview)

> 此文件是 AI 理解项目的核心入口。请保持更新。

## 1. 项目元数据
- **项目名称**: 通用电力数据融合智能分析平台 (Universal Power Data Fusion Platform)
- **核心目标**: 构建适配异构电站数据的通用平台，实现多源数据融合、统一模型建模与智能分析。
- **技术栈**: 
  - **后端**: Java (Spring Boot), Netty (协议采集), Calcite (异构数据查询 - 待定)
  - **大数据/存储**: TSDB (**TDengine**), MySQL (元数据 & 资产模型)
  - **前端**: Vue.js, ECharts, X6 (DAG 编排)
- **架构模式**: 多源融合层 (Data Fusion) -> 统一模型 & 算子层 (Model & Orchestration) -> 智能应用层 (Intelligent App)

## 2. 核心业务全景
- **数据源融合层 (Southbound Fusion)**: 
  - 支持 **多源异构接入**: 既支持实时协议 (IEC 104, Modbus)，也支持直连第三方库 (MySQL, TDengine)。
  - 实现 **动态映射**: 将异构数据源标准化为统一内部格式。
- **核心模型 & 算子层 (Core Model & Ops)**: 
  - **资产树 (Asset Tree)**: 建立 `机组-设备-部件-点位` 四层逻辑模型 (参考 IEC 61850)。
  - **算子编排 (Orchestration)**: 提供可视化画布，编排清洗、计算与分析算子 (如振动区识别、工况转换)。
- **北向应用 (Northbound)**: 
  - 算法驱动的智能报表。
  - 实时监测大屏、故障诊断、能效分析。

## 3. 当前里程碑 (Current Milestone)
- **阶段**: 需求分析与架构设计
- **重点**: 设计“多源融合适配器”与“算子编排引擎”。

## 4. 已知风险与技术债 (Known Issues)
- **时序库选型**: 已锁定 **TDengine** (标准 SQL + 超高写入性能)。
- **编排引擎**: 确认使用 **LiteFlow** (后端执行) + **AntV X6** (前端绘制)。
- **协议多样性**: 现场私有协议接入仍需脚本化支持。

---
> [!NOTE]
> **维护指引**: 
> 1. 修改 `Solid Asset` 时，必须在此同步受影响范围。
> 2. 产生的短期技术债应在此登记，并在 `05_项目进度 (Tasks).md` 中排期清理。
