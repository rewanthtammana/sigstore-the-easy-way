# Download artifacts

Whenever the operations like signing & attesting of artifacts is performed, the signatures or attestations or SBOMs are uploaded directly to the registry.

## Pre-requisite

Post [attest-and-verify-sbom](./attest-and-verify-sbom.md) section, you will have your attestation uploaded to registry.

```bash
IMAGE=rewanthtammana/aavs
```

## Crane

Docker CLI doesn't help in this case to pull/read these special artifacts. When you try to pull the artifact using the docker command, it tries to un-tar the compressed file to an image format.

```bash
docker pull rewanthtammana/aavs:sha256-91d5b6827ff7f88e56ecac8e8ab9fa19e3f821b79e577a82d40ce613312dea8b.att
```

<Screenshot here>

[Crane](https://github.com/google/go-containerregistry/blob/main/cmd/crane/doc/crane.md) can be helpful to download these artifacts from registries.

Instead of `docker pull <image-sha>.sig`, we can use `crane pull <image-sha>.sig <file-location>` to save the signatures, attestations & sboms to local system.

<Screenshot of terminal here>

```bash
crane pull rewanthtammana/aavs:sha256-91d5b6827ff7f88e56ecac8e8ab9fa19e3f821b79e577a82d40ce613312dea8b.att 91d5b6827ff7f88e56ecac8e8ab9fa19e3f821b79e577a82d40ce613312dea8b.att
tar xvf 91d5b6827ff7f88e56ecac8e8ab9fa19e3f821b79e577a82d40ce613312dea8b.att # Extracts manifest.json, sha file & attestation
```

<Screenshot of terminal here>


## Cosign

We can download the attestation artifact with the below command,

```bash
cosign download attestation $IMAGE
```
<Screenshot of terminal here>

Similiar to above, we can download the signature & sbom of an image from the registry with `cosign download signature $IMAGE` & `cosign download sbom $IMAGE`.

## Cosign vs Crane for downloading artifacts

Crane downloads the complete artifact & upon extraction, we will find *manifest.json*, *sha256 file with architecture, layers, etc information* & *another tar.gz file with appropriate contents*.

Cosign checks for the object related to the image & it downloads just the file. If the image gets over-ridden in future, `cosign download` will not be able download the previous attestation because the shasum value of the previous image is different from the new image.

## Debugging tip

Just an FYI, sometimes when you do try to download the objects you might get an error, *MANIFEST_UNKNOWN* or similar. It means, the artifact isn't existing in the registry.

attestation - Uploads `.att` file to the registry
signature - Uploads `.sig` file to the registry
sbom - Uploads `.sbom` file to the registry
