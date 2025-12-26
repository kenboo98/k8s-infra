# In-Cluster Docker Registry 

### Registries.yaml
This file points the cluster to the internal registry hosted within it. Any images that start with registry.kenta.ca will fetch it from the local registry. The registry itself does not start with that so will default to fetching the image from docker.io