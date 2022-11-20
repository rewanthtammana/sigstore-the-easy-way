# Attach artifacts

In this example, we will attach [SBOMs](../sbom/index.md) to images. The workflow is similar for attaching signatures/attestations to an artifact.

## Set image

We can follow the steps from [this section](./sign-and-verify-with-key.md#set-image) to set the image. Let's ensure the `IMAGE` variable is set.

```bash
echo $IMAGE
```

![set-image-variable](../images/set-image-variable.png)


## Generate SBOM for the image

The SBOM contains the list of dependencies & vulnerabilities in it (depending on how its generated). [SBOM section](../sbom/generate.md#trivy) decrypts the below command & its output in great detail. It explains more about SBOMs as well.

```bash
trivy i --format cosign-vuln $IMAGE > image.sbom
```

![sbom-trivy-cosign-vuln-format](../images/sbom-trivy-cosign-vuln-format.png)

## Attach SBOM with the image

It's recommended to keep track of all the known vulnerabilities when committing/pushing an image. We will attach the above-generated SBOM to the image & push it to the registry.

```bash
cosign attach --sbom image.sbom $IMAGE
```

![cosign-attach-sbom](../images/cosign-attach-sbom.png)

We can see the SBOM artifact uploaded to the registry. In this case, it's dockerhub.

![cosign-attach-sbom-ui](../images/cosign-attach-sbom-ui.png)

## NOTE

The `attach` feature only uploads the provided artifact to the registry. It doesn't sign the artifact, so anyone can tamper with it & there's no way to verification. To sign artifacts like SBOMs, etc., we have to [attest](./attest-and-verify-artifacts.md) the artifact instead of attaching it.

