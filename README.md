# Groundcover lab LFS storage

This independent public repository stores four recovered binary assets using Git LFS for [mjsikorsky/groundcover-lab](https://github.com/mjsikorsky/groundcover-lab). That fork's `.lfsconfig` points to this repository's LFS endpoint, preserving the fork relationship while keeping the asset downloads public.

## Usage

Use the application repository normally:

```sh
git clone https://github.com/mjsikorsky/groundcover-lab.git
cd groundcover-lab
git lfs install
git lfs pull
```

To download only the recovered asset set:

```sh
git clone https://github.com/mjsikorsky/groundcover-lab-lfs.git
cd groundcover-lab-lfs
git lfs install
git lfs pull
git lfs fsck
```

## Contents and checksums

| Relative path | Bytes | SHA-256 |
|---|---:|---|
| `mesh/raw/calamagrostis-canescens/calamagrostis-canescens-mesh-v1.bin` | 67,538,096 | `875624ad5a41051750ac754d7c0e06f2af636ab05735373a8897e523de4cb680` |
| `mesh/baked/009-canopy-btf/btf-v1-calamagrostis-canescens.bin` | 19,652,512 | `ae5c126ad3394ea5db7bfae9720155df0a92956d41be00a02e59129828513a55` |
| `mesh/baked/009-canopy-btf/btf-v1-elymus-repens.bin` | 19,652,512 | `ac1b06b20c112d72109a0ec1ff21204bf52655a39f89c8921e63ecc93c974c9e` |
| `mesh/baked/009-canopy-btf/btf-v1-poa-pratensis.bin` | 19,652,512 | `1289db804bb773cebe82699e43d2304c6d5b9e2f779c3575e96e83e4d27a60e5` |

The Calamagrostis source mesh was reconstructed from a deterministic upstream generator and matches the original source manifest's complete-file and geometry-payload hashes exactly. The three BTFs were recovered from upstream Git history and predate a normal-decoding correction; they are historical loadable artifacts, not regenerated current-correct bakes.

The copied [source manifest](mesh/raw/calamagrostis-canescens/manifest.json) and [provenance and verification notes](docs/recovered-mesh-assets.md) record the recovery details. The other two species' raw source meshes are not included, and the original GCRP GPU atlas was not reconstructed.

No asset license is inferred or granted by this storage repository. See the upstream projects and asset provenance for applicable rights and terms.
