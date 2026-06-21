# yujian-kb Skill

`yujian-kb` 是给 Codex / 业务 Agent 使用的隅间企业知识库 Skill。它让 Codex 查询已审核、已发布的企业知识，并把服务端返回的答案展示给用户。

这个仓库是业务方安装入口。Skill 目录里包含 `SKILL.md` 和内置轻客户端脚本，Codex 会按 Skill 说明调用脚本完成问答访问。

## 安装

在 Codex 中请求安装：

```text
请安装 GitHub 上的 yujian-kb Skill：
https://github.com/aiuluna/yujian-kb-skill/tree/main/skills/yujian-kb
```

安装完成后重启 Codex，让新 Skill 生效。

## 使用

在 Codex 中询问企业知识时，明确让它使用 `$yujian-kb`。

首次使用如果提示未初始化，把管理员发放的 Knowledge Access Token 粘贴给 Codex 完成配置。这个 token 只用于查询已发布知识，不是知识库后台管理员凭证。

## 这个 Skill 做什么

- 面向业务方和员工查询隅间企业知识。
- 只查询已审核、已发布的知识。
- 当知识库没有可信依据时，返回无可信答案。
- 保留服务端返回的结构化答案和来源依据。

## 不做什么

`yujian-kb` 不负责文件上传、Wiki 审核、Wiki 发布、知识库后台管理、本地知识同步、本地索引、gbrain、search 或 MCP。

Codex 的具体执行说明在 `skills/yujian-kb/SKILL.md`。
