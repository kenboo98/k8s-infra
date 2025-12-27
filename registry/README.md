# In-Cluster Docker Registry 

registry.kenta.ca

### How this works 
Images for this cluster need to be pushed to this registry. The registry uses the standard docker registry image. This just uses a standard persistent volume.

All traffic to the registry is routed through Hetzner load balancer and through the traefik ingress that looks for registry.kenta.ca. 

### Troubleshooting
- Make sure that you `docker login registry.kenta.ca` and that `cat ~/.docker/config.json` has the output below:
```
{
	"auths": {
		"https://registry.kenta.ca": {
			"auth": "a2VuYm9vOmtlbmJvbzE5OTg="
		},
		"registry.kenta.ca": {
			"auth": "BASE 64 USERNAME:PASSWORD"
		},
		"http://registry.kenta.ca": {
			"auth": "BASE 64 USERNAME:PASSWORD"
		}
	},
	"currentContext": "desktop-linux"
}
```
- Ensure that `REGISTRY_HTTP_HOST` is set with `https` or `http` properly since the docker client isn't smart enough to pick between the two. 
- Cloudflare may block these requests since they're usually multiple PUT requests with a hash. A security rule can be added in cloudflare under Security > Security Rules where all requests to registry.kenta.ca has the CF DDoS stuff turned off.
- Ensure that you docker build with `--platform linux/amd64` when building on macos 

### Possible improvements
- Setting up the registry to be backed up by S3 or a proper data store
- Improved authentication mechanisms
- CICD pipeline so that builds are automatically published.

### Registries.yaml
This file points the cluster to the internal registry hosted within it. Any images that start with registry.kenta.ca will fetch it from the local registry. The registry itself does not start with that so will default to fetching the image from docker.io.

