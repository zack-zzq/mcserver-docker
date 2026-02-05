# Minecraft Server Docker

使用 Docker Compose 快速部署 Minecraft 模组服务器的配置项目。

## 项目简介

本项目基于 [itzg/minecraft-server](https://github.com/itzg/docker-minecraft-server) Docker 镜像，提供了一套完整的 Minecraft 模组服务器部署方案。支持从 CurseForge 自动下载和安装模组包。

## 功能特性

- 🚀 使用 Docker Compose 一键部署
- 📦 支持 CurseForge 模组包自动下载安装
- ⚙️ 灵活的环境变量配置
- 🔧 客户端专用模组自动过滤
- 🔒 支持白名单和 RCON 管理
- 💾 数据持久化存储

## 目录结构

```
.
├── config/                 # 配置文件目录
│   └── cf-exclude-include.json  # CurseForge 模组排除/包含配置
├── data/                   # 服务器数据目录（每个模组包独立存储）
├── downloads/              # 下载缓存目录
├── modpacks/               # 本地模组包 ZIP 文件存放目录
├── docker-compose.yml      # Docker Compose 配置文件
├── .env-example            # 环境变量示例文件
└── LICENSE                 # Apache 2.0 许可证
```

## 快速开始

### 前置要求

- Docker
- Docker Compose
- CurseForge API Key（用于下载模组）

### 安装步骤

1. **克隆仓库**
   ```bash
   git clone https://github.com/zack-zzq/mcserver-docker.git
   cd mcserver-docker
   ```

2. **配置环境变量**
   ```bash
   cp .env-example .env
   ```
   编辑 `.env` 文件，填入必要的配置：
   ```env
   CF_API_KEY="your_curseforge_api_key"
   RCON_PASSWORD="your_rcon_password"
   WHITELIST_USERS="player1,player2"
   
   PACK_SLUG="modpack-slug"
   MODPACK_ZIP="/modpacks/your-modpack.zip"
   JAVA_VERSION="17"
   ```

3. **安装模组包**
   ```bash
   docker compose --profile install up
   ```
   此命令会下载并安装模组包，完成后容器会自动退出。

4. **启动服务器**
   ```bash
   docker compose --profile run up -d
   ```

### 停止服务器

```bash
docker compose --profile run down
```

## 配置说明

### 环境变量

| 变量名 | 说明 | 示例 |
|--------|------|------|
| `CF_API_KEY` | CurseForge API 密钥 | `$2a$...` |
| `RCON_PASSWORD` | RCON 远程管理密码 | `your_password` |
| `WHITELIST_USERS` | 白名单玩家列表（逗号分隔） | `player1,player2` |
| `PACK_SLUG` | 模组包标识符（用于数据目录命名） | `cti` |
| `MODPACK_ZIP` | 本地模组包 ZIP 文件路径 | `/modpacks/pack.zip` |
| `JAVA_VERSION` | Java 版本 | `17` / `21` |

### 服务器配置

`docker-compose.yml` 中预设了以下服务器配置：

- **时区**: Asia/Shanghai
- **最大玩家数**: 5
- **难度**: 困难 (hard)
- **PVP**: 关闭
- **允许飞行**: 开启
- **白名单**: 开启

如需修改，请编辑 `docker-compose.yml` 文件中的 `x-common-environment` 部分。

### 模组过滤

`config/cf-exclude-include.json` 文件配置了需要排除的客户端专用模组（如性能优化、界面美化等），确保服务器端只安装必要的模组。

可以通过以下方式自定义：
- `globalExcludes`: 全局排除的模组列表
- `modpacks.<pack-slug>.excludes`: 特定模组包的额外排除
- `modpacks.<pack-slug>.forceIncludes`: 强制包含的模组（覆盖全局排除）

## 端口

| 端口 | 用途 |
|------|------|
| 25565 | Minecraft 游戏端口 |
| 19565 | 额外端口（可用于 Prometheus 监控等） |

## 内存配置

- **安装阶段**: 4GB
- **运行阶段**: 16GB

如需调整，请修改 `docker-compose.yml` 中对应服务的 `MEMORY` 环境变量。

## 许可证

本项目采用 [Apache License 2.0](LICENSE) 许可证。

## 致谢

- [itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server) - 优秀的 Minecraft 服务器 Docker 镜像
