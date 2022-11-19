# Attest and verify artifacts

In this example, we will attest [SBOMs](../sbom/readme.md). The workflow is similar for attesting signatures/attestations.

## Default steps

```bash
IMAGE=rewanthtammana/aavs

docker tag nginx $IMAGE
docker push $IMAGE
```

## Generate SBOM for the image

The [SBOM section](../sbom/generate.md) explains more about SBOMs, etc.

<Screenshot of terminal here>

```bash
trivy i --format cosign-vuln $IMAGE > image.sbom
```


## Attest and push the sbom to OCI registry

We will attest the generated `image.sbom` as source of truth, sign it with the private key & push it to the registry.

```bash
cosign attest --key cosign.key --predicate image.sbom $IMAGE
```

<Screenshot from dockerregistry of att file>

## Verify the sbom of the image from registry

In future, whenever you want to validate the sbom of the image, you can verify it from the registry.

```bash
cosign verify-attestation --key cosign.pub $IMAGE
```

## Extract artifact from the registry

If you want to extract the artifact information for whatever reason, the below command will help to read the information from registry.

```bash
cosign verify-attestation --key cosign.pub $IMAGE | jq -r .payload | base64 -D | jq .
```


<Screenshot from terminal here>
