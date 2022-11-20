## sigstore-the-easy-way

Software signing just got easier. This guide is an easy way to get started with securing your software supply chains.

Sigstore is one of the companies in the community that's actively working towards resolving supply chain security issues. The internal working of sigstore components is quite complex, considering the level of protection it provides. In the following sections, we get started with sigstore in the easiest way(s) possible.

As we are trying to mitigate software supply chain security hacks, we often have to leverage other open-source tools to gather more information & secure the services. So, if you need help with non-sigstore toolings in the guide & don't get surprised.

## Outline

- [Authors](authors.md)
- [Tools](tools.md)
- [Cosign](cosign/index.md)
    - [Sign and verify with key](cosign/sign-and-verify-with-key.md)
    - [Sign and verify without key](cosign/sign-and-verify-without-key.md)
    - [Attach artifacts](cosign/attach-artifacts.md)
    - [Attest and verify artifacts](cosign/attest-and-verify-artifacts.md)
    - [Download artifacts](cosign/download-artifacts.md)
- [Rekor](rekor/index.md)
    - [Upload artifacts to public rekor](rekor/upload-artifacts-to-public-rekor.md)
    - [Upload artifacts to private rekor](rekor/upload-artifacts-to-private-rekor.md)
    - [Query transparency log](rekor/query-transparency-log.md)
    - [Compare the signatures uploaded to registry and transparency log](rekor/compare-the-signatures-uploaded-to-transparency-log-and-registry.md)
- [Fulcio](fulcio/index.md)
    - [Keyless signing certificates](fulcio/analyzing-code-signing-certificates.md)
- [SBOM](sbom/index.md)
    - [Use case](sbom/use-case.md)
    - [Formats](sbom/formats.md)
    - [Generate](sbom/generate.md)
- [Contribute](contribution.md)
- [Resources](resources.md)
