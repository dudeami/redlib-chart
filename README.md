# Redlib Helm Chart

A Helm chart for easily deploying a [redlib][1] instance to a kubernetes
cluster.

## Configuration of redlib

You can configure the redlib instance using the following yaml options.

```yaml
redlib:
  address: "[::]"
  settings:
    sfwOnly: "off"
    banner: ""
    robotsDisableIndexing: "off"
    pushshiftFrontend: "undelete.pullpush.io"
    # To change port set $.service.port
    enableRSS: "off"
    fullURL: ""
  userSettings:
    theme: "system"
    frontPage: "default"
    layout: "card"
    wide: "off"
    postSort: "hot"
    commentSort: "confidence"
    blurSpoiler: "off"
    showNSFW: "off"
    blurNSFW: "off"
    useHLS: "off"
    hideHLSNotification: "off"
    autoplayVideos: "off"
    subscriptions: ""
    hideAwards: "off"
    disableVisitRedditConfirmation: "off"
    hideScore: "off"
    hideSidebarAndSummary: "off"
    fixedNavbar: "on"
```

### Address option

The default address is the local IPv6 address `"[::]"`. If your cluster does not
have IPv6 available, set this to `"0.0.0.0"`.

## Example configuration

The following configuration is a fairly standard implementation using the [Nginx
Ingres Controller][2] with [cert-manager][3] to handle the SSL certifications.

```yaml
image:
  pullPolicy: Always
  tag: "latest"

redlib:
  address: "0.0.0.0"
  userSettings:
    frontPage: "all"
    showNSFW: "on"
    blurNSFW: "on"

service:
  port: 8080

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: redlib.dudeami.win
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
  - secretName: redlib-dudeami-win-cert
    hosts:
      - redlib.dudeami.win
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    cert-manager.io/cluster-issuer: cluster-issuer

```

[1]: https://github.com/redlib-org/redlib
[2]: https://kubernetes.github.io/ingress-nginx/
[3]: https://cert-manager.io/