# Performance of WebAssembly Runtimes in 2026

> 来源: https://00f.net/2026/06/23/webassembly-runtimes-2026
> 日期: 2026-06-23
> 分类: 工具链
> 标签: WebAssembly, WASM, runtime, Wasmer, Wasmtime, WAVM, WAMR, Bun, Node.js, 性能基准测试, libsodium, wide_arithmetic, AOT

## 概述

Frank Denis（Jedi/Sector One）使用 libsodium 加密库作为基准测试，连续三年（2024、2025、2026）对比了主流 WebAssembly runtime 的性能表现。这项测试的独特价值在于使用真实密码学库而非合成 benchmark，能准确反映 WebAssembly 在计算密集型任务中的实际性能。

## 测试环境

- **CPU**: AMD Ryzen AI 9 HX 470, 12 核 24 线程, 基础频率 2 GHz
- **操作系统**: Linux 7.1.0-rc7
- **编译工具**: Zig 0.17.0-dev
- **基准**: libsodium 基准测试套件（stream ciphers、hashing、signing、key exchange 等）

## 测试的 Runtime 及版本

| Runtime | 2024 | 2025 | 2026 |
|---------|------|------|------|
| Bun | 1.1.16 | 1.2.17 | 1.3.14 |
| Node.js | 22.3.0 | 24.2.0 | 26.3.1 |
| WAMR | 2.1.0 | 2.3.1 | 2.4.4 |
| WABT (wasm2c) | 1.0.35 | 1.0.37 | 1.0.41 |
| WasmEdge | 0.14.0 | 0.14.1 | 0.17.0 |
| Wasmer | 4.3.2 | 6.0.1 | 7.1.0 |
| Wasmtime | 22.0.0 | 34.0.0 | 46.0.0 |
| WAVM | — | — | nightly/2026-04-05 |
| Wazero | 1.7.3 | 1.9.0 | 1.12.0 |

## 核心发现

### 1. Wasmer 是综合性能最佳的 runtime

Wasmer 在 2026 年的表现最为出色，尤其是在启用 `wide_arithmetic` 指令后，性能从 native 的 2.08x 提升到 1.33x。

### 2. WAVM 的优化器表现惊艳

WAVM 拥有最好的优化器，能从基础 WebAssembly 字节码生成极快的机器代码。虽然版本较新（nightly），但其优化能力已经令人印象深刻。

### 3. Wasmtime 稳步提升

Wasmtime 每年都在持续进步：
- **2024**: 2.67x native
- **2025**: 2.54x native
- **2026**: 2.41x native

在启用 `wide_arithmetic` 后，更是从 2.41x 飙升到 1.46x native。

### 4. WAMR 的 AOT 模式非常优秀

WAMR 在 AOT（Ahead-of-Time）编译模式下保持了约 1.5x native 的优秀表现，是一个可靠的高性能选择。

### 5. wasm2c 仍然是非常好的选择

将 WebAssembly AOT 翻译为 C 代码再编译的方案（wasm2c）一直表现不错，证明了简单方法的有效性。

### 6. Bun 在 2025→2026 间飞跃式提升

Bun 从 2025 到 2026 年有约 3 倍的性能提升，进步显著。

## 关键技术：`wide_arithmetic` 指令

文章最核心的发现之一是 WebAssembly 新增的 `wide_arithmetic` 提案对加密代码意义重大。密码学运算大量使用 64 位整数乘法等宽整数运算，新的指令让 runtime 可以直接使用硬件指令而非软件模拟：

| Runtime | 启用前 | 启用后 | 提升 |
|---------|--------|--------|------|
| Wasmtime | 2.41x | 1.46x | ~1.65x |
| Wasmer | 2.08x | 1.33x | ~1.56x |

这个提案对任何涉及密码学或大整数运算的 WebAssembly 应用都有深远影响。

## 测试维度

基准测试覆盖了 libsodium 的多个关键操作：
- **Stream ciphers**: ChaCha20
- **Hashing**: SHA-256, SHA-512
- **Signing**: Ed25519
- **Key exchange**: X25519
- **Secretbox**: 对称加密
- **Generichash**: 通用哈希

不同 runtime 在不同操作上的表现有差异，但总体排名相对稳定。

## 结论与选择建议

- **追求极致性能**: Wasmer 或 WAVM
- **稳定性与持续改进**: Wasmtime（进步最快、社区最活跃）
- **嵌入式/资源受限场景**: WAMR AOT 模式
- **简单可靠方案**: wasm2c（编译为原生代码）
- **需要 JavaScript 生态集成**: Bun（进步显著）

## 相关页面

- [[Nub-Node.js全合一工具包]]

---

*由 knowledge-wiki skill 自动收录并翻译整理*
