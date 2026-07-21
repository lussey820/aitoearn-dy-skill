# AiToEarn Trae Skills

AiToEarn 平台的 Trae IDE 技能集合——热点抓取 → AI 生成脚本/图文/视频 → 抖音自动发布，全链路内容创作自动化。

## 技能概览

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

> 图文 skill（SKILL.md）独立运行，不依赖脚本 skill。

## 项目结构

```
├── SKILL-SCRIPT.md       # 脚本创作技能
├── SKILL-VIDEO.md        # 视频生成与发布技能
├── SKILL.md              # 图文生成与发布技能
├── config.example.json   # 配置文件模板
├── aitoearn-api-guide.md # AiToEarn MCP API 参考文档
└── README.md
```

## 快速开始

### 1. 配置

复制 `config.example.json` 为 `config.json`，填入你的 API Key 和平台账号：

```json
{
  "apiKey": "你的_API_Key",
  "accountIds": {
    "douyin": "douyin__你的抖音ID"
  }
}
```

> 如何获取 API Key：[AiToEarn API KEY 教程](https://aitoearn.cn/zh/use/api-key)

### 2. 安装技能

将 `.md` 文件导入 Trae IDE 的技能管理即可使用。

### 3. 使用示例

**生成图文：**
```
帮我生成赵怀真帅气光阴效果的图片发布抖音
做一期今日AI新闻发抖音
```

**生成视频：**
```
先用脚本 skill 生成视频脚本，再用 video-script.json 生成视频发抖音
```

## 核心特性

- **7 步意图解析** — 将"赵怀真帅气光阴"等模糊中文描述拆解为结构化视觉词汇（主体、风格、媒介、空间、时间、感官）
- **风格矛盾检测** — 自动识别 Q版 vs 写实、简约 vs 华丽等冲突组合，问用户而不是猜
- **创意访谈** — 面对模糊需求时主动用画面问题引导用户理清思路
- **实时查价** — 调用前通过 `getDraftGenerationPricing` 获取实时积分，不做硬编码
- **模型自动降级**（视频 skill） — 失败时同 scope 内找替代模型，不直接放弃
- **逐条生成+逐条发布** — 批量模式下每条生成完立即发布，不等全部完成
- **敏感信息隔离** — API Key 和 AccountId 统一在 `config.json` 中管理

## 隐私提醒

- `config.json` 存储 API Key 和账号 ID，已在 `.gitignore` 中排除
- 本仓库不含任何真实密钥
- 使用前请基于 `config.example.json` 创建自己的配置
