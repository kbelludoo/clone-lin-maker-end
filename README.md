# clone-lin-maker-end

Experimental **LIN** clone of MakerDAO `End` global-settlement math
(`cage(ilk)` tag / `skim` gap / `flow` fix; uint64-scale WAD'=1e6 RAY'=1e9).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/end.sol` sha256 `40eca7c82e6794a4dabfa9b6ae41f177dd3c0bafacefc14f060b88f52667adef` blob `14e17b195c82bf0266db7bb78252b7b689f8b42f`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_end.lin`](src/lin_maker_end.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_end_c11.c`](test/oracles/maker_end_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_end_kernel.c`](test/oracles/maker_end_kernel.c)
- **Proofs (lin-open only):** `examples/maker_end/` and `make maker-end-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256. Not a production End replacement.

Named goldens (uint64-scale):

| id | inputs | result |
|---|---|---|
| cage tag | par=1e9 pip=2e9 | **500000** |
| cage while live | live=1 | **0** |
| skim full | ink=100000 owe=50000 | gap' **0** |
| skim under | ink=30000 owe=50000 | gap' **20000** |
| flow after under | Art=100e6 gap=20000 debt=1e17 | fix **300000** |
| cash 100 DAI | wad=100e6 fix=300000 | **30000** |
| flow no gap | gap=0 | fix **500000** |

`live` / `tag` / `par` / `pip` / `art` / `rate` / `ink` / `gap` / `debt` are arguments
(Solidity uses OSM `pip.read`, `vat.ilks`/`vat.urns` SLOADs and `vat.grab`).
`7e18+3e18` fail-closes while naive i64 wraps. Distinct from Vow settle and Spotter.poke.

## Reproduce proofs (in lin-open)

```
make maker-end-proof
```

## Not claimed

Uint256 / mainnet WAD=1e18 / RAY=1e27 / actual `pip.read` or `vat.grab`.
LIN replacing Maker, Sky, or the EVM. TVL or dollar savings. zk soundness.
Hidden OSM / Vat SLOADs.
