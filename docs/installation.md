# 安装与升级

当前稳定版 **1.4.5**。Windows 使用 x64 安装包；Linux、飞牛及其他 NAS 使用 Docker，镜像支持 **Linux amd64 / x86_64**。ARM 设备没有原生镜像。

## Docker / 飞牛 / Linux

准备 Docker 和三个持久化目录，执行：

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

浏览器打开 `http://<服务器或NAS的局域网IP>:8787`。命令发布到宿主机网卡，请通过可信局域网、VPN 或受保护的 HTTPS 入口访问，勿直接开放到公网。

飞牛等 NAS 使用容器管理界面时，填写相同的镜像、端口和三个目录映射。宿主目录可自行选择，升级必须继续挂载原目录；不要把容器内 `/app/data` 当成宿主目录。新建实例时检查宿主端口 8787 是否已被占用。

首次启动自动生成 `config/config.yaml` 和 API 令牌；网页管理员账号由你创建，API 令牌不是管理员密码或订阅激活码。启动脚本会修正持久化目录权限，再以 UID 10001 运行。受 NAS 权限限制时，为该 UID 授予读写权限，不要清空目录来排障。

```bash
docker ps --filter name=xuangrid
docker logs --tail 100 xuangrid
```

## Windows x64

1. 从 [1.4.5 Release](https://github.com/xue663/xuangrid-releases/releases/tag/v1.4.5) 下载 ZIP 和同名 `.sha256` 文件。
2. 在下载目录打开 PowerShell，核对以下哈希与校验文件第一段相同：

```powershell
Get-FileHash .\xuangrid-1.4.5-win-x64.zip -Algorithm SHA256
Get-Content .\xuangrid-1.4.5-win-x64.zip.sha256
```

3. 解压到独立目录，进入包含 `run.bat` 的最内层目录，双击它；不要直接运行 EXE。
4. 保持控制台窗口开启，访问 `http://127.0.0.1:8787`。页面暂不可用时，等程序启动后刷新。

启动失败查看 `logs\startup.log`。需要服务托管时，按安装包说明准备 NSSM，再以管理员身份运行 `install_service.bat`。

## 第一次打开：按六步引导完成

1. **创建管理员**：保存账号和密码；一次性初始化码用于创建管理员，页面通常会自动填入。
2. **激活或试用**：输入订阅激活码，或按页面提示申请设备试用。
3. **连接账户**：首次建议测试网，填写对应环境的币安合约 API；仅开启读取和合约交易权限，关闭提现权限并配置出口 IP 白名单。
4. **配置策略**：选择交易对、网格方向和参数。
5. **资金与风险预演**：阅读检查结果，处理页面列出的阻塞。
6. **确认启动**：输入交易对并确认。仅完成安装或激活不会自动开始交易。

测试网与主网 API 不通用。首次主网连接会验证凭据并读取合约钱包余额；余额为零无法完成设置。首次启动要求账户空仓、无挂单，请使用符合要求的账户，不要为了通过检查擅自删除已有保护单。

## Docker 升级到 1.4.5

1. 在 Web 暂停策略，确认命令完成、没有待处理的参数应用，核对仓位和真实平仓保护单。
2. **停止容器后再备份**，使数据库与相关文件保持一致：

```bash
docker stop -t 60 xuangrid
umask 077
tar -C "$HOME/xuangrid" -czf "$HOME/xuangrid-backup-$(date +%Y%m%d-%H%M%S).tar.gz" config data logs
docker pull jun663/xuangrid:1.4.5
docker rm xuangrid
```

3. 使用安装段落的 `docker run` 命令重建，保持原目录、端口和自定义环境变量。路径与示例不同的实例，备份和重建都使用自己的实际路径。
4. 核对 Web 版本、授权、仓位、订单和保护状态。**已暂停的策略仍保持暂停**；确认风控中心没有阻塞后再手动恢复。

固定版本便于确认安装版本；`latest` 会随后续正式发布变化。使用 Compose 的实例见 [Docker Hub 说明](../DOCKERHUB.md)。不要删除持久化目录或使用 `docker compose down -v` 清除数据。

## Windows 升级

先在 Web 暂停并核对保护单，再停止程序或服务；备份 `config.yaml`、完整 `data/` 和 `logs/`，如使用自定义 `.env` 也须保留。解压新包到新目录，回填原配置与完整数据后启动。服务托管的实例还需更新服务的程序路径和工作目录。

保留数据也会保留授权设备身份，常规升级无需解绑、重新创建管理员或重新启动首次引导。启动后核对版本、授权及保护单，手动恢复前处理风控阻塞。

## 出现问题

- **网页打不开**：检查程序或容器状态、日志、宿主端口冲突和局域网防火墙；Docker 使用服务器 IP，Windows 本机使用 127.0.0.1。
- **保证金核查一直等待、恢复失败**：按 [暂停与保证金核查指南](operations.md) 处理，不要反复恢复或重复划转。
- **升级后授权异常**：先检查是否挂载原 `data/`，再参考 [激活与迁移](activation.md)，不要先删除授权数据。
