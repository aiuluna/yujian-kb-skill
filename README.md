# yujian-kb Skill

`yujian-kb` 是隅间企业知识库给 Codex / 业务 Agent 使用的 Knowledge Skill。安装后，Codex 可以通过 Skill 内置轻客户端查询已发布企业知识。

## 安装

在 Codex 中请求安装这个 GitHub Skill：

```text
请安装 GitHub 上的 yujian-kb Skill：
https://github.com/aiuluna/yujian-kb-skill/tree/main/skills/yujian-kb
```

安装完成后重启 Codex，让新 Skill 生效。

也可以手动安装：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb"
cp -R skills/yujian-kb "${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb"
```

## 初始化 Token

向管理员获取低权限 Knowledge Access Token。这个 token 只用于已发布知识问答，不是 yujian-knowledge-hub 后台 token，也不是管理员 token。

```bash
"${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb/scripts/yujian-kb" auth login --token ykh_xxx
"${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb/scripts/yujian-kb" auth status --verify
```

默认服务端是：

```text
https://yujian-kb.xaidev.tech
```

不要配置 `127.0.0.1:8000` 或 `5173`；`5173` 只是知识库平台本地开发用的 Vite 端口。

## 使用

在 Codex 中使用 `$yujian-kb`，或者直接询问企业知识问题。Codex 会按 Skill 说明调用：

```bash
"${CODEX_HOME:-$HOME/.codex}/skills/yujian-kb/scripts/yujian-kb" ask "孕妇顾客怎么处理"
```

Skill 内置脚本只负责调用远程 Knowledge QA Service 并展示服务端 response。它不负责本地总结、改写或补充业务规则。服务端如果返回结构化答案，Codex 应保留结构；服务端如果返回无可信答案，Codex 应保留 no-answer 结果。

## 边界

这个 Skill 不支持 `search`，不支持 MCP，不做本地 gbrain、本地知识库同步、本地索引、本地 embedding、workspace/source 参数、文件上传、Wiki 审核、Wiki 发布、OAuth、device flow 或飞书扫码登录。

不要把 Knowledge Access Token 写入 docs、issue、日志或最终回答，也不要在回答里复述 token。
