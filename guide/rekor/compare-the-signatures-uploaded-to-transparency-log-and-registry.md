# Compare the signatures uploaded to registry and transparency log

As we discussed in [keyless signing](../cosign/sign-and-verify-without-key.md), the signature is uploaded to the transparency log before pushing to the registry.

To keep this simple, let's save the signature of an artifact (docker image in this case) before uploading it the registry. We can use `--output-signature` flag to perform this action.

## Get the tlog entry and signature during keyless signing

```bash
COSIGN_EXPERIMENTAL=1 cosign sign $IMAGE --output-signature image.sig
...
tlog entry created with index: 7398935
...
```

The signature is uploaded to the registry & it's stored in `image.sig` on your local filesystem. Alternatively, you can use one of the methods mentioned in [download artifacts](../cosign/download-artifacts.md) section to get the signature later.

```bash
cat image.sig
MEUCIAnxczctngv4pkJFwu/LyAwaF7snba8rndD4lUERutTPAiEAhIb7KLmPIuOTBWiaONumHOygVK3cPiCdBkl+IXAbGNg=
```

## Get the signature from transparency log rekor

Visit, [query transparency log](./query-transparency-log.md) section to read more on how to query rekor.

To compare it, we can extract the `tlog index` from the above ouptut & query the public/private rekor instance accordingly.

```bash
export tlogindex=7398935
rekor-cli get --log-index $tlogindex
```

We can parse the output fields easily when we print the output in json format.

```bash
rekor-cli get --log-index $tlogindex --format json | jq '.Body.HashedRekordObj.signature.content'
"MEUCIAnxczctngv4pkJFwu/LyAwaF7snba8rndD4lUERutTPAiEAhIb7KLmPIuOTBWiaONumHOygVK3cPiCdBkl+IXAbGNg="
```

As you can see from the above, the values extracted from transparency log with `rekor-cli` & the signature from the registry is the same. In this way, if required, we can verify the signatures & integrity of the artifacts with case.
