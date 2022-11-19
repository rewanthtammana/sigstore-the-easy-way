# Upload artifacts to private rekor

As you have seen the previous section, [upload aartifacts to public rekor](./upload-artifacts-to-public-rekor.md), let's say for some reason, you can't use public rekor instance because you have sensitive/private information. In this case, it's possible to spin up your rekor instance locally.

```bash
git clone https://github.com/sigstore/rekor.git
cd rekor
docker-compose up
```

![rekor-private-docker-compose-start](../images/rekor-private-docker-compose-start.png)

The docker-compose has to start multiple services like redis, mysql, trillan signer, trillian server & rekor server. So, it will take a while to spin up. Be patient. The service exposes port 3000.

To use the private instance of rekor, the same steps can be followed from the [upload artifacts to public rekor](./upload-artifacts-to-public-rekor.md) guide along with an additional parameter,`--rekor_server` to the rekor-cli.

