# GG

关闭项目前的收尾 Skill——自动整理 `CLAUDE.md`，并同步更新 `agentmemory` 长期记忆。

## 这是干什么的

每次结束一段工作、准备关闭项目前，手动整理文档和记忆很容易漏掉、懒得做，久而久之要么 `CLAUDE.md` 一堆信息没归类，要么下次重开项目时 Claude 完全不记得之前踩过的坑。

`GG` 把这套收尾流程固定成一条命令，一次触发做两件事：

1. **整理 `CLAUDE.md`**：把本次会话中零散的规则、流程、约束分类归纳到对应分区，保留原有内容，过时的信息标注"已废弃"而不是直接删除。
2. **同步 `agentmemory`**：把本次做了什么、踩了什么坑、还有什么没做完、做了哪些关键决策，分条存进长期记忆，方便以后 `/recall` 精准搜到。

## 使用前提

- 已安装 [Claude Code](https://claude.com/claude-code)
- 已安装 [agentmemory](https://github.com/rohitg00/agentmemory) 插件，并且本地 memory server 正在运行
```powershell
  curl http://localhost:3111/agentmemory/health
```
  如果没起，先另开一个终端运行：
```powershell
  npx @agentmemory/agentmemory
```

## 安装

```powershell
mkdir "$env:USERPROFILE\.claude\skills\GG" -Force
```

把本目录下的 `SKILL.md` 放进 `C:\Users\<你的用户名>\.claude\skills\GG\SKILL.md`。

安装范围说明：
- 放在 `~/.claude/skills/GG/`（用户目录）→ 所有项目通用
- 放在项目根目录 `.claude/skills/GG/` → 仅当前项目可用，且会随项目一起进版本控制，方便团队共用

## 使用方法

工作告一段落、准备关闭项目前，在 Claude Code 会话里输入：/gg

Claude 会自动读取当前会话上下文、结合历史相关记忆，完成 `CLAUDE.md` 整理和 `agentmemory` 更新，无需额外手动操作。

## 分工原则

| | CLAUDE.md | agentmemory |
|---|---|---|
| 读取方式 | 每次开会话都全文读入 | 按需检索（`/recall`），用到才读 |
| 适合内容 | 长期有效、每次都用得上的硬性规则和约束 | 一次性的踩坑细节、调试过程、具体决策权衡 |

不要把偶发的、具体的调试记录塞进 `CLAUDE.md`，否则文件会越来越臃肿，每次开会话都要多读一堆用不上的内容；这类信息交给 `agentmemory`，需要时按关键词搜就行。

## 常见问题

**执行后发现 agentmemory 里存了重复的记忆？**
Skill 里已经要求先 `/recall` 确认是否已存过再决定要不要 `/remember`，但检索不是 100% 精准，如果发现明显重复，可以手动 `/forget` 清理。

**`/GG` 打不出来，命令没反应？**
检查文件夹路径和文件名是否正确（`SKILL.md` 必须放在 `skills/GG/` 目录下），改动后需要重启 Claude Code 或开一个新会话才会生效。也可以尝试小写 `/gg`。

**CLAUDE.md 里的旧规则被误判"已废弃"了？**
Skill 只会在**本次对话中有明确推翻依据**时才标注废弃，如果发现误判，直接手动改回来即可，不影响下次继续用。
