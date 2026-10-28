# Dependency refresh (2026-10-28)

Docker SDK 28.5.2 had two high advisories with no fixed SDK release (GHSA-rg2x-37c3-w2rh and GHSA-x86f-5xw2-fm2r). The SDK has now been removed from go.mod, go.sum and vendor. This project used only its build-argument, archive, xattr and image-health configuration helpers; no daemon API is required.

Archive/compression operations now use maintained moby/go-archive 0.3.3 directly. Health configuration uses the same docker-image-spec type that Docker aliased. Linux xattr operations use containerd/continuity/sysx, preserving no-follow operations, absent-attribute handling and unsupported-filesystem behavior. Non-Linux capability handling remains a no-op, as in the former Docker helper.

The small build-argument helper and its upstream tests are extracted from Moby v28.5.2 into internal/buildargs, with LICENSE and NOTICE retained. This is Dockerfile ARG/proxy-filtering logic; it includes no Docker daemon/client code. The existing Kaniko wrapper and upstream helper tests verify its behavior.

Validation before publication: upstream build-argument tests, Kaniko Dockerfile tests, selected archive/utility tests, Linux executor/warmer cross-builds and Linux executor-test compilation pass. A full native util run has one Linux-only /proc/self/mountinfo failure on macOS; full executor tests also require Linux. Full Linux CI must be assessed on this exact replacement commit. No running builder service has been deployed.

The go-containerregistry 0.22.1 pull limiter exposed an existing stream-lifetime bug: deferring all layer closes until the full extraction finished exhausted registry permits. Layer extraction now scopes each reader close before the next layer opens, including error paths. A six-layer regression test (more than the default four permits) fails before the fix and passes afterward; malformed-archive cleanup is covered too. Historical integration fixtures still require externally available Debian Bullseye/UBI 7 packages and are reported separately if those package repositories fail.

Build and CI use Go 1.27.3, the current stable patch verified against go.dev release metadata on October 28. It includes the October crypto/x509 security fixes; Go 1.27.0 was insufficient. go.mod retains the supported Go 1.26 line at patched minimum 1.26.10 and selects toolchain go1.27.3. This updates repository build requirements, not installed host services.
