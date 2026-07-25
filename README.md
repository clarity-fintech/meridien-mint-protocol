# Meridien Mint Protocol

**Moniversive Invariant Static (MIS)** — Creator **Chandler William Ferguson**

Cross-Chain, Bridges & Interoperability suite. MIS-only (`.mis`). Compiler `bin/misc`.  
Settlement **clrty-1 / chain 1202**. Embed gates **3..=6**.

Backlinked to [https://github.com/clarity-fintech/moniversive_invariant_static_ML](https://github.com/clarity-fintech/moniversive_invariant_static_ML), [https://github.com/clarity-fintech/CLRTY-MIS-Kernel](https://github.com/clarity-fintech/CLRTY-MIS-Kernel), Helix, payment rails, and CLRTY-1 catalogs.

## Layout

- `mis/kernel/MeridienMintProtocolKernel.mis`
- `mis/packs/` — 5×20 = **100** invariants
- `mis/sections/` — MIS + CLRTY-1 + edge backlinks
- `mis/backlinks/MisBacklinkIndex.mis`
- `manifests/`

## Compile

```bash
bin/misc mis/kernel/MeridienMintProtocolKernel.mis --check --compact-letters
find mis -name '*.mis' -print0 | xargs -0 -n1 bin/misc --check --compact-letters
```

## Org

[clarity-fintech/meridien-mint-protocol](https://github.com/clarity-fintech/meridien-mint-protocol)
