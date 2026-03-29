# openclaw-bin

为桌面应用提供的预构建、平台特定的 [openclaw](https://github.com/openclaw/openclaw) tarball。

## 这是什么？

openclaw 以源码包的形式发布在 npm 上。通过 `npm install` 安装需要解析依赖、编译原生模块，耗时较长。本仓库提供预构建的 tarball，所有依赖已预装，桌面应用可以直接下载解压使用，无需用户机器上安装 npm 或 Node.js。

每个 tarball 包含：
- `openclaw.mjs`（入口文件）
- `dist/`（预构建的 JS 产物）
- `node_modules/`（所有依赖，包括预编译的原生模块）
- `assets/`、`skills/`、`dist-runtime/`

## 下载

Tarball 作为 GitHub Release 资源发布，每个 release tag 对应 openclaw 版本号。

| 平台 | 架构 | 文件名 |
|------|------|--------|
| macOS | Apple Silicon | `openclaw-{version}-darwin-arm64.tar.gz` |
| macOS | Intel | `openclaw-{version}-darwin-x64.tar.gz` |
| Linux | x64 | `openclaw-{version}-linux-x64.tar.gz` |
| Windows | x64 | `openclaw-{version}-win32-x64.tar.gz` |

## 使用方法

```bash
# 下载并解压
curl -L https://github.com/boguan/openclaw-bin/releases/download/v2026.3.28/openclaw-2026.3.28-darwin-arm64.tar.gz | tar -xz -C ./openclaw

# 启动 gateway
node openclaw/openclaw.mjs gateway --port 18789
```

## 构建新版本

### GitHub Actions（全平台）

前往 **Actions > Build and Release > Run workflow**，输入 openclaw 版本号（如 `2026.3.28`）。

Workflow 会启动 4 个 runner（macOS arm64、macOS x64、Linux x64、Windows x64），在各平台上执行 `npm install openclaw@{version}` 编译原生模块，打包 tarball 并上传到 GitHub Release。

### 本地构建（仅当前平台）

在 [AgentHow](https://github.com/boguan/AgentHow) 仓库中执行：

```bash
pnpm release:runtime
```

## macOS Gatekeeper

macOS 会给从网上下载的文件打上 quarantine 扩展属性（`com.apple.quarantine`），导致 Gatekeeper 拦截未经 Apple 公证的原生 `.node` 文件（如 `clipboard.darwin-universal.node`），弹出「Apple 无法验证……是否包含恶意软件」的提示。

CI 打包时已对所有 `.node` 文件做了 ad-hoc 签名（`codesign --force --sign -`），但 ad-hoc 签名不足以通过 quarantine 检查。下载解压后需要清除 quarantine 属性：

```bash
xattr -rd com.apple.quarantine ./openclaw
```

如果应用自动下载 tarball（如 Electron 应用），建议在解压完成后通过代码清除：

```typescript
import { execSync } from 'child_process';

if (process.platform === 'darwin') {
  execSync(`xattr -rd com.apple.quarantine "${installDir}"`, { stdio: 'ignore' });
}
```

## 兼容性

- 原生模块在 CI 构建时基于 Node.js 22 编译。发布前请确认与目标运行时（如 Electron 内嵌的 Node.js）的兼容性。
- 每个 tarball 仅适用于特定平台和架构。

## 许可证

Tarball 中包含的 openclaw 采用 [MIT](https://github.com/openclaw/openclaw/blob/main/LICENSE) 许可证。本仓库仅托管构建产物。
