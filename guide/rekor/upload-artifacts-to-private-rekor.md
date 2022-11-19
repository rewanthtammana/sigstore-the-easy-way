# Upload artifacts to private rekor

As you have seen the previous section, [upload aartifacts to public rekor](./upload-artifacts-to-public-rekor.md), let's say for some reason, you can't use public rekor instance because you have sensitive/private information. In this case, it's possible to spin up your rekor instance locally.

```bash
git clone https://github.com/sigstore/rekor.git
cd rekor
docker-compose up
```

![rekor-private-docker-compose-start](../images/rekor-private-docker-compose-start.png)

The docker-compose has to start multiple services like redis, mysql, trillan signer, trillian server & rekor server. So, it will take a while to spin up. Be patient. The service exposes port 3000.

To use the local/private rekor instance, `--rekor_server` flag must be passed to the cli.

You can follow the previous guide, [upload artifacts to public rekor](./upload-artifacts-to-public-rekor.md). The only change would be to pass an additional parameter, `--rekor_server http://localhost:3000` to the rekor-cli.

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
