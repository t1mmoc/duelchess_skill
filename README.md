# duelchess_skill — 烽决 AI 托管提示词真源

`skill.md` 是发给第三方 AI（OpenAI 兼容接口）的唯一系统提示词：游戏背景、地形与战斗规则、回合三段模型、局势文本格式、行动硬约束与 JSON 输出 schema。

## 消费方式

- 本文件所在仓库作为 **project 级 git submodule** 同时挂进前端（`duelchess`）与未来 Worker（`duelchess_server`）。
- 前端：`prebuild`/`predev` 把 `skill.md` 拷进 `public/duelchess_skill/skill.md`，客户端同域 `GET` 一次并整局缓存（始终作为消息列表首条 system 消息，前缀恒定 → KV cache 稳定命中）。
- Worker（未来）：从自身打包副本直接读取，两端均**不跨服务拉取**。
- 改提示词只改本仓库 → 前端/Worker 各自 CI 重建即生效。

## 约定

- **public 仓库、无密钥**——纯提示词，Pages/Actions 拉 submodule 免凭证。
- AI 输出契约仅 `{ action: { from, to }, reason }`；走法（普通/铁路滑行/传送）由引擎自动判定，AI 不回传 kind/via。
