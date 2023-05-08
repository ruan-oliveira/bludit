# Bludit Helm Chart

Pacote Helm para deploy do CMS Bludit

### Como usar

Deploy simples:

```yaml
helm install bludit -n bludit --create-namespace . 
```

Exemplo de deploy com TLS (Letsencrypt com Cert Manager) e persistência de volume:

1. Escreva em um arquivo o conteúdo abaixo:
  - Altere para o domínio desejado
  - Altere para o tamanho do volume desejado

```yaml
ingress:
  enabled: true
  className: "nginx"
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: demo.bludit.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: 
  - hosts:
    - demo.bludit.com
    secretName: bludit-tls

securityContext:
  runAsUser: 999

podSecurityContext:
  fsGroup: 997

persistence:
  enabled: true
  size: "1Gi"
```

Agora, podemos implementar o pacote:

```cmd
helm install bludit -n bludit --create-namespace -f /tmp/values-bludit.yaml bludit/
```

Neste exemplo, o conteúdo do values customizado foi escrito em /tmp/values-bludit.yaml. 
