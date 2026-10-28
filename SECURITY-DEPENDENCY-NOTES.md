# Dependency refresh (2026-10-28)

The tracked vendor tree is regenerated from go.mod/go.sum. Docker SDK 28.5.2 still references the older go-archive compression API. Three vendored files retain a small compatibility patch for go-archive 0.3.3: pkg/archive/archive_deprecated.go uses compression.Compression; daemon/graphdriver/fsdiff.go and builder/remotecontext/git.go import compression and use compression.None. Reapply these adaptations after regenerating vendor until Docker provides a compatible release. The uncompressed archive behavior is unchanged.

Validation: Linux executor cross-build, Linux utility-test compilation, and native TestGetInputFrom/TestRetry/TestRetryWithResult pass. Full privileged Linux/container integration tests were not run on the Mac.

Docker SDK 28.5.2 remains subject to GHSA-rg2x-37c3-w2rh and GHSA-x86f-5xw2-fm2r; no fixed SDK release was available. These concern daemon archive/copy operations. This dependency update does not claim those advisories fixed or dismiss their alerts. Running container-builder hosts/services have not been deployed or upgraded by this source change.
