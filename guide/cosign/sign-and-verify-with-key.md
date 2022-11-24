# Sign and verify with key

## Generate key pair

The first step towards signing your software is to generate a key-pair. In the background, cosign uses ECDSA (Elliptic Curve Digital Signature Algorithm) & elliptic.P256 cryptography to generate key-pair, but let's not worry about that & keep things simple.

```bash
cosign generate-key-pair
```

![cosign-generate-key-pair](../images/cosign-generate-key-pair.png)

```bash
ls -la
```

![cosign-list-files](../images/cosign-list-keys.png)

## Set image

We can follow the steps from [this section](./set-image.md) to set the image. Let's ensure the `IMAGE` variable is set.

```bash
echo $IMAGE
```

![set-image-variable](../images/set-image-variable.png)

## Sign the artifact

This will sign that thing & push the signature to the OCI registry. For this example, make sure you are logged in to dockerhub from cli.

```bash
cosign sign --key cosign.key $IMAGE
```

![cosign-sign-the-artifact](../images/cosign-sign-the-artifact.png)

## Verify the artifact

```bash
cosign verify --key cosign.pub $IMAGE
```

![cosign-verify-the-artifact](../images/cosign-verify-the-artifact.png)

Observe the list of checks performed above in this method,

```
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - The signatures were verified against the specified public key
```

There's no involvement of [transparency log](../rekor/index.md) or [short lived certificates](../fulcio/index.md) in this method. We will learn about them in the later sections.

