# visage3d — working memory

## architecture
- three.js WebGL renderer with GLTFLoader
- VisageViewer: mount into any container, load GLB, play animations
- AssetManager: fetch-on-demand with blob URL caching
- morph target API: setMorphTargets(), getMorphTargetNames(), resetMorphTargets()
- fallback silhouette on load failure

## build
- vite + typescript strict
- lib output: ~97KB
- harness pages at /harness/viewer.html and /harness/assets.html

## assets
- ellie GLB (74MB via LFS) from personas repo
- registered in ASSET_REGISTRY with CC BY 4.0 attribution
- fetched from raw.githubusercontent.com on demand

## open questions
- actual morph target names on Ellie model unknown (need to load and discover)
- git-lfs not installed in containers
- no automated tests (per unix-web constraint: visual inspection only)

## branches shipped
- feature/sophia-scaffold (merged) — initial viewer + asset manager
- feature/sophia-morph-targets — morph target API + fallback + harness sliders
