# Hydra Oathkeeper Sidecar

Protects Hydra without a whole DB and separate service

## Deploy

### OpenShift

```oc new-app -f hydra.yaml -p HYDRA_PUBLIC_URL=https://hydra.example.com```