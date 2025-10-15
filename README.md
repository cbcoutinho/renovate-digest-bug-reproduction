# Renovate Digest Bug Reproduction

This repository reproduces the digest concatenation bug when using `replacementNameTemplate` with `registryAliases` in docker-compose files.

## Bug Description

When using `replacementNameTemplate` package rules combined with `registryAliases` for Docker images in docker-compose files, Renovate corrupts digests by concatenating the entire digest value, resulting in `sha256:sha256:HASHHASH` instead of `sha256:HASH`.

## Files

- `renovate.json`: Renovate configuration with registry aliases and replacement template
- `docker-compose.yml`: Docker Compose file with digest-pinned redis image

## Expected Behavior

Renovate should create a replacement branch with:
```yaml
image: docker.io/library/redis:alpine@sha256:987c376c727652f99625c7d205a1cba3cb2c53b92b0b62aade2bd48ee1593232
```

## Actual Behavior

Renovate fails with digest corruption, doubling the digest to:
```
sha256:sha256:987c376c727652f99625c7d205a1cba3cb2c53b92b0b62aade2bd48ee1593232c727652f99625c7d205a1cba3cb2c53b92b0b62aade2bd48ee1593232
```

## Reference

See: https://github.com/renovatebot/renovate/discussions/38577
