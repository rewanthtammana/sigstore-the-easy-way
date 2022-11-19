# Sign and verify with key

## Generate key pair

The first step towards signing your software is to generate a key-pair. In the background, cosign uses ECDSA (Elliptic Curve Digital Signature Algorithm) & elliptic.P256 cryptograhy to generate keypair but let's not worry about that & keep things simple.

```bash
cosign generate-key-pair
```

```bash
ls -la
```

## Set image

```bash
# Replace it the user, image & tag name accordingly
IMAGE=rewanthtammana/aava
```

```bash
docker pull nginx
docker tag nginx $IMAGE
```

## Sign the artifact

This will sign that thing & push the signature to the OCI registry. For this example, make sure you are logged in to dockerhub from cli.

```bash
cosign sign --key cosign.key $IMAGE
```

## Verify the artifact

```bash
cosign verify --key cosign.pub $IMAGE
```

