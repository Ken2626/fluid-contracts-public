[根目录](../CLAUDE.md) > **fluid-contracts-public**

# fluid-contracts-public — Instadapp Fluid 合约源（外部 fork）

> Instadapp Fluid is a combination of DeFi protocols with a Liquidity layer at the core。

---

## 一、模块职责

**外部仓库 fork**（`instadapp/fluid-contracts-public`），在本工作区作为 Fluid 合约的 ABI / 源码参考，被 `lendingbot` 的 Fluid path / Vault T1 相关插件与测试引用。

不修改源码（除非有明确升级需求），保持与上游同步。

---

## 二、入口与启动

### Foundry

```bash
forge install foundry-rs/forge-std --no-git
forge install a16z/erc4626-tests --no-git
forge install transmissions11/solmate@b5c9aed --no-git
forge install OpenZeppelin/openzeppelin-contracts@v4.8.2 --no-git
forge install OpenZeppelin/openzeppelin-contracts-upgradeable@v4.8.2 --no-git

npm i

forge build
forge test                     # mainnet 版本
RUN_OTHERS=true forge test -v --ffi   # non-mainnet liquidity layer
```

### Hardhat（部分 JS 辅助）

```bash
npm run compile
npm test
# 数学测试（precision / fixtures）：
npm run forge-test-getRatioAtTick
npm run forge-test-bigMathVault-mulDivNormal
# 等等（见 package.json scripts）
```

---

## 三、对外接口

合约目录（典型上游布局，`contracts/`，本次未深扫）：

- Liquidity Layer
- Vault T1（被 lendingbot 使用）
- DEX
- Lending（fToken, FluidLending）

JS 辅助：`test/js-scripts/{getRatioAtTick,bigMathVault/*}.ts` — 数学函数 oracle / 黄金值生成。

---

## 四、关键依赖与配置

- `hardhat ^2.x`、`@nomicfoundation/hardhat-toolbox`、`ts-node`
- Foundry（`foundry.toml` + `forge install`）
- OpenZeppelin v4.8.2（注意：与 lendingbot 的 v5 不同版本）
- Solmate `b5c9aed`
- erc4626-tests（a16z）

环境：`.env` 含 `MAINNET_RPC_URL` 等（参考 `.env.example`）。

---

## 五、数据模型

Fluid Liquidity Layer + Vault + DEX 的标准结构；详见上游文档 [docs.md](https://github.com/instadapp/fluid-contracts-public/blob/main/docs/docs.md)。

---

## 六、测试与质量

- `forge test` — mainnet 版本
- `RUN_OTHERS=true forge test -v --ffi` — non-mainnet
- `npx hardhat test ./test/hardhat/**` — Hardhat 测试
- `test/js-scripts/*` — Math 辅助

---

## 七、常见问题 (FAQ)

- **Q：为什么用 OZ v4.8.2 而不是 v5？**
  A：上游决策，保持兼容；不要随意升级，可能破坏 storage layout / 接口。
- **Q：本仓库可以直接被 lendingbot 引用吗？**
  A：lendingbot 通过相对路径 / 自有 ABI 拷贝引用部分接口。请勿直接修改本目录的 Solidity 源以解决 lendingbot 问题；改 lendingbot 侧。

---

## 八、相关文件清单

- `foundry.toml`、`package.json`、`hardhat.config.*`
- `contracts/`（合约源）
- `test/hardhat/`、`test/js-scripts/`
- `docs/`（上游设计文档）
- `README.md`

> 上游目录树本次未深度展开。

---

## 九、变更记录 (Changelog)

| 日期 | 内容 |
|------|------|
| 2026-05-20 | 首次由架构师扫描生成模块 CLAUDE.md。 |
