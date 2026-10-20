# BoringCache BuildKit

[![CI](https://github.com/boringcache/buildkit/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/boringcache/buildkit/actions/workflows/ci.yml)
[![Verify BuildKit Image](https://github.com/boringcache/buildkit/actions/workflows/verify-image.yml/badge.svg)](https://github.com/boringcache/buildkit/actions/workflows/verify-image.yml)
[![Sign BuildKit Image](https://github.com/boringcache/buildkit/actions/workflows/sign-image.yml/badge.svg)](https://github.com/boringcache/buildkit/actions/workflows/sign-image.yml)

BoringCache provides a managed BuildKit image for shared Docker layer cache,
persistent cache mounts, and supported tool caches. Use it with the BoringCache
CLI or GitHub Action. See the [Docker setup guide](https://boringcache.com/docs/adapters/docker)
for configuration and requirements.

Report security issues privately through
[GitHub private vulnerability reporting](https://github.com/boringcache/buildkit/security/advisories/new).

## Image

```text
ghcr.io/boringcache/buildkit:v0.33.1-bc
```

Tags follow upstream BuildKit versions with a BoringCache patch suffix:

- `v0.33.1-bc.4` is upstream BuildKit `v0.33.1` plus BoringCache patch release
  `4`. It prevents canceled uploads of image metadata after cache export,
  fixes concurrent cache-key snapshots and updates runtime security dependencies.
  It retains the managed `type=boringcache` layer-cache path, concurrent
  Bake upload coalescing, and opt-in cache-mount and tool-cache support. It also
  retains the tar extraction containment and dependency security fixes, and
  fixes a race between solver request cancellation and finalization. Cache-mount
  archives use stable, readable mount names across workers; archives under
  previous mount names are not selected by the new names. One daemon-wide
  limit admits one to four cache-mount archive worker processes from the
  BuildKit CPU budget and reports active, queued, peak, and wait metrics.
  Cache mounts can be addressed by their own namespace instead of the layer
  tag, and a namespaced mount is published merge-safely against the snapshot
  the publisher merged.
- `v0.33.1-bc` is the managed stable channel for the latest signed BoringCache
  patch release on that upstream base.
- `latest` moves only when BoringCache promotes a new managed BuildKit image.

This image is the BoringCache managed builder image used by the BoringCache CLI
and `boringcache/one`.

## Releases

Release tags correspond to managed BuildKit images for Linux `amd64` and
`arm64`.

Every exact release image is published with provenance/SBOM attestations,
scanned for HIGH/CRITICAL vulnerabilities, and signed by digest with
Sigstore/cosign. This public repository signs and verifies the exact image
digest before promoting `v0.33.1-bc` and `latest` to that digest.

The signed Git release tag also records the image tag and immutable digest. The
signing and verification workflows compare that signed metadata with GHCR before
trusting the image.

Inspect the image:

```sh
docker buildx imagetools inspect ghcr.io/boringcache/buildkit:v0.33.1-bc.4
```

From a checkout of this repository, verify the signature with its release key:

```sh
digest="$(
  docker buildx imagetools inspect \
    ghcr.io/boringcache/buildkit:v0.33.1-bc.4 \
    --format '{{json .Manifest.Digest}}' |
    jq -r .
)"

cosign verify \
  --key .github/release-cosign-v1.pub \
  "ghcr.io/boringcache/buildkit@${digest}"
```
