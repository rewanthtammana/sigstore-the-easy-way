# Analyzing code signing certificates

The experimental feature of cosign allows us to use keyless signing feature.

Let's extract the output certificate for analysis.

## Set image

We can follow the steps from this [section](./sign-and-verify-with-key.md#set-image), to set image. Just make sure you have $IMAGE set appropriately.

```bash
echo $IMAGE
```

![set-image-variable](../images/set-image-variable.png)

## Generate & export certificate

In the [keyless signing section](../cosign/sign-and-verify-without-key.md), we saw it's possible to sign the artifacts/images by logging into the OIDC providers like google/microsoft/github/etc. Let's save the certificate that's being used by cosign for signing for analysis.

```bash
COSIGN_EXPERIMENTAL=1 cosign sign $IMAGE --output-certificate fulcio.crt
ls -lh fulcio.crt
```

![fulcio-save-cert](../images/fulcio-save-cert.png)

## Inspect certificate

We can use the [step](https://smallstep.com/docs/step-cli/installation) tool for analyzing the certificates. The json format of the output makes it easier for parsing & we use [jless](https://github.com/PaulJuliusMartinez/jless) for visualization.

```bash
step certificate inspect fulcio.crt --format json | jless
```

![fulcio-jless-analysis](../images/fulcio-jless-analysis.png)

As we can see above, there are lots of useful information that will help us to identify the signer & issuer of an artifact.

### Extract certificate validity

In the above screenshot, we can see the validity of the certificate as well. Sigstore is generating a short-lived certificate with only 10 minutes validity. The artifact is signed with this short-lived certificate & after 10 minutes the certificate becomes unusable.

```bash
step certificate inspect fulcio.crt --format json | jq -r '.validity'
```

![fulcio-certificate-validity](../images/fulcio-certificate-validity.png)

### Identify signer infomration

Analyze the certificate to identify the singer information.

```bash
step certificate inspect fulcio.crt --format json | jq -r '.extensions.subject_alt_name.email_addresses'
```

![fulcio-certificate-subject-email](../images/fulcio-certificate-subject-email.png)

## Further exploration

The root sigstore certificate signing certificates are usually stored in `~/.sigstore/root` directory. Feel free to explore & inspect the certificates for deeper understanding.

```
 ~/.sigstore/root$ tree
.
├── targets
│   ├── artifact.pub
│   ├── ctfe.pub
│   ├── ctfe_2022.pub
│   ├── fulcio.crt.pem
│   ├── fulcio_intermediate_v1.crt.pem
│   ├── fulcio_v1.crt.pem
│   ├── rekor.0.pub
│   └── rekor.pub
└── tuf.db
    ├── 000004.ldb
    ├── 000033.ldb
    ├── 000092.ldb
    ├── 000123.log
    ├── CURRENT
    ├── CURRENT.bak
    ├── LOCK
    ├── LOG
    └── MANIFEST-000124
```
