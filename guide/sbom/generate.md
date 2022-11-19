# Generate SBOM

There are multiple tools that help us to generate SBOMs for docker images, OCI artifacts, SIF images, filesystems, etc.

## [Docker](https://github.com/docker/cli)

```bash
docker sbom $IMAGE > image.sbom
```

Docker CLI uses [syft](https://github.com/anchore/syft) in the background. For extensive usage & customizations, we can download syft.

## [Syft](https://github.com/anchore/syft)

```bash
syft packages $IMAGE > image.sbom
```

## [Trivy](https://github.com/aquasecurity/trivy)

The `cosign-vuln` format is a custom type created by trivy to store SBOM data along with the list of vulnerabilities & associated CVEs. This is really helpful to review the list of vulnerabilities at the time of packaging & now.

```bash
trivy i --format cosign-vuln $IMAGE > image.sbom
```

<Screenshot of terminal here>
