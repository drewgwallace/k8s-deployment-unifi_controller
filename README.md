# Kubernetes Deployment

## Purpose
  Deploy Ubiquiti's Unifi controller to a single node with a TLS enabled nginx ingress at a subdomain.

----

## Execution

<pre>
    Edit unifi.yaml replacing the domain with what's appropriate for your environment:
    spec:
      rules:
      - host: unifi.<b>EXAMPLE.COM</b>
        http:
          paths:
          - backend:
              serviceName: unifi-service
              servicePort: 8443
      tls:
      - hosts:
        - unifi.example.com
        secretName: unifi.<b>EXAMPLE.COM</b>-secret
</pre>

  ### Variables:
<pre>
    # kubernetes.io/hostname: <b>NODE.EXAMPLE.COM</b> : This is an optional node selector to pair the deployment to a specific node.
    host: unifi.<b>EXAMPLE.COM</b>-secret : This is a kubernetes secret container a public certificate and private key.
    image: drewgwallace/unifi: Docker hub image of the unifi controller. [GitHub](https://github.com/drewgwallace/unifi-docker)
    hostPath:
          path: /opt/data/unifi/database : Path on node to database volume
          path: /opt/data/unifi/logs : Path on node to logfiles volume

</pre>
----

## Notes

+ This was built for a bare-metal install, the volume used for stateful persistence is of type hostPath. In a multi-node deployment the volumes will need to be replicated to multiple nodes or the deployment assigned to a specific node with nodeSelector.