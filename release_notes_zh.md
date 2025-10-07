[English](https://github.com/KiteMC/VerifyMC/releases/tag/v1.2.3) | 简体中文 | [官方文档](https://kitemc.com/docs/verifymc/)

# VerifyMC v1.2.3 更新日志

## 🔧 Folia 兼容性修复

### 修复的问题

- **玩家踢出崩溃**：修复 `IllegalStateException: Player is already removed from player chunk loader` 错误
- **插件启动失败**：修复 `UnsupportedOperationException` 错误，插件现在可在 Folia 上正常启动

### 解决方案

- 使用 `PlayerLoginEvent` 在登录阶段拦截玩家，避免区块加载冲突
- 自动检测 Folia 环境，禁用不兼容的异步任务（whitelist.json 监控、版本提醒）
