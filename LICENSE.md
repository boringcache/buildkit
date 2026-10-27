# License

The BoringCache BuildKit image contains a modified version of
[BuildKit](https://github.com/moby/buildkit), which is licensed under the
Apache License, Version 2.0.

- Boringtech Ltd's changes and additions to BuildKit, and the `buildkitd` and
  `buildctl` programs built with them, are licensed under the
  [Elastic License 2.0](https://www.elastic.co/licensing/elastic-license).
- The original BuildKit code remains licensed under the Apache License,
  Version 2.0.
- The other programs and libraries in the image, including runc, libseccomp,
  musl, the CNI plugins, the QEMU user-mode emulators and the Alpine packages,
  keep their own licenses.

The v0.34.0-bc.2 image includes license files and notices in
`/usr/share/licenses/boringcache-buildkit/`. `NOTICE` lists where each part is.
`native-libraries/runc/` records the statically linked native libraries and
includes libseccomp's license, credits and source references:

```sh
docker run --rm --entrypoint cat \
  ghcr.io/boringcache/buildkit:v0.34.0-bc.2 \
  /usr/share/licenses/boringcache-buildkit/NOTICE
```

Copyright 2026 Boringtech Ltd.
