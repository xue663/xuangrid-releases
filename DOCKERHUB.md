# XuanGrid

XuanGrid 是面向 Binance USD-M Futures 的智能网格交易系统，提供 ATR 自适应间距、中性 / 做多 / 做空网格、策略预演、分层风控、收益分析和多通道通知。

当前公开稳定版本：`1.0.2`

镜像平台：Linux `amd64`

> 加密资产交易风险较高。首次部署请使用测试网完成参数、挂单、通知和风控验证，再决定是否切换实盘。

## 快速安装

```bash
mkdir -p ~/xuangrid/config ~/xuangrid/data ~/xuangrid/logs

docker run -d \
  --name xuangrid \
  --restart unless-stopped \
  -p 8787:8787 \
  -e TZ=Asia/Shanghai \
  -v ~/xuangrid/config:/app/config \
  -v ~/xuangrid/data:/app/data \
  -v ~/xuangrid/logs:/app/logs \
  jun663/xuangrid:1.0.2
```

启动后访问：`http://服务器IP:8787`

首次启动会自动生成：

- `config/config.yaml`：系统及交易所配置，默认使用测试网。
- `config/grid-token.txt`：本机 Web 管理凭据，请妥善保管。
- `data/`：数据库、授权和运行状态。
- `logs/`：运行日志。

查看运行状态与日志：

```bash
docker ps --filter name=xuangrid
docker logs -f --tail 200 xuangrid
```

## Docker Compose

```yaml
services:
  xuangrid:
    image: jun663/xuangrid:1.0.2
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

启动：

```bash
docker compose up -d
docker compose logs -f --tail 200
```

## 升级

升级前先在 Web 页面暂停新增风险，并备份 `config/`、`data/` 和 `logs/`。然后执行：

```bash
docker pull jun663/xuangrid:1.0.2
docker rm -f xuangrid
```

再使用原来的端口、环境变量和挂载目录重新创建容器。不要删除或更换原有挂载目录；启动后应核对版本、持仓、挂单和保护状态。

## 镜像与下载

- Docker Hub：`jun663/xuangrid:1.0.2`
- Windows 与校验文件：[GitHub Releases](https://github.com/xue663/xuangrid-releases/releases/tag/v1.0.2)

## 相关链接

- [XuanGrid 官网](https://www.1990663.xyz/)
- [在线演示](https://demo.1990663.xyz/)
- [购买订阅](https://www.1990663.xyz/buy/)
- [客户门户](https://www.1990663.xyz/portal/)
- [安装与使用文档](https://github.com/xue663/xuangrid-releases/blob/main/docs/usage.md)
