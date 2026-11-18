# tk素材工坊 · 更新仓库

这个仓库只放**更新清单** `update.json` 和发行包（Release 资产），不放源码。

客户端启动时会读：

```
https://raw.githubusercontent.com/chenquaerere/tk素材工坊/main/update.json
```

清单字段：

| 字段 | 说明 |
| --- | --- |
| `version` | 最新版本号，如 `1.9.7`（客户端按点分段比较，`1.9.10 > 1.9.9`） |
| `notes` | 更新说明，纯文本，多行用 `\n` |
| `page_url` | 发布页地址（弹窗「去下载」跳这里） |
| `asset_url` | 安装包直链（Release 资产） |
| `sha256` / `size` | 安装包校验值与字节数 |
| `min_version` | 低于此版本建议强制更新（预留，当前只提示） |

## 发版流程

1. 构建新版本覆盖包：`python scripts/build_exe.py --release --offline` → `scripts/make_patch_zip.py`
2. 生成清单：
   ```
   python scripts/make_update_manifest.py --version 1.9.7 ^
     --zip "D:\tiktok去水印\发布\TK素材工坊-覆盖包-v1.9.7-2026xxxx.zip" ^
     --notes "1. 修复…\n2. 优化…"
   ```
3. 提交清单：`git add update.json && git commit -m "release: v1.9.7" && git push`
4. 建 Release 并上传安装包（名字必须和清单里的 `asset_name` 一致）：
   ```
   gh release create v1.9.7 "D:\tiktok去水印\发布\TK素材工坊-覆盖包-v1.9.7-2026xxxx.zip" -t "v1.9.7" -n "…"
   ```

> 仓库必须是 **public**：客户端未登录时也要能读 raw 文件。
