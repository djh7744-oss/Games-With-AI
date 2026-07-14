# 战国 · 乱世风云 — 游戏策划工程

> 以中国战国时期（公元前476年-前221年）为背景的开放世界沙盒游戏。
> 使用 Unreal Engine 5 开发，Blender 制作美术资产。

---

## 工程结构

```
战国策划工程/
├── README.md                    ← 你在这里
├── 000_GDD_概要/                顶层设计 · 游戏愿景与核心支柱
├── 100_系统设计/                核心系统设计文档
├── 200_数据表/                  数值平衡表、配置数据
├── 300_叙事设计/                剧情、人物、对话
├── 400_地图关卡/                地图设计、关卡布局
├── 500_美术规范/                美术风格指南与技术规范
├── 600_音频设计/                音效、音乐、配音设计
├── 700_UI_UX/                   界面与用户体验设计
├── 800_技术设计/                技术方案与架构决策
├── 900_参考资料/                历史考据与游戏参考
└── Templates/                   文档模板
```

## 版本控制规范

### Git 仓库结构

```
main              ← 正式发布版本（与游戏版本同步）
  └── develop     ← 开发主线
       ├── feature/combat-v2     ← 战斗系统V2开发
       ├── feature/economy       ← 经济系统开发
       └── feature/dlc-changping ← 长平之战DLC
```

### 提交信息规范

```
[类型] 简述

类型包括：
  [GDD]    - 策划文档增改
  [DATA]   - 数值平衡调整
  [NARR]   - 叙事/剧情/对话
  [MAP]    - 地图/关卡设计
  [ART]    - 美术规范
  [AUDIO]  - 音频设计
  [UI]     - UI/UX设计
  [TECH]   - 技术设计
  [REF]    - 参考资料
  [TEMPLATE] - 模板更新

示例：
  [GDD] 新增军团指挥系统设计文档
  [DATA] 调整魏武卒招募成本：800金→650金
  [NARR] 完成白起个人列传初稿
```

### 文档命名规范

```
格式：编号_中文名称_版本号.md

示例：
  110_战斗系统概述_v2.3.md
  210_秦国兵种数据_v1.0.csv
  330_白起对话库_v0.5.md
```

### 版本号规则

```
v0.x  — 草稿阶段，内容不完整
v1.0  — 初版完成，待评审
v1.x  — 评审后修改
v2.0  — 重大修订（需在文档内记录变更日志）
vFINAL — 已锁定，对应游戏内实装版本
```

### 文档状态标记

每份文档头部必须包含：

```yaml
---
id: 110
title: 战斗系统概述
version: 1.3
status: review          # draft | review | approved | implemented | obsolete
author: 策划-张
last_modified: 2026-07-14
reviewers: [程序-李, 美术-王]
related: [111, 112, 113]
game_version: v0.3.0    # 对应的游戏版本
---
```

---

## 工作流程

### 1. 新建文档

```bash
# 复制模板
cp Templates/系统设计模板.md 100_系统设计/xxx_新系统_v0.1.md

# 编辑文档（使用 Markdown 编辑器：VS Code / Obsidian / Typora）
# 完成初稿后提交
git add .
git commit -m "[GDD] 新增XXX系统设计文档 v0.1"
```

### 2. 评审流程

```
策划写初稿 (v0.x)
    ↓
程序评审 "技术上可实现吗？"
美术评审 "资产工作量合理吗？"
    ↓
策划根据反馈修改 (v1.x)
    ↓
制作人/主策最终审批
    ↓
状态改为 approved → 进入开发排期
```

### 3. 与代码/资产的关联

```
策划文档中的每个功能点，对应：
  ├── JIRA/TAPD 任务编号
  ├── UE5 Blueprint/C++ 类名
  ├── UE5 DataTable 行名
  └── 美术资产文件路径

示例：
  魏武卒兵种 → BP_WeiEliteInfantry.uasset
              → DT_SoldierStats.csv (Row: Wei_Elite)
              → /Content/Characters/Soldiers/Wei/SK_Wei_Heavy.fbx
              → JIRA: WG-0147
```

---

## 工具推荐

| 用途 | 工具 | 说明 |
|------|------|------|
| 文档编辑 | VS Code / Obsidian | Markdown 写作，支持双向链接 |
| 图表绘制 | Mermaid / Draw.io | 流程图、系统图嵌入 Markdown |
| 数值表 | Excel / Google Sheets | 导出为 CSV 纳入版本控制 |
| 原型验证 | UE5 Blueprint | 策划可直接搭建逻辑原型 |
| 项目管理 | TAPD / Linear / JIRA | 任务追踪，与文档关联 |
| AI 辅助 | Claude / ChatGPT | 初稿生成、数值校验、对话润色 |

---

## 关键原则

1. **文档即真相** — 游戏怎么做，文档说了算。口头约定不算数
2. **先文档后代码** — 没有审批通过的文档，不开始写代码
3. **数据驱动** — 能用 CSV/DataTable 的数值，不硬编码在文档里
4. **图文并茂** — 每份系统文档至少有一张流程图
5. **AI 辅助，人审定** — AI 生成初稿可以，最终版本必须人审过
6. **保持同步** — 文档版本和游戏版本对应，改了游戏要更新文档
