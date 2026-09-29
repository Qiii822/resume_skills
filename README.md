# Resume Tailor · 定制化简历生成 Skill

一个遵循开放 **Agent Skills 格式** 的 AI Skill：根据你的个人资料和目标岗位 JD，自动生成一份高度定制化的中文/英文简历（Markdown），并附带透明的「定制说明」，交代它做了什么取舍、哪里需要你补数据。

> 不绑定 Claude Code —— 任何支持 Agent Skills 格式的 agent 都能加载。仅需一项宿主能力：联网搜索（可选，缺了会降级并如实标注）。

## 特性

- 🎯 **JD 拆解**：提取硬性技能（区分必须/加分）、核心职责关键词，并从 JD 措辞反推团队文化
- 🧭 **经历匹配打分**：每段经历与 JD 逐条比对（高/中/低），保留所有不减分的经历——相关度决定展开 vs 压缩，低相关压缩而非舍弃，让简历更饱满
- ✍️ **STAR/CAR 重写**：每条经历压成一句话，术语向 JD 靠拢，语气随公司文化调整
- 📄 **篇幅控制**：内容密度控制，目标 1 页、上限 2 页
- 📋 **定制说明**：每次生成都附带「保留/舍弃了什么、需补什么数据、文化判断依据」
- 🔒 **零编造**：绝不杜撰你未提供的数字、项目名、公司名、技能

## 文件结构

```
resume-tailor/
├── SKILL.md                          # 主 Skill（5 步流程 + 禁止事项 + 边界处理）
├── profile-template.md               # 个人资料模板（首次填写，存为 profile.md 复用）
├── reference/
│   ├── style-guide.md                # 公司类型 → 简历风格倾向对照
│   ├── scoring-pitfalls.md           # 经历匹配打分的常见误区（高估/低估）
│   ├── jd-extraction-anti-patterns.md# JD 关键词提取的反模式（过度解读/遗漏）
│   ├── byte-dance-ai-pm-preferences.md # 字节 AI 产品经理招聘偏好（7 条）及落法
│   └── case-study.md                 # 脱敏示例（改前 → 改后 → 定制说明）
├── LICENSE                            # MIT
└── README.md
```

## 安装

### Claude Code

**个人级**（所有会话可用）：

```bash
cp -R resume-tailor ~/.claude/skills/
```

**项目级**（仅某个项目内可用）：

```bash
cp -R resume-tailor <项目根目录>/.claude/skills/
```

安装后**重启 Claude Code**（或新开一个会话）即可识别。

### 其他 Agent

本 Skill 遵循开放 Agent Skills 格式（`SKILL.md` + frontmatter），可被其他支持该格式的 agent 加载。
**使用方式因 Agent 而异**：把 `resume-tailor/` 目录放进你所用 Agent 约定的 skills 目录即可，
具体路径与触发方式详见该 Agent 的文档（如 Claude.ai，以及支持 Skills 的开源框架）。

## 使用

1. **填资料**：按 `profile-template.md` 填好，保存为 `profile.md`（一次填好，换 JD 复用，无需重复输入）。
2. **触发**：输入 `/resume-tailor`，或直接说「帮我针对这个岗位定制简历」。
3. **给材料**：

   ```
   我的 profile：<粘贴 profile.md 内容，或给路径>
   目标岗位 JD：<粘贴 JD 原文>
   招聘公司：<公司名>
   ```

随后按 5 步自动执行，输出「简历正文 + `# 定制说明`」。

## 处理流程

| 步骤 | 做什么 |
|---|---|
| Step 1 | JD 拆解：硬性技能（必须/加分）、职责关键词、文化倾向；缺文化信息时 web search 检索公司 |
| Step 2 | 经历匹配打分（高/中/低），保留所有不减分的经历——低相关压缩而非舍弃；缺量化数字用 `[需补：…]` 占位，不编造 |
| Step 3 | STAR/CAR 重写成一条要点，术语向 JD 靠拢，语气随文化调整（已贴近则只微调） |
| Step 4 | 篇幅控制：内容密度控制，末尾一行注明预计页数 |
| Step 5 | 输出简历正文 + `# 定制说明`（取舍 / 待核实 / 文化依据，标注来源） |

## 硬性约束

- ❌ 不编造任何用户未提供的数字、项目名、公司名、技能
- ❌ 不为匹配 JD 堆砌用户不具备的技能
- ❌ 不省略「定制说明」部分
- ❌ 信息不全时先追问，不假设着往下写

## License

[MIT](./LICENSE)
