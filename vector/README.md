# Vector catalog export

This directory contains the reviewed, locally generated catalog consumed by
Vector's release tooling. It is data, not an executable provider implementation.

## Provenance

- Owner repository: https://github.com/scr1pter/vector-model-catalog
- Original repository: https://github.com/anomalyco/models.dev
- Source revision: `b4f2643a258f7c7a5eaa92367ac11d45414760c1`
- Source branch at review: `dev`
- Generated on: 2 October 2026
- Runtime: Bun `1.3.14`
- Lockfile SHA-256: `d1646e70ffeffd33c35a1a22f656ab2ae42292c7c6455dbd3de7f4d322ff13ae`
- Generated source: `packages/web/dist/_api.json`
- Export SHA-256: `fd4557276f60170ad7757d8ff13f26fe7131234688dd275044ad29bb3fa78799`
- Export size: 5,310,370 bytes; 226 providers and 8,383 provider-model records.

The complete original MIT license remains in [`../LICENSE`](../LICENSE), including
Copyright (c) 2025 models.dev. Provider names and artwork remain their respective
owners' marks. The raw upstream data, code, and attribution are preserved.

## Reproduce

From a clean checkout of the source revision above, using the recorded Bun version:

```sh
bun --no-env-file install --frozen-lockfile --ignore-scripts --registry=https://registry.npmjs.org
cd packages/web
bun run --no-env-file build
```

Copy the exact generated `dist/_api.json` bytes into `vector/api.json` at the
repository root. The build was run twice from unchanged tracked source; both JSON
outputs had the recorded digest. Dependencies were installed from the frozen
lockfile with lifecycle scripts disabled. No sync, inference, deployment, account,
credential, or publishing operation was used to generate this export.

The build reads this checkout's `models/` and `providers/` TOML files, validates
and merges model metadata, and applies the upstream default model-type filter.
That default omits 12 specialized `decision` records; `dist/_api-all.json` contains
8,395 records and is not the release export selected here.

The reviewed source has 171 model symlinks, all confined to the checkout. Its 33
broken SiliconFlow-CN aliases are skipped by the local generator and absent from
both JSON variants. No source entries were repaired, invented, or substituted
with a live API download.

Vector's application release gate separately filters approved provider identities
and bundled SDKs, validates imported SVGs, and records its prepared catalog digest.
This raw export is not a claim that every provider/model is supported, free, or
currently available. The OpenRouter free-only runtime checks remain separate.

The final owner-fork commit containing this export must be pinned by Vector's
release settings; the source revision above records the input used for generation.
