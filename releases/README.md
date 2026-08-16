# Releases

每个正式版本在 GitHub Releases 发布以下产物：

- `grid-v7-<version>-win-x64.zip`：Windows x64 安装包
- 对应的 `.sha256` 校验文件

Linux 用户统一使用 Docker 镜像，镜像同步发布到 `ghcr.io/xue663/grid-v7`。

`latest.json` 记录当前最新版本信息，供官网下载页使用：

```json
{
  "version": "7.1.1",
  "published_at": "2026-08-16T00:00:00Z",
  "assets": {
    "windows": "grid-v7-7.1.1-win-x64.zip",
    "sha256": "grid-v7-7.1.1-win-x64.zip.sha256"
  }
}
```
