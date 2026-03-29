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

Tarballs are built from the official npm package. The build script lives in the consumer project (e.g. [AgentHow](https://github.com/boguan/AgentHow)):

```bash
# In the AgentHow repo
pnpm release:runtime
```

This reads the version from `openclaw/package.json`, packages the directory, and uploads to this repo's GitHub Releases.

## Compatibility

- Native modules are compiled for the Node.js version used during `npm install`. Verify compatibility with your target runtime (e.g. Electron's embedded Node.js) before shipping.
- Tarballs are platform and architecture specific. Cross-platform distribution requires separate tarballs for each target.

## License

The tarballs contain openclaw, which is licensed under [MIT](https://github.com/openclaw/openclaw/blob/main/LICENSE). This repository only hosts build artifacts.
