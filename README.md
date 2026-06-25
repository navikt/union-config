# Union config action

Denne actionen lar deg konfigurere service accounts og egress regler for trafikk fra Union tasks. Spesifiser en ressurs av typen `UnionTeamServiceAccount` som under:

`utsa.yaml`:
```yaml
apiVersion: data.nav.no/v1alpha1
kind: UnionTeamServiceAccounts
metadata:
  name: union-team-eksempel
spec:
  domain: development
  project: eksempel
  serviceAccounts:
  - externalAllowlist:
    - host: hest.no
    internalAllowlist:
    - host: a01dbfl039.adeo.no
    name: sa1
  - externalAllowlist:
    - host: example.com
    name: sa2
```

>Prosjekt over er `Union prosjekt` og domene er `development, staging eller production`

Denne kan så applies til clusteret med gitub action som eksempelet under:

```yaml
name: Apply Union configuration

on:
  push:

jobs:
  apply-union-config:
    name: Apply Union configuration
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write

    steps:
      - name: Checkout
        uses: actions/checkout@df4cb1c069e1874edd31b4311f1884172cec0e10 # v6.0.3

      - uses: navikt/union-config@v2
        with:
          manifest: utsa.yaml
```
