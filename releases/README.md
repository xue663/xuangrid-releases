# Releases

每个正式版本在 GitHub Releases 发布以下产物：

- `xuangrid-<version>-win-x64.zip`：Windows x64 安装包
- 对应的 `.sha256` 校验文件

Linux 用户统一使用 Docker Hub 镜像 `jun663/xuangrid`。

`latest.json` 记录当前最新版本信息，供官网下载页使用：

```json
{
  "version": "1.0.2",
  "published_at": "2026-08-31T14:17:16Z",
  "assets": {
    "windows": "xuangrid-1.0.2-win-x64.zip",
    "sha256": "xuangrid-1.0.2-win-x64.zip.sha256"
  }
}
```
