# 🟢 Sentinel-K 启动内核 (System Kernel)

> [!IMPORTANT]
> **SSOT (Single Source of Truth) 说明**: 本文件是 Sentinel-K 项目的唯一权威口径。所有其他文档及 AI 工作流中的术语、阈值和规则必须引用本文件定义的锚点。

## 1. 术语表 (Glossary) `[K-TERM]`
- **指挥官 (Commander)**: 人类。负责定义边界 (Intent)、审计安全 (Audit)、验收结果 (Accept)。
- **执行官 (Agent)**: AI。负责推演逻辑 (Reasoning)、原子执行 (Execution)、提供证据 (Evidence)。
- **坚固资产 (Solid Asset)**: 风险极高的核心文件，触碰需遵循 `[K-ADR]`。详见 `[K-ASSET]` 分级。
- **证据块 (Evidence Block)**: 代码交付时必须附带的物理验证摘要，格式见 `[K-EVIDENCE]`。
- **授权令牌 (Approval_Hash)**: 用于 `Solid-Strict` 资产变更的人类临时授权标识。
- **合规回扣 (ADR_Entry)**: 指向某条 ADR 的引用与回扣说明（Liquid-only 允许写 `N/A`）。

> [!NOTE]
> **命名统一**: 为避免口径漂移，本文以 `Approval_Hash` / `ADR_Entry` / `Compliance_Status` 作为标准键名；出现 Legacy 写法（如 "Approval Hash" / "ADR Entry" / "Compliance Status"）时，视为同义别名。

## 2. 语言协议 (Language Protocol) `[K-LANG]`
- **原则**: 除代码符号、日志关键字及错误堆栈外，所有对话、文档 (Artifacts)、思考过程必须使用 **中文 (简体)**。

## 3. 启动协议 (Boot Protocol) `[K-BOOT]`
- **Pack-0 (Kernel)**: 内核基石。包含 `00_总纲` + `05_进度` + `.cursorrules` (或项目级指令)。
- **Pack-1 (Bone-Arch)**: 架构蓝图。在 Pack-0 基础上加载 `01_全景` + `02_规约` + `06_拓扑`。
- **Pack-2 (Muscle-Impl)**: 执行包。在 Pack-1 基础上加载 业务代码 + `03_决策日志` + `04_答疑库`。

## 4. 协作法则 (Core Laws)

### 4.1 切披萨法则 (Pizza Logic) `[K-PIZZA]`
- **判定阈值**: 预估变更内容 **> 3 个文件** 或 **> 100 行代码**。
- **强制约束**: 触发阈值后，必须物理拆分为 `Sub-Intent`。
    - **Stage A (Bone/骨架)**: 仅定义接口、DTO、契约常数。
    - **Stage B (Stub/桩点 - 可选)**: 实现 Mock 返回并跑通端到端链路。
    - **Stage C (Muscle/肌肉)**: 实现具体业务逻辑与标准证据交付。
- **协作模式**:
    - **默认两片**: Stage A + Stage C (跳过 Stub)。
    - **可选三片**: Stage A + Stage B + Stage C (复杂/高风险推荐)。

### 4.2 资产分级治理 (Asset Grading) `[K-ASSET]`
- **Solid-Strict (强管制)**: 对外契约、SQL、CI、.env、核心依赖。
    - **动作**: 必须在 `03_决策日志 (ADR).md` 登记新条目，并提供任务级 `Approval_Hash`。
    - **证据**: 证据块必含 `ADR_Entry` 与 `Asset_Update`。
- **Solid-Regulated (受控)**: 核心领域模型、关键业务策略。
    - **动作**: 必须在 `03_决策日志 (ADR).md` 登记新条目。
    - **证据**: 证据块必含 `ADR_Entry` 与 `Asset_Update`。
- **Liquid (流动)**: 业务实现、Controller、新功能测试。
    - **动作**: 自由演进，无需 ADR。
    - **证据**: 满足标准证据链即可。

### 4.3 反向测试证明 (Reverse Testing) `[K-TEST]`
- **场景**: 核心算法、安全拦截、复杂状态转换。
- **流程**: 先写测试证明其失败 (Red) -> 实现业务 (Green) -> 故意破坏业务逻辑再次确认失败 (Destructive) -> 恢复 (Recover)。

### 4.4 验证证据标准 (Evidence Standard) `[K-EVIDENCE]`
交付物必须包含以下标准键名的证据块。

**标准交付模板 (Copy-paste Ready)**:
```markdown
### 📢 Verification Evidence (IDE)
- **Compliance_Status**: 🟢 [Pass] | ⚠️ [Warning] | 🔴 [Partial]
- **Context_Log**: [Pack-0/1/2] + [Extra Files]
- **Diff_Summary**: [改动文件数] | [逻辑摘要]
- **Test_Run**: [Runner] -> [Pass/Total] | [Result Link/Img]
- **Scan_Result**: [Runner] -> [Block/Critical Errors Count]
- **Asset_Update**: [Solid-Strict/Regulated 的变更留痕 | Liquid-only 写 NO_CHANGE]
- **ADR_Entry**: [ADR-ID] -> [Decision Point Review | Liquid-only 写 N/A]
- **Reverse_Proof**: [Position] -> [Expected Error] -> [Recovered | 若不适用写 N/A]
```

**可复核加固 (Recommended/Audit-Ready)**:
- `Evidence_Source`: 原始证据来源（如截图路径、日志摘要）。
- `Artifact_Ref`: 关联的工件引用。
- `Raw_Excerpt`: 代码/日志的原始片段摘录。

### 4.5 信心评分标准 (Confidence Score) `[K-SCORE]`
每次主要交互必须输出 **Confidence Score Card** (整数 0-100)：
- **[90 - 100] (Pass)**: 路径清晰，证据齐备，准予执行。
- **[80 - 89] (Warning)**: 存在模糊点或潜在风险，需人类介入或 ACK。
- **[0 - 79] (Block)**: 关键缺失或严重歧义，强制熔断 (Circuit Break)。

**Score Card 模板**:
```markdown
### 🧠 Confidence Score Card
- **Score**: [0-100]
- **Status**: [🟢/⚠️/🔴]
- **Reasoning**: [打分逻辑简述]
- **Gaps**: [遗留风险/知识盲区]
- **Action**: [Proceed | Need_ACK | Circuit_Break]
```

## 5. 架构决策记录 (ADR Rules) `[K-ADR]`
- **触发条件**: 修改 `Solid Asset` 或引入新的技术栈、重大设计变更。
- **存储位置**: `Sentinel-K_内核/03_决策日志 (ADR).md`。

---
> *修订记录:*
> - *2026-02-16 - 建立 SSOT 权威口径*
> - *2026-02-16 - 引入 Stub (Stage B) 三段式 Pizza 模型*
> - *2026-02-16 - 补全资产分级、增强证据链及评分熔断协议 (Standardization Patch)*
