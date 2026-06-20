# Nub — Node.js 全合一工具包

> 来源: https://nubjs.com/blog/introducing-nub
> 日期: 2026-06-20
> 分类: 工具链
> 标签: Node.js, Rust, Bun, 包管理器, TypeScript, DX, 工具链, CLI

Nub 是一个用 Rust 编写的 Node.js 全合一工具包。一个 Rust 二进制文件即可运行文件和脚本、安装依赖、管理 Node 本身——在你已有的 Node 之上提供 Bun 式的现代开发体验（DX）。不需要采用新的运行时，没有锁定：每个增强都基于 Node 自己的公开扩展接口。

GitHub: https://github.com/nubjs/nub

## 核心组件

### 1. 文件运行器 `nub <file>`

`node <file>` 的逐 flag 替代品，也能直接运行 TypeScript 和 JSX——无需 tsconfig，无需构建步骤。

- 整个 TS 语法面都支持，包括不可擦除语法（enum、namespace、参数属性）和带 `emitDecoratorMetadata` 的旧式装饰器
- 导入解析方式与编辑器一致（无扩展名、`.js` → `.ts`、tsconfig paths）
- `.env*` 文件自动加载并支持 `${VAR}` 展开
- 数据文件可作为解析值导入（`.yaml`, `.toml`, `.jsonc`, `.json5`, `.txt`）
- 即使旧版 Node 也拥有现代全局变量（Temporal、URLPattern、WebSocket、EventSource、`node:sqlite`、Web Workers 等），每个都通过特性检测的 polyfill 或按 Node 版本段的 flag 注入实现

### 2. 脚本运行器 `nub run`

`npm run` / `pnpm run` 的替代品，冷启动约快 24 倍。完整的 pnpm workspace 表面支持。

### 3. 包运行器 `nubx` / `nub dlx`

`npx` / `pnpm dlx` 的替代品，本地优先 + registry 回退。在 Rust 中解析 `node_modules/.bin` 并直接 exec，约快 19 倍。

### 4. 包管理器 `nub install`

内置 pnpm 形状的安装引擎（嵌入 aube 引擎），支持 npm/pnpm/Bun lockfile 读写，Yarn 只读。通过全局内容寻址存储去重，reflink/hardlink 物理化。依赖构建脚本默认拒绝执行。

### 5. 包元管理器 `nub pm`

Corepack 的 Rust 实现，可配置和运行项目 pin 的确切 pnpm/npm/yarn 版本。

### 6. Node 版本管理器 `nub node`

支持 `.node-version`/`.nvmrc`/`engines.node` 版本锁定，从 nodejs.org 获取、SHA-256 验证、缓存。

### 7. Watch 模式 `nub watch`

基于依赖图 + 离图文件的变更重启。

## 工作原理

Nub 不是 Node fork。它是一个 Rust CLI，通过 Node 自己的扩展接口协调已安装的 Node：

- `module.registerHooks()` 用于 TS 转译和解析
- `--import` 预加载用于 polyfill
- V8 flag 注入用于解锁实验性特性
- 基于 oxc 的 N-API 插件用于快速转译

## 系统要求

- Node 18.19+
- macOS (arm64/x64)、Linux (x64/arm64)、Windows (x64/arm64)

## 许可证

MIT

## 相关页面

- [[OpenCLI]]

---

*由 knowledge-wiki skill 自动收录并翻译整理*
