# OpenDroneMap on Kubernetes using Kustomize

The following is a kustomize base for deploying OpenDroneMap. The following components are deployed:

- WebODM
- NodeODM
- PostGIS
- Redis

See `base/` directory for the kustomize base.

For a simple deployment in the `odm` namespace, run the following.

First, preview all the resources that will be created:

```bash
kubectl kustomize overlays/basic
```

If that all looks good: 

```bash
kubectl apply -k overlays/basic
kubectl port-forward svc/webodm 8000:8000 -n odm
```

Then navigate to `http://localhost:8000` to access the webodm interface. You will need to create the account and then add a node to start processing tasks. The node address is `nodeodm:3000`.

See `overlays/hugepages-ingress` for an example of how to control resource usage (such as hugepages) and expose the webodm interface via an ingress. That example uses the caddy ingress controller and automatically provisions a TLS certificate using letsencrypt.


# TODO
- [ ] Automatically add NodeODM instances to the WebODM interface
- [ ] Make storage for webodm persistent (right now restarting the webodm instance will lose all your work)
- [ ] Use secrets for redis and postgres passwords
- [ ] Do not run as a privileged user, fixing `Warning: would violate PodSecurity...`
