<p align="center">
  <img src="docs/brand/logo.svg" width="88" alt="XuanGrid" />
</p>

<h1 align="center">XuanGrid</h1>

<p align="center">
  面向 Binance USD-M Futures 的智能网格交易系统<br/>
  ATR 自适应间距 · 中性 / 做多 / 做空网格 · 分层风控 · 订阅授权
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-7.1.7-00b4d8" alt="version" />
  <img src="https://img.shields.io/badge/platform-Linux%20%7C%20Windows-6366f1" alt="platform" />
  <img src="https://img.shields.io/badge/exchange-Binance%20USD--M-f59e0b" alt="exchange" />
  <img src="https://img.shields.io/badge/license-proprietary-ef4444" alt="license" />
</p>

本仓库是 XuanGrid 的发布通道，提供 Windows 安装包、Linux 运行包、Docker 镜像与使用文档。软件为专有授权产品：下载免费，完整功能需要使用激活码激活。

---

## 快速开始

### Docker

```bash
mkdir -p ~/xuangrid/config ~/xuangrid/data
docker run -d --name xuangrid --restart unless-stopped \
  -p 8787:8787 \
  -v ~/xuangrid/config:/app/config \
  -v ~/xuangrid/data:/app/data \
  ghcr.io/xue663/xuangrid:7.1.7
```

首次启动会自动生成 `config.yaml`（默认测试网），随后在浏览器完成初始化并配置币安 API。`data/` 保存数据库、凭证与许可证，升级时替换镜像并保留目录即可。

### Windows

1. 下载 `grid-v7-7.1.7-win-x64.zip`。
2. 解压后运行 `run.bat`。
3. 浏览器打开 `http://127.0.0.1:8787` 完成初始化。

启动失败时窗口会暂停并显示错误码，详细日志在 `logs\startup.log`。

---

## 购买与激活

1. 前往 [购买页面](https://www.1990663.xyz/buy/) 选择套餐并支付。
2. USDT TRC20 到账后自动发放激活码。
3. 在 XuanGrid Web 的「订阅与授权」输入激活码完成激活。

同邮箱续费会自动叠加时长；换设备请先在 [客户门户](https://www.1990663.xyz/portal/) 解绑旧设备。

---

## 功能特性

| 能力 | 说明 |
| --- | --- |
| ATR 自适应网格 | 根据波动自动调整间距 |
| 方向网格 | 中性 / 做多 / 做空三种模式 |
| 分层风控 | 强平距离、容量、压力损失预警 |
| 持仓重建 | 受控减仓、均价保护、双向激活 |
| 收益分析 | 收益日历、统一账本、年化收益率 |
| 多通道通知 | 飞书 / 钉钉 / Telegram |

---

## 文档

- [安装与部署](docs/installation.md)
- [激活与设备迁移](docs/activation.md)
- [发布产物说明](releases/README.md)

---

## 授权说明

XuanGrid 是专有软件，保留所有权利。未经授权不得分发、反编译或用于商业用途。每个激活码默认绑定 1 台设备，换设备前必须在客户门户解绑。

---

<p align="center">
  <a href="https://www.1990663.xyz/">官网</a> ·
  <a href="https://www.1990663.xyz/buy/">购买订阅</a> ·
  <a href="https://www.1990663.xyz/portal/">客户门户</a>
</p>
