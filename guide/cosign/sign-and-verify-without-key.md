# Sign and verify without key (keyless signing)

In the previous section, we have generated a key pair, it's stored on file system & used for signing & verification of artifacts. But the keyless signing feature from cosign allows us to sign & verify without having the need to store keys.

## Set image

```bash
IMAGE=rewanthtammana/aava
docker pull nginx
docker tag nginx $IMAGE
```

## Sign the artifact

This stage redirects you to a page to sign-in with your OIDC provider like Google, Github & Microsoft to login & sign the artifact. You can have your own OIDC configured but to keep things simple as possible, let's skip that.

```bash
COSIGN_EXPERIMENTAL=1 cosign sign $IMAGE
```

In the output, we can see the `tlog entry created with index: 7398058`. The index differs on every upload. We can visit [compare the signatures](../rekor/compare-the-signatures-uploaded-to-transparency-log-and-registry.md) section to read on how to query the tlog index & more.

## Verify the artifact

```bash
COSIGN_EXPERIMENTAL=1 cosign verify $IMAGE
```

The below checks were performed for verification

```
The following checks were performed on each of these signatures:
  - The cosign claims were validated
  - Existence of the claims in the transparency log was verified offline
  - Any certificates were verified against the Fulcio roots.
```

## Inspecting the signature

To identify the the issuer & signed owner, execute the below command

```bash
COSIGN_EXPERIMENTAL=1 cosign verify $IMAGE | jq -r '.[].optional| .Issuer + "-" + .Subject'
```
