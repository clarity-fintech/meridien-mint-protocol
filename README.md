# Meridien Mint Protocol

> **Moniversive Invariant Static (MIS)** module · settles on **CLRTY-1** (chain **1202**) · compiler **`misc`**  
> Creator **Chandler William Ferguson** · Org [`clarity-fintech/meridien-mint-protocol`](https://github.com/clarity-fintech/meridien-mint-protocol)

Cross-Chain, Bridges & Interoperability suite. MIS-only (`.mis`). Compiler `bin/misc`.

This repository is **self-contained and 100% downloadable**: it ships the prebuilt MIS
compiler at [`bin/misc`](bin/misc), so a fresh clone builds and checks with no extra setup.

---

## Contents

- [What this is](#what-this-is)
- [Quickstart](#quickstart)
- [Command reference](#command-reference)
- [Architecture & layout](#architecture--layout)
- [Kernel & invariants](#kernel--invariants)
- [Code sample — an invariant pack](#code-sample--an-invariant-pack)
- [Network binding (CLRTY-1)](#network-binding-clrty-1)
- [Verification / test output](#verification--test-output)
- [Bootstrap & portability](#bootstrap--portability)
- [Backlinks](#backlinks)

## What this is

**Meridien Mint Protocol** is written entirely in **MIS** (Moniversive Invariant Static, `.mis`) — a
letter-hashed, invariant-static language. Every module compiles under the **`misc`** kernel
(foreign kernels are rejected) to a deterministic *typed-letter root*, so a check is a
cryptographic proof that the module's invariants hold.

- **29** `.mis` modules: **1** kernel · **5** packs · **20** sections · **3** command modules
- **9** active invariants and **4** outcomes in the backlink kernel (verified below)
- Settlement network **CLRTY-1 / chain 1202**, RPC `https://rpc.clarity-fintech.com`
- Embed gates **3..=6**; deep root anchored to `moniversive`

## Quickstart

```bash
git clone https://github.com/clarity-fintech/meridien-mint-protocol
cd meridien-mint-protocol

# 0. (optional) refresh / rebuild the compiler — a prebuilt bin/misc is already committed
make bootstrap

# 1. compile-check the kernel + every .mis module in the repo
make check

# 2. validate the command surface
make commands-check

# 3. print the live CLRTY-1 network binding
make network-connect
```

## Command reference

| Command | What it does |
|---|---|
| `make bootstrap` | Ensure/refresh `bin/misc` (copies a shared build, or rebuilds from the CLRTY-MIS-Kernel Rust source). |
| `make check` | Compile-check the kernel and **every** `.mis` file with `--compact-letters`; prints the kernel proof JSON. |
| `make commands-check` | Compile-check only the command catalog under `mis/commands/`. |
| `make network-connect` | Runs `commands-check`, then prints the CLRTY-1 (chain 1202) binding, RPC, and command module. |
| `bin/misc mis/kernel/MeridienMintProtocolKernel.mis --check --compact-letters` | Check a single module directly with the compiler. |

Direct compiler invocation (what the Makefile runs under the hood):

```bash
bin/misc mis/kernel/MeridienMintProtocolKernel.mis --check --compact-letters
find mis -name '*.mis' -print0 | xargs -0 -n1 bin/misc --check --compact-letters
```

## Architecture & layout

```
mis/kernel/MeridienMintProtocolKernel.mis
mis/packs/cctp_burn_mint.mis
mis/packs/clrty1_usdc.mis
mis/packs/native_usdc.mis
mis/packs/pqc_settle.mis
mis/packs/worker_consensus.mis
mis/sections/CloudflareEdgeBind.mis
mis/sections/ClrtyMisKernelRepo.mis
mis/sections/GradientBand.mis
mis/sections/HelixManifest.mis
mis/sections/LanguageRoot.mis
mis/sections/MisAllCodeIndex.mis
mis/sections/MisArchitecture.mis
mis/sections/MisChainIntake.mis
mis/sections/MisCodeIndex.mis
mis/sections/MisEcosystemRepos.mis
mis/sections/MisKernelActiveOnly.mis
mis/sections/MisKernelSource.mis
mis/sections/MisNativeKernels.mis
mis/sections/MiscCompiler.mis
mis/sections/MlKernelRepo.mis
mis/sections/PaymentRails.mis
mis/sections/ProtocolStructures.mis
mis/sections/StaticMlCatalog.mis
mis/sections/TickArchitecture.mis
mis/sections/TickStakingGradient.mis
mis/commands/MeridienMintProtocolCommandCatalog.mis
mis/commands/MeridienMintProtocolCommands.mis
mis/commands/MeridienMintProtocolNetworkBind.mis
manifests/commands_manifest.json
manifests/domain_full_index.json
manifests/mis_repo_manifest.json
```

- **`mis/kernel/`** — the module's kernel entry: invariants + outcomes that gate everything else.
- **`mis/packs/`** — invariant packs (the executable logic units).
- **`mis/sections/`** — MIS + CLRTY-1 + edge backlink sections binding this repo into the ecosystem.
- **`mis/commands/`** — the callable command catalog and its CLRTY-1 network binding.
- **`manifests/`** — machine-readable domain, command, and repo manifests.

## Kernel & invariants

`mis/kernel/MeridienMintProtocolKernel.mis` — the kernel declares the invariants the compiler enforces on every check:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Meridien Mint Protocol Kernel
// Suite: Cross-Chain Bridges & Interoperability

module MeridienMintProtocolKernel {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant extension_mis: source_extension == mis;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant active_kernel_only: active_kernel == misc;
  invariant no_foreign_kernel: foreign_kernel_active == false;
  invariant creator_bound: creator == chandler_william_ferguson;
  invariant domain_bound: domain == meridien_mint;
  invariant clrty1_architecture_bound: clrty1_architecture_bound == true;
  invariant cross_chain_suite_bound: cross_chain_suite_bound == true;
  invariant ecosystem_catalog_bound: ecosystem_catalog_bound == true;

  invariant pack_cctp_burn_mint_flag: pack_cctp_burn_mint_bound == true;
  invariant pack_native_usdc_flag: pack_native_usdc_bound == true;
  invariant pack_pqc_settle_flag: pack_pqc_settle_bound == true;
  invariant pack_worker_consensus_flag: pack_worker_consensus_bound == true;
  invariant pack_clrty1_usdc_flag: pack_clrty1_usdc_bound == true;
```

## Code sample — an invariant pack

`mis/packs/cctp_burn_mint.mis`:

```rust
// Moniversive Invariant Static (MIS) — deep root
// Creator: Chandler William Ferguson
// Compile: bin/misc <file>.mis --check --compact-letters
// Title: Meridien Mint Protocol · CctpBurnMint
// Suite: Cross-Chain Bridges & Interoperability

module MeridienMintProtocolCctpBurnMint {
  invariant letter_hash_bound: letter_hash_root != @0;
  invariant deep_root_moniversive: deep_root == moniversive;
  invariant settlement_chain: chain_id == 1202;
  invariant settlement_network_clrty1: settlement_network == clrty_1;
  invariant kernel_is_misc: compiler_kernel == misc;
  invariant pack_cctp_burn_mint_bound: pack_bound == true;
  invariant catalog_backlink: mis_catalog_bound == true;

  invariant cctp_burn_mint_001: cctp_burn_mint_001_bound == true;
  invariant cctp_burn_mint_002: cctp_burn_mint_002_bound == true;
  invariant cctp_burn_mint_003: cctp_burn_mint_003_bound == true;
  invariant cctp_burn_mint_004: cctp_burn_mint_004_bound == true;
  invariant cctp_burn_mint_005: cctp_burn_mint_005_bound == true;
  invariant cctp_burn_mint_006: cctp_burn_mint_006_bound == true;
  invariant cctp_burn_mint_007: cctp_burn_mint_007_bound == true;
```

## Network binding (CLRTY-1)

`make network-connect` binds the command surface to the settlement network:

```
CONNECTED clrty-1/1202 meridien-mint-protocol
RPC https://rpc.clarity-fintech.com
MODULES mis/commands/MeridienMintProtocolCommands.mis
```

## Verification / test output

`make check` passes — the kernel proof for this repo:

```json
{
  "ok": true,
  "module": "MeridienMintProtocolBacklinkIndex",
  "kernel": "misc",
  "invariant_count": 9,
  "outcome_count": 4,
  "typed_letters": 1217,
  "active_kernel_only": true
}
```

*Reproduce:* `make check` → `"ok": true`, module `MeridienMintProtocolBacklinkIndex`, 9 invariants, 4 outcomes, 1217 typed letters.

## Bootstrap & portability

- A **prebuilt macOS `bin/misc`** is committed, so `make check` works immediately after clone.
- On other platforms (or to rebuild), run **`make bootstrap`** — it rebuilds `misc` from the
  [CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) Rust source via `cargo`,
  or copies a shared build if one is present in a parent checkout.
- The compiler accepts **MIS only**; foreign kernels are rejected by design.

## Backlinks

- [clarity-fintech/CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel) — the sole active `misc` compiler
- [clarity-fintech/moniversive_invariant_static_ML](https://github.com/clarity-fintech/moniversive_invariant_static_ML) — MIS language root
- [clarity-fintech/meridien-mint-protocol](https://github.com/clarity-fintech/meridien-mint-protocol) — this repository

---
MIS · CLRTY-1 (chain 1202) · Creator Chandler William Ferguson
