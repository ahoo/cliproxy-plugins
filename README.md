# CLIProxyAPI Plugins Store

CLIProxyAPI 插件仓库,通过 [Plugin Store](https://github.com/router-for-me/CLIProxyAPI) 机制发布/安装。

## 插件列表

| ID | 版本 | 说明 |
|----|------|------|
| `deepseek-harness-session` | 0.1.1 | 将 `X-DeepSeek-Harness-Session-Id` 映射为 `X-Session-ID`,使 deepseek-harness 客户端的 session-affinity 缓存生效 |

## 安装

代理端配置 `plugins.store-sources` 指向本仓库 `registry.json`,然后通过管理 API 安装:

```yaml
plugins:
  enabled: true
  store-sources:
    - "https://raw.githubusercontent.com/ahoo/cliproxy-plugins/main/registry.json"
  configs:
    deepseek-harness-session:
      enabled: true
      priority: 1
```

```
POST /v0/management/plugin-store/deepseek-harness-session/install
```

## 构建插件

```bash
cd examples/plugin/deepseek-harness-session/go
CGO_ENABLED=1 GOOS=linux GOARCH=amd64 go build -buildmode=c-shared \
  -o deepseek-harness-session-v0.1.1.so .
```

## 发布新版本

1. 构建 `.so`,打包 `<id>_<version>_<goos>_<goarch>.zip`(zip 根目录放 `deepseek-harness-session-v<version>.so`)
2. 生成 `checksums.txt`(zip 的 sha256)
3. `gh release create v<version>` 附带 zip + checksums.txt
4. 更新 `registry.json` 中的 `version`