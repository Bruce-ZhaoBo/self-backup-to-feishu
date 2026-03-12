# Self-backup to Feishu

AI 助手状态自动备份到飞书文档 - 让你的 AI 助手拥有"云端记忆"。

## 简介

本技能为 OpenClaw AI 助手提供状态自动备份与恢复能力，支持飞书文档存储。重装系统或换设备后可一键恢复所有记忆和配置。

## 核心功能

### 1. 自动备份

**触发条件：**
- 每日定时备份（建议凌晨3点）
- 掌握新技能时
- 完成自动化任务时
- 添加重要联系人时
- 用户明确要求时

**备份内容：**
| 文件 | 说明 |
|-----|------|
| IDENTITY.md | AI 身份定义 |
| USER.md | 用户偏好配置 |
| SOUL.md | AI 行为准则 |
| MEMORY.md | 长期记忆 |
| .msmtprc | 邮箱发信配置 |
| cron 任务 | 定时任务列表 |

### 2. 手动备份

随时要求备份，增量更新飞书文档，保留历史信息。

### 3. 一键恢复

从飞书读取备份，重建所有状态文件，快速恢复完整记忆。

### 4. 跨设备同步

重装系统后快速恢复，多设备间同步 AI 状态。

## 使用方法

### 备份状态

```
用户：备份一下状态
AI：好的，我来备份当前状态到飞书...
```

### 恢复状态

```
用户：从飞书恢复状态
AI：正在读取飞书备份文档...
     正在重建文件...
     恢复完成！
```

## 安装

### 方法 1：ClawHub 安装

```bash
clawhub install self-backup-to-feishu
```

### 方法 2：从 GitHub 安装

```bash
cd ~/.openclaw/workspace/skills
git clone https://github.com/Bruce-ZhaoBo/self-backup-to-feishu.git
```

### 方法 3：手动安装

1. 下载本仓库
2. 解压到 `~/.openclaw/workspace/skills/self-backup-to-feishu/`
3. 重启 OpenClaw

## 配置

### 1. 创建飞书应用

1. 访问 [飞书开放平台](https://open.feishu.cn/)
2. 创建企业自建应用
3. 获取 `App ID` 和 `App Secret`
4. 开通文档权限（`docx:document`）

### 2. 创建备份文档

1. 在飞书创建一个文档
2. 从 URL 提取文档 token（`/docx/XXX` 中的 XXX）
3. 确保应用有权限访问该文档

### 3. 配置脚本

编辑 `scripts/daily-backup.py`，修改配置：

```python
CONFIG = {
    "feishu_app_id": "YOUR_APP_ID",
    "feishu_app_secret": "YOUR_APP_SECRET",
    "state_backup_doc_token": "YOUR_DOC_TOKEN",
    # ...
}
```

### 4. 设置定时任务

```bash
# 每日凌晨3点自动备份
crontab -e

# 添加以下行
0 3 * * * /usr/bin/python3 ~/.openclaw/workspace/skills/self-backup-to-feishu/scripts/daily-backup.py >> ~/.openclaw/logs/daily-backup.log 2>&1
```

## 文件结构

```
self-backup-to-feishu/
├── SKILL.md                      # OpenClaw 技能说明文件
├── README.md                     # 本文档
├── scripts/
│   ├── daily-backup.py           # 每日自动备份脚本
│   └── manual-backup.py          # 手动/事件触发备份脚本
└── references/
    └── recovery-guide.md         # 恢复流程详细指南
```

## 设计原则

### 增量更新

更新飞书文档时，先读取现有内容，在原有基础上更新，不直接覆盖历史信息。

### 双重备份

建议同时维护：
- **状态备份文档** - 能力、配置、联系人
- **沟通历史备份** - 对话历史、情感连接

### 本地缓存

先保存本地备份文件，会话时再同步到飞书，避免频繁 API 调用。

## 技术要求

- Python 3.6+
- 飞书开放平台应用
- OpenClaw 运行环境

## 许可证

MIT License

## 作者

@Bruce-ZhaoBo

## 相关链接

- [OpenClaw 文档](https://docs.openclaw.ai)
- [ClawHub 技能市场](https://clawhub.com)
- [飞书开放平台](https://open.feishu.cn)
