# Upload artifacts to public rekor

Sigstore has a [public rekor instance](https://rekor.sigstore.dev) that is used to upload the artifacts.

In this example, we will [generate an SBOM](../sbom/generate.md), sign & upload it to public rekor instance for transparency & immutability.

## Sign and upload artifact to rekor

To sign the sbom, we have to generate cryptographic keys. For this example, let's use ed25519 algorithm to generate asymmetric keys.

```bash
ssh-keygen -t ed25519 -f id_ed25519
```

The above step prompts for passphrase but if you want to pass the value through CLI, `-N` flag can be used.

```bash
ssh-keygen -t ed25519 -f id_ed25519 -q -N ""
```

If you already have SBOM, it's okay. If not, follow the steps, [here](../sbom/generate.md) to generate SBOM.

Once we have the artifact, let's sign it with the ed25519 private key generated above.

```bash
ssh-keygen -Y sign -f id_ed25519 -n file image.sbom
rekor-cli upload --artifact image.sbom --signature image.sbom.sig --public-key id_ed25519.pub --pki-format ssh
```

## Query rekor

There are multiple ways to extract the information from rekor. Whenever an object is uploaded to rekor instance, an UUID is returned. We can use the UUID to query information.

### Curl request

You can make a curl request to the UUID to gather information of the artifact.

```bash
curl https://rekor.sigstore.dev/api/v1/log/entries/24296fb24b8ad77a2181566581dcd340ea4cf3ba041bd28cc560b91e80e73c5280991b4d7440a05f
```

### Rekor-cli with UUID

```bash
rekor-cli get --uuid 24296fb24b8ad77a2181566581dcd340ea4cf3ba041bd28cc560b91e80e73c5280991b4d7440a05f
```

### Rekor-cli with artifact

The below command will throw an error because we never uploaded `abcd.txt` to rekor.

```bash
echo "helloworld" > abcd.txt
rekor-cli search --artifact abcd.txt
```

Instead, if you search with the above generated SBOM object, you will get a result.

```bash
rekor-cli search --artifact image.sbom
```

Once you have the UUID, you can use `rekor-cli get` or `curl` to query the endpoint to gather more information.
