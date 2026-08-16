<p align="center">
  <img src="docs/brand/logo.svg" width="96" alt="XuanGrid" />
</p>

<h1 align="center">XuanGrid</h1>

<p align="center">面向 Binance USD-M Futures 的智能网格交易系统<br/>ATR 自适应间距 · 分层风控 · 方向网格 · 订阅授权</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-7.1.1-0ea5e9" alt="version" />
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20Windows-6366f1" alt="platform" />
  <img src="https://img.shields.io/badge/exchange-Binance%20USD--M-f59e0b" alt="exchange" />
  <img src="https://img.shields.io/badge/license-proprietary-ef4444" alt="license" />
</p>

本仓库是 XuanGrid 的**发布通道**，提供 Windows 安装包、Linux 运行包、Docker 镜像与使用文档。软件为专有授权产品：下载免费，完整功能需要使用激活码激活。

---

## 快速开始

<table>
  <tr>
    <td><b>Docker</b><br/><br/>适合 Linux 服务器<br/><br/><code>docker pull ghcr.io/xue663/grid-v7:7.1.1</code><br/><code>docker compose up -d</code></td>
    <td><b>Windows</b><br/><br/>解压即用，支持开机自启<br/><br/><code>run.bat</code> 启动<br/><code>install_service.bat</code> 安装服务</td>
  </tr>
</table>

启动后浏览器打开 `http://<服务器>:8787`，完成初始化并配置币安 API。

### Docker 首次运行

```bash
mkdir -p ~/grid-v7/config ~/grid-v7/data
docker run -d --name grid-v7 --restart unless-stopped \
  -p 8787:8787 \
  -v ~/grid-v7/config:/app/config \
  -v ~/grid-v7/data:/app/data \
  ghcr.io/xue663/grid-v7:7.1.1
```

首次启动会自动从示例生成 `/app/config/config.yaml`（默认测试网），随后在浏览器完成初始化并配置币安 API；`data/` 目录保存数据库、凭证与许可证，升级时只需替换镜像、保留目录。

---

## 购买与激活

1. 前往 [购买页面](https://www.1990663.xyz/buy/) 选择套餐并支付
2. USDT TRC20 到账后自动发放激活码
3. 在网格 Web「订阅与授权」输入激活码完成激活

同邮箱续费会自动叠加时长；换设备请在 [客户门户](https://www.1990663.xyz/portal/) 先解绑旧设备。

---

## 文档

- [安装与部署](docs/installation.md)
- [激活与设备迁移](docs/activation.md)
- [发布产物说明](releases/README.md)

---

## 功能特性

<table>
  <tr>
    <td><b>ATR 自适应网格</b><br/>根据波动自动调整间距</td>
    <td><b>方向网格</b><br/>中性 / 做多 / 做空三种模式</td>
    <td><b>分层风控</b><br/>强平距离、容量、压力损失预警</td>
  </tr>
  <tr>
    <td><b>持仓重建</b><br/>受控减仓、均价保护、双向激活</td>
    <td><b>收益分析</b><br/>收益日历、统一账本、年化收益率</td>
    <td><b>多通道通知</b><br/>飞书 / 钉钉 / Telegram</td>
  </tr>
</table>

---

## 授权说明

XuanGrid 是专有软件，保留所有权利。未经授权不得分发、反编译或用于商业用途。每个激活码默认绑定 1 台设备，换设备前必须在客户门户解绑。

---

<p align="center">
  <a href="https://www.1990663.xyz/">官网</a> ·
  <a href="https://www.1990663.xyz/buy/">购买订阅</a> ·
  <a href="https://www.1990663.xyz/portal/">客户门户</a>
</p>
