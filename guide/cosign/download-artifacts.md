# Download artifacts

Whenever the operations like signing & attesting of artifacts are performed, the signatures, attestations, SBOMs, or blobs are uploaded directly to the registry. In this case, docker cli doesn't help pull/read these non-image artifacts. When we pull the artifact with the docker command, it tries to un-tar the compressed file to an image format. Numerous other ways exist to pull/read the signatures/attestations/SBOMs/blobs from the registry. We will discuss a few in this section.

```bash
docker pull rewanthtammana/sigstore-the-easy-way:sha256-212dad360f60d13c80c7836df53ff98bb7f660bab5152baeba127a79a0df51f2.att
```

![download-artifact-docker](../images/download-artifact-docker.png)

## Set image

We can follow the steps from [this section](./sign-and-verify-with-key.md#set-image) to set the image. Let's ensure the `IMAGE` variable is set.

```bash
echo $IMAGE
```

![set-image-variable](../images/set-image-variable.png)

## Pre-requisite

Since we are trying to download the attested file from the registry, it's essential to complete the previous section. We can follow the same/similar steps to download signatures/blobs/sboms/etc.

In the [previous section](./attest-and-verify-artifacts.md#attest-and-push-the-sbom-to-oci-registry), the attested artifact is uploaded to the registry with `.att` extension.

![cosign-attest-sbom-ui](../images/cosign-attest-sbom-ui.png)

We can copy the artifact id from the UI. For me, it is, `rewanthtammana/sigstore-the-easy-way:sha256-212dad360f60d13c80c7836df53ff98bb7f660bab5152baeba127a79a0df51f2.att`

## Crane

[Crane](https://github.com/google/go-containerregistry/blob/main/cmd/crane/doc/crane.md) can be helpful for downloading these artifacts from registries.

Instead of `docker pull <image-sha>.sig`, we can use `crane pull <image-sha>.sig <file-location>` to save the signatures, attestations & sboms to local system.

```bash
crane pull rewanthtammana/sigstore-the-easy-way:sha256-212dad360f60d13c80c7836df53ff98bb7f660bab5152baeba127a79a0df51f2.att 212dad360f60d13c80c7836df53ff98bb7f660bab5152baeba127a79a0df51f2.att
```

![download-artifact-crane](../images/download-artifact-crane.png)

Let's analyze the file type.

```bash
file *.att
```

It's a POSIX tar archive. Let's extract the file to see the compressed files & data within them.

![download-crane-extract-tar](../images/download-crane-extract-tar.png)

As you can see, the attestation file contains 3 files - *sha256sum*, *manifest.json* & *another tar archive*. This guide aims to keep things simple, so I will stop here, but you can feel free to explore further.

## Cosign

We can download the attestation artifact with the below command,

```bash
cosign download attestation $IMAGE
```

![download-artifact-cosign](../images/download-artifact-cosign.png)

Similar to above, we can download the signature & SBOM of an image from the registry with `cosign download signature $IMAGE` & `cosign download sbom $IMAGE`.

## Cosign vs Crane for downloading artifacts

Crane downloads the complete artifact & upon extraction, we will find *manifest.json*, *sha256 file with architecture, layers, etc. information* & *another tar.gz file with appropriate contents*.

Cosign only downloads the attestation file associated with the image. If you are using tags like `latest` for your image & if that image gets overridden in the future, `cosign download` will not be able to download the previously uploaded attestation because the shasum value of the new image will be different. To be safe from these kinds of edge cases, we made sure in the [beginning](./sign-and-verify-with-key.md#set-image) to use shasum values instead of tags.

## Debugging tip

When you try downloading the objects, you might get an error, *MANIFEST_UNKNOWN* or similar. It means the artifact isn't existing in the registry.

attestation - Uploads `.att` file to the registry
signature - Uploads `.sig` file to the registry
sbom - Uploads `.sbom` file to the registry
