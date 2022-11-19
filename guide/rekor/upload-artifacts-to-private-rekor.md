# Upload artifacts to private rekor

As you have seen the previous section, [upload aartifacts to public rekor](./upload-artifacts-to-public-rekor.md), let's say for some reason, you can't use public rekor instance because you have sensitive/private information. In this case, it's possible to spin up your rekor instance locally.

```bash
git clone https://github.com/sigstore/rekor.git
cd rekor
docker-compose up
```

The docker-compose has to start multiple services like redis, mysql, trillan signer, trillian server & rekor server. So, it will take a while to spin up. Be patient. The service exposes port 3000.

To use the local/private rekor instance, `--rekor_server` flag must be passed to the command.

You can follow the previous guide, [upload aartifacts to public rekor](./upload-artifacts-to-public-rekor.md), with an additional parameter to rekor-cli, `--rekor_server http://localhost:3000` for it to work. You can read detailed information on the commands from the [previous section](./upload-artifacts-to-public-rekor.md).

## Sign and upload artifact to rekor

Detailed information on the commands, [here](./upload-artifacts-to-public-rekor.md#sign-and-upload-artifact-to-rekor)

```bash
ssh-keygen -t ed25519 -f id_ed25519 -q -N ""
ssh-keygen -Y sign -f id_ed25519 -n file image.sbom
rekor-cli upload --rekor_server http://localhost:3000 --artifact image.sbom --signature image.sbom.sig --public-key id_ed25519.pub --pki-format ssh
```

## Query rekor

Detailed information on the commands, [here](./upload-artifacts-to-public-rekor.md#query-rekor)

```bash
UUID=<your-uuid>
curl http://localhost:3000/api/v1/log/entries/$UUID 
rekor-cli get --uuid $UUID --rekor_server http://localhost:3000
rekor-cli search --rekor_server http://localhost:3000 --artifact image.sbom
```
