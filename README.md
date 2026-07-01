# BoringCache BuildKit

[![CI](https://github.com/boringcache/buildkit/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/boringcache/buildkit/actions/workflows/ci.yml)
[![Verify BuildKit Image](https://github.com/boringcache/buildkit/actions/workflows/verify-image.yml/badge.svg)](https://github.com/boringcache/buildkit/actions/workflows/verify-image.yml)
[![Sign BuildKit Image](https://github.com/boringcache/buildkit/actions/workflows/sign-image.yml/badge.svg)](https://github.com/boringcache/buildkit/actions/workflows/sign-image.yml)

This is the public distribution surface for the managed BuildKit image used by
BoringCache.

The repository is intentionally thin. It tracks image tags, signing,
verification, and release metadata for the managed image.

## Image

```text
ghcr.io/boringcache/buildkit:v0.30.0-bc.1
```

Tags follow upstream BuildKit versions with a BoringCache patch suffix:

- `v0.30.0-bc.1` is upstream BuildKit `v0.30.0` plus BoringCache patch release
  `1`.
- `v0.30.0-bc` moves to the latest BoringCache patch release for that upstream
  base.
- `latest` moves only when BoringCache promotes a new managed BuildKit image.

This image is the BoringCache managed builder image used by the BoringCache CLI
and `boringcache/one`.

## Releases

Release tags correspond to managed BuildKit images for Linux `amd64` and
`arm64`.

Every release image is published with provenance/SBOM attestations, scanned for
HIGH/CRITICAL vulnerabilities, and signed by digest with Sigstore/cosign. This
public repository signs and verifies the promoted image digest.

Inspect the image:

```sh
docker buildx imagetools inspect ghcr.io/boringcache/buildkit:v0.30.0-bc.1
```

Verify the signature:

```sh
digest="$(
  docker buildx imagetools inspect \
    ghcr.io/boringcache/buildkit:v0.30.0-bc.1 \
    --format '{{json .Manifest.Digest}}' |
    jq -r .
)"

cosign verify \
  --certificate-identity 'https://github.com/boringcache/buildkit/.github/workflows/sign-image.yml@refs/heads/main' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  "ghcr.io/boringcache/buildkit@${digest}"
```
