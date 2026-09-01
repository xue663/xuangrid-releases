# 安装与部署

## 环境要求

- 可访问币安 API 的服务器，建议 2 核 4GB 起步
- 币安 API Key 仅开启交易与只读权限，关闭提现权限，并配置服务器 IP 白名单

## Docker 部署

```bash
mkdir -p xuangrid/{config,data,logs}
cd xuangrid
docker pull jun663/xuangrid:1.0.3
docker run -d --name xuangrid -p 8787:8787 \
  -v ./config:/app/config \
  -v ./data:/app/data \
  -v ./logs:/app/logs \
  -e TZ=Asia/Shanghai \
  jun663/xuangrid:1.0.3
```

首次启动后浏览器打开 `http://<服务器IP>:8787` 完成管理员初始化，并配置币安 API。

### 公开只读演示（仅测试网）

官网演示容器可在确认 `config.yaml` 使用 `environment: testnet` 后增加：

```bash
-e XUANGRID_PUBLIC_DEMO=true
-p 8788:8788
```

演示入口默认使用 `8788` 端口，也可以通过 `XUANGRID_PUBLIC_DEMO_PORT` 或 `api.public_demo_port` 修改。`8787` 仍保留正常登录控制台，两者互不影响。访客在 `8788` 会自动以观察员身份进入，只能读取行情、网格、订单、收益和风险数据。暂停、恢复、参数、用户、交易所凭证、激活与通知配置等写操作均由后端拒绝。主网实例启用该选项会直接拒绝启动。

## Linux 部署（Docker）

Linux 用户统一使用 Docker 镜像部署，公开仓库不提供 Linux 源码运行包：

```bash
mkdir -p xuangrid/{config,data,logs}
cd xuangrid
docker pull jun663/xuangrid:1.0.3
docker run -d --name xuangrid -p 8787:8787 \
  -v ./config:/app/config \
  -v ./data:/app/data \
  -v ./logs:/app/logs \
  -e TZ=Asia/Shanghai \
  jun663/xuangrid:1.0.3
```

需要 systemd 托管时，参考私有仓库 `deploy/` 目录中的服务模板。

## Windows 部署

1. 解压 `xuangrid-<version>-win-x64.zip`
2. 双击 `run.bat`（不要直接双击 exe），首次会自动生成 `config.yaml` 和 API Token
   - Web 初始化页面会自动显示并填入「一次性初始化码」
3. 浏览器打开 `http://127.0.0.1:8787` 完成初始化

启动失败时窗口会暂停并显示错误码，详细日志在 `logs\startup.log`。

首次配置 Binance 凭证时，可在 Web 页面选择「测试网 testnet」或「主网 mainnet」；主网会显示真实资金风险确认提示。

开机自启：下载 [NSSM](https://nssm.cc) 放入 PATH，以管理员身份运行 `install_service.bat`。

## 升级

- Docker：拉取新镜像后重建容器，保留 `config/` 与 `data/` 目录
- Windows：停止服务或关闭窗口，解压新版本，复制旧版本的 `config.yaml` 与 `data/` 目录后重新启动

## 常见问题

### 网页打不开

确认防火墙放行 8787 端口；Docker 部署检查 `docker logs xuangrid`。

### 容器启动报 Permission denied / 无法创建 config.yaml

Docker 镜像以 UID 10001 的非 root 用户运行。使用 `-v ./config:/app/config` 这类目录挂载时，如果宿主目录由 root 创建，容器内用户可能没有写入权限。可执行：

```bash
sudo chown -R 10001:10001 xuangrid/config xuangrid/data xuangrid/logs
docker restart xuangrid
```

新版镜像的启动脚本会自动修正 `/app/config`、`/app/data`、`/app/logs` 的归属，再切换到非 root 用户运行程序。

### 许可证提示未激活或已过期

在官网购买后输入激活码；换设备请先在客户门户解绑旧设备。

### 如何确认安装包未被篡改

每个 Release 附件都附带 `.sha256` 校验文件，下载后核对哈希。
