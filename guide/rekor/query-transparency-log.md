# Query transparency log

This is a short handbook on how to query the transparency log i.e rekor in this case for infomration.


## Rekor-cli with tlog index

In this guide, we discussed about [keyless signing](../cosign/sign-and-verify-without-key.md). If you have observed the output after signing the artifact with one of the OIDC providers, you will be a tlog entry.

```bash
export IMAGE=rewanthtammana/aava #Change the image name accordingly
COSIGN_EXPERIMENTAL=1 cosign sign $IMAGE
...
tlog entry created with index: 7398058
...
```

We can use the tlog index to query the rekor instance to verify the signature uploaded to rekor.

```bash
rekor-cli get --log-index 7398058
```

## Rekor-cli with email

We can list all the objects signed by a specific person/entity.

```bash
rekor-cli search --email testinguser883@gmail.com
```

We can use the above UUIDs to gather more information on the signatures/artifacts that are uploaded to the transparency log.

## Rekor-cli with shasum of artifact

You can query the transparency log with tlog index or uuid. In the output, you will find the sha256sum of the object. We can even use that value to query the transprency log.

```bash
rekor-cli get --log-index 7398935 --format json | jq '.Body.HashedRekordObj.data.hash.value'
```

Once we have the sha value from above, we can use that to search the transparency log for instances of it.

```bash
# Replace the sha value with your id
rekor-cli search --sha 6fc9188728405c5465fd08e96fd1c25506af3818403c4c3d9689536b82780092
```

## Curl request

This is covered in detail as part of [upload artifacts to public rekor](./upload-artifacts-to-public-rekor.md#curl-request) section.

## Rekor-cli with UUID

This is covered in detail as part of [upload artifacts to public rekor](./upload-artifacts-to-public-rekor.md#rekor-cli-with-uuid) section.

## Rekor-cli with artifact

This is covered in detail as part of [upload artifacts to public rekor](./upload-artifacts-to-public-rekor.md#rekor-cli-with-artifact) section.
