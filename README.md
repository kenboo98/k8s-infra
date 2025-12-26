# k8s-infra

## What to do for setup
### LoadBalancer
A hetzner load balancer exists to send traffic to each node

### Traefik
Traefik was already set up so just need to add routers. See `trafik/` for details.

### Internal IP needs to be used for kube system commmunication
```
kubectl get nodes -o wide
```

If `INTERNAL-IP` shows public IPs, you need to tell k3s to use the private interface.

**For k3s**, edit the service file:

On control plane, edit `/etc/systemd/system/k3s.service` and add to the `ExecStart` line:
```
--node-ip=<private-ip> --advertise-address=<private-ip> --flannel-iface=<private-interface>
```

On workers, edit `/etc/systemd/system/k3s-agent.service`:
```
--node-ip=<private-ip>
