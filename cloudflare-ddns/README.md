# Cloudflare DDNS for Windows Containers

基于 [favonia/cloudflare-ddns](https://github.com/favonia/cloudflare-ddns/) 的 Windows 容器版本。

## ✨ 功能特点

- 🪟 原生 Windows 容器支持
- 🔄 自动更新 Cloudflare DNS 记录
- 🌐 支持 IPv4 和 IPv6
- 🛡️ 支持 Cloudflare 代理（橙色云）
- 🔔 支持通知服务集成（Healthchecks、Uptime Kuma、Shoutrrr）
- 🎯 支持通配符域名

## 📋 前置要求

- Windows Server 2022 (或 Windows 10/11 with Containers)
- Docker Desktop for Windows (启用 Windows 容器模式)
- Cloudflare API Token

## 🚀 快速开始

### 1. 获取 Cloudflare API Token

1. 访问 [Cloudflare Dashboard](https://dash.cloudflare.com/profile/api-tokens)
2. 点击 "Create Token"
3. 使用 "Edit zone DNS" 模板或创建自定义 Token
4. 需要的权限：
   - Zone.Zone (Read)
   - Zone.DNS (Edit)

### 2. 配置环境变量

复制示例配置文件：

```powershell
Copy-Item .env.example .env
```

编辑 `.env` 文件，填入你的信息：

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=example.com,www.example.com
PROXIED=true
```

### 3. 构建并运行

```powershell
# 构建镜像
docker-compose build

# 启动容器
docker-compose up -d

# 查看日志
docker-compose logs -f cloudflare-ddns
```

## 🎛️ 配置说明

### 必需配置

| 环境变量 | 说明 | 示例 |
|---------|------|------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare API Token | `your_token_here` |
| `DOMAINS` | 要更新的域名列表（逗号分隔） | `example.com,www.example.com` |

### 可选配置

| 环境变量 | 说明 | 默认值 |
|---------|------|--------|
| `PROXIED` | 启用 Cloudflare 代理 | `false` |
| `IP6_PROVIDER` | IPv6 提供商（设为 `none` 禁用） | `cloudflare.trace` |
| `UPDATE_CRON` | 更新频率 | `@every 5m` |
| `TTL` | DNS 记录 TTL（秒） | `1` (自动) |
| `RECORD_COMMENT` | DNS 记录备注 | - |
| `HEALTHCHECKS` | Healthchecks.io URL | - |
| `UPTIMEKUMA` | Uptime Kuma Push URL | - |
| `SHOUTRRR` | Shoutrrr 通知 URL | - |

更多配置选项请参考 [官方文档](https://github.com/favonia/cloudflare-ddns/)。

## 📝 示例配置

### 基础配置 - 单个域名

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=example.com
PROXIED=false
```

### 多域名 + Cloudflare 代理

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=example.com,www.example.com,blog.example.com
PROXIED=true
```

### 仅 IPv4 + 自定义更新频率

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=example.com
IP6_PROVIDER=none
UPDATE_CRON=@every 10m
```

### 通配符域名

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=*.example.com,example.com
PROXIED=true
```

### 集成通知服务

```env
CLOUDFLARE_API_TOKEN=your_token_here
DOMAINS=example.com
HEALTHCHECKS=https://hc-ping.com/your-uuid-here
SHOUTRRR=telegram://token@chatid
```

## 🔧 管理命令

```powershell
# 查看运行状态
docker-compose ps

# 查看日志
docker-compose logs -f

# 重启服务
docker-compose restart

# 停止服务
docker-compose stop

# 停止并删除容器
docker-compose down

# 重新构建并启动
docker-compose up -d --build
```

## 🐛 故障排查

### 检查容器日志

```powershell
docker-compose logs cloudflare-ddns
```

### 进入容器调试

```powershell
docker exec -it cloudflare-ddns cmd
```

### 常见问题

1. **容器无法启动**
   - 确认 Docker 已切换到 Windows 容器模式
   - 检查环境变量是否正确配置

2. **无法更新 DNS 记录**
   - 验证 API Token 权限是否正确
   - 检查域名是否在 Cloudflare 上托管

3. **IPv6 检测失败**
   - 如不需要 IPv6，设置 `IP6_PROVIDER=none`

## 📚 参考资料

- [上游项目](https://github.com/favonia/cloudflare-ddns/)
- [Cloudflare API 文档](https://developers.cloudflare.com/api/)
- [Docker Windows 容器](https://docs.microsoft.com/en-us/virtualization/windowscontainers/)

## 📄 许可证

本项目遵循上游项目的 Apache 2.0 with LLVM exceptions 许可证。
