# openclaw-bin

Pre-built platform-specific tarballs of [openclaw](https://github.com/openclaw/openclaw) for embedding in desktop applications.

## What is this?

openclaw is published on npm as a source package. Installing it requires `npm install` which resolves dependencies, compiles native modules, and takes several minutes. This repo hosts ready-to-run tarballs with all dependencies pre-installed, so desktop apps can download and extract them without needing npm or Node.js on the user's machine.

Each tarball contains:
- `openclaw.mjs` (entry point)
- `dist/` (pre-built JS bundles)
- `node_modules/` (all dependencies, including pre-compiled native modules)
- `assets/`, `skills/`, `dist-runtime/`

## Downloads

Tarballs are published as GitHub Release assets. Each release tag matches the openclaw version.

| Platform | Architecture | Filename |
|----------|-------------|----------|
| macOS | Apple Silicon | `openclaw-{version}-darwin-arm64.tar.gz` |
| macOS | Intel | `openclaw-{version}-darwin-x64.tar.gz` |
| Linux | x64 | `openclaw-{version}-linux-x64.tar.gz` |
| Windows | x64 | `openclaw-{version}-win32-x64.tar.gz` |

## Usage

```bash
# Download and extract
curl -L https://github.com/boguan/openclaw-bin/releases/download/v2026.3.28/openclaw-2026.3.28-darwin-arm64.tar.gz | tar -xz -C ./openclaw

# Run the gateway
node openclaw/openclaw.mjs gateway --port 18789
```

## Building a new release

### GitHub Actions (all platforms)

Go to **Actions > Build and Release > Run workflow**, enter the openclaw version (e.g. `2026.3.28`).

The workflow spins up 4 runners (macOS arm64, macOS x64, Linux x64, Windows x64), runs `npm install openclaw@{version}` on each to compile native modules natively, packages tarballs, and uploads them to a GitHub Release.

### Local (current platform only)

From the [AgentHow](https://github.com/boguan/AgentHow) repo:

```bash
pnpm release:runtime
```

## Compatibility

- Native modules are compiled against Node.js 22 during the CI build. Verify compatibility with your target runtime (e.g. Electron's embedded Node.js) before shipping.
- Each tarball is platform and architecture specific.

## License

The tarballs contain openclaw, which is licensed under [MIT](https://github.com/openclaw/openclaw/blob/main/LICENSE). This repository only hosts build artifacts.
