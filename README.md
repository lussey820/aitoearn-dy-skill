# AiToEarn Trae Skills

让 [AiToEarn](https://aitoearn.cn) 平台的内容创作在 [Trae IDE](https://trae.ai)（字节跳动 AI IDE）中实现端到端自动化：热点抓取 → AI 生成脚本/图文/视频 → 抖音自动发布。

## 这是什么

三个 Trae 技能文件（`.md`），每个技能定义了一套完整的 AI 工作流：

| 技能 | 文件 | 做什么 |
|------|------|--------|
| 脚本创作 | `SKILL-SCRIPT.md` | 热点抓取蒸馏 / 创意访谈 → 生成结构化分镜脚本 JSON |
| 视频生成 | `SKILL-VIDEO.md` | 加载脚本 JSON → AI 生成视频 → 自动发布抖音 |
| 图文发布 | `SKILL.md` | 热点/创意 → 7步意图解析 → AI 生成图文 → 自动发布抖音 |

### 上下游关系

```
SKILL-SCRIPT.md（上游）
    │  抓取热点 / 创意访谈
    │  产出：结构化分镜 JSON
    ▼
SKILL-VIDEO.md（下游）
    │  加载 JSON → 填入视频 Prompt
    │  生成视频 → 发布抖音
```

> 图文 skill（`SKILL.md`）独立运行，不依赖脚本 skill。

## 前置条件

- [AiToEarn](https://aitoearn.cn) 账号（需有可用积分）
- AiToEarn API Key（[获取教程](https://aitoearn.cn/zh/use/api-key)）
- [Trae IDE](https://trae.ai) 桌面版
- 已绑定的抖音账号

## 安装

### 1. 克隆仓库

```bash
git clone https://github.com/你的用户名/仓库名.git
```

### 2. 配置

复制 `config.example.json` 为 `config.json`，填入你的 API Key 和平台账号：

```json
{
  "apiKey": "你的_API_Key",
  "accountIds": {
    "douyin": "douyin__你的抖音ID"
  }
}
```

`config.json` 需要放在与对应 skill 文件相同的目录下（Trae IDE 的技能文件夹中）。

### 3. 导入技能

将 `.md` 文件复制到 Trae IDE 的技能目录：

```
C:\Users\你的用户名\.trae-cn\skills\
```

或者通过 Trae IDE 的技能管理界面手动导入。每个技能需要一个独立文件夹，`config.json` 与 `SKILL.md` 放在同一文件夹内。

## 使用示例

在 Trae IDE 对话中直接输入：

**生成图文：**
```
帮我生成赵怀真帅气光阴效果的图片发布抖音
做一期今日AI新闻发抖音
```

**生成视频：**
```
先用脚本 skill 写一个关于「AI改变生活」的15秒视频脚本，
再用生成的 JSON 生成视频发抖音
```

## 项目结构

```
├── SKILL-SCRIPT.md       # 脚本创作技能
├── SKILL-VIDEO.md        # 视频生成与发布技能
├── SKILL.md              # 图文生成与发布技能
├── config.example.json   # 配置文件模板（不含真实密钥）
├── aitoearn-api-guide.md # AiToEarn MCP API 参考文档
└── README.md
```

## 与 AI 助手内置技能的区别

| | 通用 AI 内置技能 | 本项目的 Skill |
|------|------|------|
| 角色 | 无预设人设，机械执行指令 | 每个 skill 有明确角色（视觉产品经理 / 视频制作人），**像懂行的朋友一样协作** |
| 模糊需求 | 直接填入 prompt，效果随机 | **主动追问、创意访谈**，帮用户把"帅""好看"翻译成光线和构图 |
| Prompt 质量 | 中文直译英文，丢失细节 | **7 步意图解析**，拆解主体/风格/媒介/空间/时间/感官再组装 |
| 风格冲突 | 不检测，生成结果可能矛盾 | **矛盾检测矩阵**，Q版 vs 写实等问题先问用户再决定 |
| 错误处理 | 返回错误码，用户自己解决 | **自动重试 + 模型降级**，同 scope 内找替代模型，不让用户面对 503 |
| 积分管理 | 不知道价格 | 调用前**实时查价**，余额不足直接告知差额 |
| 批量任务 | 一次失败全部中断 | **逐条生成+逐条发布**，临时故障跳过继续，结束时统一汇报 |
| 平台适配 | 不知道平台差异 | 抖音 9:16 竖版、标题句式库、发布后三种终态判断 |

## 核心特性

- **7 步意图解析** — 将"赵怀真帅气光阴"等模糊中文描述拆解为结构化视觉词汇（主体、风格、媒介、空间、时间、感官）
- **风格矛盾检测** — 自动识别 Q版 vs 写实、简约 vs 华丽等冲突组合，询问用户而非猜测
- **创意访谈** — 面对模糊需求时主动用画面问题引导用户理清思路
- **实时查价** — 调用前通过 `getDraftGenerationPricing` 获取实时积分，不做硬编码
- **模型自动降级**（视频 skill） — 失败时同 scope 内查找替代模型，不直接放弃
- **逐条生成+逐条发布** — 批量模式下每条生成完立即发布，不等全部完成
- **敏感信息隔离** — API Key 和 AccountId 统一在 `config.json` 管理，已加入 `.gitignore`

## 隐私提醒

- `config.json` 包含你的 API Key 和账号 ID，已通过 `.gitignore` 排除
- 本仓库不含任何真实密钥，所有示例均为占位符
- 使用前基于 `config.example.json` 创建自己的 `config.json`
