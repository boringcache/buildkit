# Security Policy

## Reporting a Vulnerability

Please do not open a public issue or pull request for a potential security
vulnerability.

Use GitHub's private vulnerability reporting for this repository:

https://github.com/boringcache/buildkit/security/advisories/new

Include the image tag or digest, the workflow/run link if relevant, the impact
you believe is possible, and enough reproduction detail for us to verify the
report. Do not include live credentials, customer data, or secrets.

## Supported Versions

This repository supports the current promoted BoringCache managed BuildKit
image tags:

- `ghcr.io/boringcache/buildkit:latest`
- `ghcr.io/boringcache/buildkit:v0.30.0-bc`
- `ghcr.io/boringcache/buildkit:v0.30.0-bc.8`

Older tags may be superseded by a new promoted image instead of receiving a
backport.

## Scope

Reports for this repository should cover the BoringCache managed BuildKit image
distribution surface: image publication, signing, verification, release
metadata, attestations, and vulnerabilities present in the published image.

If a report concerns upstream BuildKit itself and is not specific to the
BoringCache managed image, please also consider the upstream BuildKit security
process.
