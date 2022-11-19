# Analyzing code signing certificates

The experimental feature of cosign allows us to use keyless signing feature.

Let's extract the output certificate for analysis.

## Generate & export certificate

```bash
IMAGE=rewanthtammana/aava
COSIGN_EXPERIMENTAL=1 cosign sign $IMAGE --output-certificate fulcio.crt
COSIGN_EXPERIMENTAL=1 cosign verify $IMAGE
```

## Inspect certificate

We can use the [step](https://smallstep.com/docs/step-cli/installation) tool for analyzing the certificate.

```bash
step certificate inspect fulcio.crt --format json
```

### Extract certificate validity

As sigstore claims, you can see this is a short-lived certificate with 10 minutes validity. The artifact is signed with this short-lived certificate & after 10 minutes the certificate becomes unusable.

```bash
step certificate inspect fulcio.crt --format json | jq -r '.validity'
```

<screenshot here>

### Identify signer infomration

Analyze the certificate to identify the singer information.

```bash
step certificate inspect fulcio.crt --format json | jq -r '.extensions.subject_alt_name.email_addresses'
```

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
