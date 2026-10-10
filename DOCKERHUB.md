# XuanGrid

面向 Binance U 本位永续合约的网格交易工具：ATR 自适应间距、中性 / 做多 / 做空网格、策略预演、分层风控、收益分析和飞书 / 钉钉 / Telegram 通知。

当前稳定版 **1.4.5**，镜像平台 **Linux amd64 / x86_64**，适合 Linux 服务器、飞牛及兼容的 NAS。ARM 设备没有原生镜像。首次请使用测试网验证，交易不承诺收益。

## 资金由你掌控，API 权限由你授权

**XuanGrid 不托管你的交易资金。** 资金保留在你自己的币安账户中，策略通过 API 执行交易，无需将交易本金转入 XuanGrid 或第三方账户。

**不提供提现或向外部钱包转账功能。** 创建 API 时，只开启所需的读取与合约交易权限，关闭提现和不需要的划转权限。提现权限在币安端关闭后，该 API 无法通过提现接口转出资金。逐仓保证金追加或释放，是你账户内的仓位保证金调整。

**API 密钥保存在你部署的实例中。** 交易凭证保存在你部署的 Docker 实例的数据目录中，订阅激活与授权校验不需要提交交易所 API Key 或 Secret。保存后，设置页面仅显示遮掩后的 Key，不回显 Secret。

**为 API 配置运行设备的出口 IP 白名单。** 限制其他 IP 使用该密钥，保护好服务器、NAS 和管理账户，不要分享 Secret、凭证文件或带有密钥的截图。

## 快速安装

```bash
mkdir -p ~/xuangrid/config ~/xuangrid/data ~/xuangrid/logs
docker pull jun663/xuangrid:1.4.5
docker run -d --name xuangrid --restart unless-stopped \
  -p 8787:8787 \
  -e TZ=Asia/Shanghai \
  -v ~/xuangrid/config:/app/config \
  -v ~/xuangrid/data:/app/data \
  -v ~/xuangrid/logs:/app/logs \
  jun663/xuangrid:1.4.5
```

访问 `http://<服务器或NAS的局域网IP>:8787`，通过可信局域网、VPN 或受保护的 HTTPS 入口使用，勿直接开放到公网。

按六步引导完成：**创建管理员 → 激活或试用 → 连接账户 → 配置策略 → 资金与风险预演 → 确认启动**。测试网与主网使用各自的 API；关闭提现权限，配置出口 IP 白名单。仅安装或激活不会开始交易。

NAS 容器界面填写相同镜像、端口和三个目录映射。首次自动生成配置与 API 令牌；管理员密码由你设置，API 令牌不是管理员密码或激活码。启动脚本修正目录归属后以 UID 10001 运行；NAS 限制权限时为该 UID 授予读写权限。

## Docker Compose

把以下内容保存为 `compose.yaml`，在同一目录运行 `docker compose up -d`：

```yaml
services:
  xuangrid:
    image: jun663/xuangrid:1.4.5
    container_name: xuangrid
    restart: unless-stopped
    ports:
      - "8787:8787"
    environment:
      TZ: Asia/Shanghai
    volumes:
      - ./config:/app/config
      - ./data:/app/data
      - ./logs:/app/logs
```

固定版本便于核对部署版本；`latest` 会随后续正式发布变化。

## 升级与备份

先在 Web 暂停并确认命令完成，核对仓位与真实平仓保护单；**停止容器后再备份完整配置、数据和日志**。不要删除持久化目录。Docker 命令实例按 [完整升级步骤](https://github.com/xue663/xuangrid-releases/blob/main/docs/installation.md) 重建并保留原目录、端口与环境变量。

Compose 实例在 `compose.yaml` 所在目录停止，备份同目录 `config/`、`data/`、`logs/`，将镜像版本改为目标版本后拉取并启动：

```bash
docker compose stop -t 60
# 此时完成配置、数据和日志备份，再继续以下命令
docker compose pull
docker compose up -d
```

启动后核对版本、授权、仓位、订单和保护状态。常规升级无需解绑设备；**已暂停策略保持暂停**，处理风控阻塞后再手动恢复。不要使用 `docker compose down -v` 清除数据。

## 1.4.5：核查与提醒更清楚

保证金结果不明时系统后台核查，符合条件的临时保护可自动解除，保留用户手动暂停。旧疑难记录按“检查当前账户 → 管理员密码确认”处理，系统保存依据；处理后仍保持暂停，不重发原划转。运行通知改为简短中文并减少无操作价值的观察消息。

[暂停与保证金核查指南](https://github.com/xue663/xuangrid-releases/blob/main/docs/operations.md)

## 检查与帮助

```bash
docker ps --filter name=xuangrid
docker logs --tail 100 xuangrid
```

网页打不开时检查容器日志、端口冲突及局域网防火墙；升级后授权异常时先确认挂载原数据目录。

- [Windows 下载与校验文件](https://github.com/xue663/xuangrid-releases/releases/latest)
- [安装与升级](https://github.com/xue663/xuangrid-releases/blob/main/docs/installation.md)
- [官网](https://www.1990663.xyz/) · [只读演示](https://demo.1990663.xyz/)
- [新购订阅](https://www.1990663.xyz/buy/) · [客户门户续费与迁移](https://www.1990663.xyz/portal/)
