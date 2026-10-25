# Recovered mesh assets

Four recovered binaries are stored with Git LFS. Install Git LFS and run the following from the repository root after cloning:

```sh
git lfs install
git lfs pull
git lfs fsck
```

The repository's `.lfsconfig` directs LFS transfers to the independent public
[groundcover-lab-lfs storage repository](https://github.com/mjsikorsky/groundcover-lab-lfs).
The Git source and LFS pointer files remain in this fork; the LFS objects are
stored separately because GitHub rejected new LFS uploads to the original
public fork network. Both repositories retain the same asset paths and hashes.

## Exact Calamagrostis source mesh

`mesh/raw/calamagrostis-canescens/calamagrostis-canescens-mesh-v1.bin` is 67,538,096 bytes. Both its complete-file SHA-256 and geometry-payload SHA-256 match the existing [manifest](../mesh/raw/calamagrostis-canescens/manifest.json).

```text
complete file:
875624ad5a41051750ac754d7c0e06f2af636ab05735373a8897e523de4cb680

geometry payload:
ca98a3941055a04bb414ab0db98913a7db5a9280ff62912066b48a9a9ce7a0d1
```

The file was deterministically reconstructed from profile ID 2 in `Braffolk/fable5-world-demo`, pinned to commit `53d9a2527af744e453d1bb15e53760f461473813`. The [original generator](https://raw.githubusercontent.com/Braffolk/fable5-world-demo/53d9a2527af744e453d1bb15e53760f461473813/tools/groundcover-bake/EstonianGraminoids.ts) produces the mesh; the [original GCRP v4 packer](https://raw.githubusercontent.com/Braffolk/fable5-world-demo/53d9a2527af744e453d1bb15e53760f461473813/tools/groundcover-bake/PeriodicProfile.ts) produces the quantized geometry records independently of the GPU atlas.

The reconstruction uses the packed geometry and the documented 192-byte GCMESH1 header. The original GCRP source hash is preserved as provenance from the manifest; the original GPU-baked GCRP atlas was not reconstructed. The complete resulting GCMESH1 file matches the manifest hash exactly.

## Historical 009 BTF artifacts

The three files under `mesh/baked/009-canopy-btf/` were recovered from the last tracked snapshot before upstream deleted its baked assets, commit `e07458132972241f5ec6fb707e52c43b82f55078`. The [deletion commit](https://github.com/Braffolk/groundcover-lab/commit/92d1ea45c51174fade069e826186ed40a8133b04) explicitly retains the artifacts in Git history.

| File | Bytes | SHA-256 |
|---|---:|---|
| `btf-v1-calamagrostis-canescens.bin` | 19,652,512 | `ae5c126ad3394ea5db7bfae9720155df0a92956d41be00a02e59129828513a55` |
| `btf-v1-elymus-repens.bin` | 19,652,512 | `ac1b06b20c112d72109a0ec1ff21204bf52655a39f89c8921e63ecc93c974c9e` |
| `btf-v1-poa-pratensis.bin` | 19,652,512 | `1289db804bb773cebe82699e43d2304c6d5b9e2f779c3575e96e83e4d27a60e5` |

These artifacts predate the [source-normal decoding correction](https://github.com/Braffolk/groundcover-lab/commit/8d4d7c7). They are loadable historical bakes, not regenerated current-correct assets. The current loader prefers an existing compatible BTF over regenerating it from the raw mesh, so installing the Calamagrostis source does not automatically replace its legacy BTF.

## Verification and boundaries

- The exact source passes the current GCMESH1 parser, every triangle index is in bounds, all triangle padding words are zero, and sampled decoded normals have unit length.
- All three historical BTFs pass the current `loadOrBakeSpecies` implementation without raw-mesh requests and survive a byte-identical unpack/repack round trip.
- Vite serves all four files as binary responses with matching hashes.
- Direct Vite builds preserve these hashes; raw meshes require `GC_DEPLOY_MESHES=1` for inclusion in static builds.
- No full browser/GPU bake, rendered-frame comparison, or performance qualification was performed.
- Elymus and Poa raw source meshes are still absent; only their historical 009 BTF artifacts are included.
- The pre-existing `npm run build` TypeScript failures involving missing preview/material exports are not fixed by this asset recovery.

CI and deployment checkouts must fetch LFS content before building. On GitHub Actions, configure `actions/checkout` with `lfs: true`; otherwise Vite may copy small LFS pointer files instead of the binaries.
