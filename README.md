# BoringCache BuildKit

This is the public distribution surface for the managed BuildKit image used by
BoringCache.

The repository is intentionally thin. It tracks image tags, signing,
verification, and release metadata; it is not a source mirror, image builder, or
standalone BuildKit fork.

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

The image is intended for BoringCache-managed builders and is not a standalone
replacement for upstream BuildKit.

## Releases

Release tags correspond to managed BuildKit images for Linux `amd64` and
`arm64`.

Release images are built from BoringCache's private monorepo source, then this
public repository signs and verifies the already-published digest. Every release
image is published with provenance/SBOM attestations, scanned for HIGH/CRITICAL
vulnerabilities, and signed by digest with Sigstore/cosign.

Inspect the image:

```sh
docker buildx imagetools inspect ghcr.io/boringcache/buildkit:v0.30.0-bc.1
```

Verify the signature:

```sh
cosign verify \
  --certificate-identity-regexp '^https://github.com/boringcache/.+/.github/workflows/.+@refs/(heads/main|tags/v.+)$' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com \
  ghcr.io/boringcache/buildkit:v0.30.0-bc.1
```
