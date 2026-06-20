# hl-skills

> 一套个人 [Claude Code](https://docs.claude.com/en/docs/claude-code) skills 合集，以可安装的插件形式分发。

[English](README.md) | **简体中文**

本仓库是一个 **插件 marketplace**：用一个仓库容纳不断增长的多个插件，每个插件捆绑一个或多个 skill。只需添加一次 marketplace，之后按需安装任意插件。

## 安装

```bash
# 1. 添加本 marketplace
/plugin marketplace add hleidev/hl-skills

# 2. 安装某个插件
/plugin install research@hl-skills
```

## 插件

| 插件 | skill | 调用 | 作用 |
|---|---|---|---|
| `research` | `reporting` | `/research:reporting` | 结构化的对外信息调研，产出一份可决策的报告 |

> `research` 插件默认**中文**输出。想换语言，改其 `SKILL.md` 里的 "Output language" 设置，或调用时直接用你的语言提问即可。

后续新插件会持续加进这同一个仓库——新增插件**永远不需要新建仓库**。

## 仓库结构

```
hl-skills/
├── .claude-plugin/
│   └── marketplace.json          # 注册表：列出下面所有插件
├── plugins/
│   └── research/
│       ├── .claude-plugin/
│       │   └── plugin.json        # 插件清单
│       └── skills/
│           └── reporting/
│               └── SKILL.md       # skill 本体
└── README.md
```

## 许可

[MIT](LICENSE)。
