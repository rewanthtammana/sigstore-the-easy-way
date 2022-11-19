# Attach artifacts

In this example, we will attach [SBOMs](../sbom/readme.md) to images. The workflow is similar for attaching signatures/attestations to an artifact.

```bash
IMAGE=rewanthtammana/aava
docker tag nginx $IMAGE
docker push $IMAGE
```

## Generate SBOM for the image

The [SBOM section](../sbom/generate.md) explains more about SBOMs, etc.

```bash
trivy i --format cosign-vuln $IMAGE > image.sbom
```

<Screenshot of terminal here>


## Attach SBOM with the image

We will attach the above generated SBOM to the image & push it to registry.

```bash
cosign attach --sbom image.sbom $IMAGE
```

<Screenshot of terminal & dockerhub here>


## NOTE

The `attach` feature only uploads the provided artifact to the registry. It doesn't sign the artifact, so anyone can tamper it & there's no way for verification. To sign artifacts like SBOMs, etc, we have to [attest](./attest-and-verify-artifacts.md) the artifact instead of attaching it.

