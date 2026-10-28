# Vector catalog export

This directory contains the reviewed, locally generated catalog consumed by
Vector's release tooling. It is data, not an executable provider implementation.

## Codium metadata restoration (28 October 2026)

The `codium-112` work branch restores four OpenRouter records from Vector's
previously reviewed 23 October catalog: DeepSeek V4.1 Flash 1017, Claude Haiku
5.5, Kimi K3.1 Pro and Gemini 4 Argon. The exact source is
`packages/web/public/models/api.json` at application commit
`bb2971e0bc10a43cc9b50f47950e48d4286cf92a` in
`https://github.com/scr1pter/claudeondfsfdaf`. That application mirror was later
replaced to match the older release input; these records had not reached this
fork's source files or committed export.

- Fork source revision: `d6c7d3eee3fd72ac8301574a9c9df12acb6d2e50`.
- Raw export SHA-256: `850dd7f9ea8b55752d421c7582c9e137a55a9fbe936f66711f3039cbb3446c99`.
- Raw export size: 5,313,020 bytes; 226 providers and 8,387 provider-model records.
- Validation: `bun run --no-env-file validate` passed. Two runs of the documented
  `packages/web` build produced identical `dist/_api.json` bytes under Bun 1.3.14
  after `bun --no-env-file install --frozen-lockfile --ignore-scripts`.
- Semantic comparison with the previous committed export found only the four
  added OpenRouter records; every existing provider and model value is unchanged.

Each new `models/` lab record and override-only `providers/openrouter/models/`
record reconstructs the corresponding reviewed JSON record exactly, including
capabilities, dates, modalities, reasoning controls, limits and costs. Existing
GPT-6 Luna metadata and its context price tiers remain unchanged. DeepSeek's
prices retain the reviewed higher time-of-day band, rather than advertising its
lower temporary rate. Haiku retains its reviewed 100,000-token context tier.

This is a restoration of captured public metadata, not a fresh availability or
ZDR audit. No inference, billing, provider sync, deployment or account-setting
operation is part of the export. The application must import a clean committed
fork revision through its normal release-catalog preparation and record the
prepared digest before using it. Do not point release settings at a moving
branch or modify the application's mirror independently of the reviewed export.

## Original export provenance (2 October 2026)

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
owners' marks. The upstream model data, generation code, and attribution are preserved. Original
artwork remains available at the source revision; the changes below normalize
its representation without replacing provider identity.

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

## Provider artwork normalization

Four upstream SVG files contained only embedded PNG images. To satisfy Vector's
unchanged static-SVG import rules, `atomic-chat`, `hpc-ai`, `orcarouter`, and
`wafer.ai` now represent those exact original pixels as static paths. Horizontal
runs of identical RGBA pixels are grouped by color, fully transparent pixels are
omitted, and all other colors and alpha values are retained. Nested SVG viewports
preserve each original image's dimensions and aspect-ratio behavior. The external
DOCTYPE in `hpc-ai` was removed along with its raster wrapper.

| Provider | Original PNG | Visible colors | Horizontal runs | Static SVG bytes |
| --- | --- | ---: | ---: | ---: |
| atomic-chat | 200 × 200 | 159 | 1,969 | 36,803 |
| hpc-ai | 384 × 86 | 4,069 | 9,482 | 381,669 |
| orcarouter | 42 × 22 | 225 | 381 | 11,732 |
| wafer.ai | 93 × 92 | 28 | 616 | 10,672 |

Their total size is 440,876 bytes. Chromium canvas comparison at each original
PNG's pixel dimensions found zero differing RGBA pixels, disregarding irrelevant
RGB channels under zero alpha. Original outer SVG dimensions remained identical;
32px and 64px side-by-side renders were also visually reviewed. This conversion
preserves raster detail; it does not invent higher-resolution brand artwork.

`regolo-ai` only loses an editor comment before its SVG root. Its existing path
geometry, viewBox, colors, and editor metadata remain unchanged. The complete MIT
license is unchanged. No model TOML or `vector/api.json` bytes changed, and the
export digest recorded above remains the same. All supplied artwork now passes
Vector's existing static-SVG content validator; missing artwork still follows
Vector's explicit neutral-icon policy.
