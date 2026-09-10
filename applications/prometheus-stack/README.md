# Prometheus stack

## Slack Alerts

Alertmanager sends alerts to Slack using the externally managed `alertmanager-slack` Secret in the `monitoring` namespace.

Create and seal the Secret with the Slack incoming webhook URL:

```sh
kubectl -n monitoring create secret generic alertmanager-slack \
  --from-literal=slack-webhook-url='https://hooks.slack.com/services/...' \
  --dry-run=client -o yaml \
  | kubeseal --format yaml > applications/prometheus-stack/templates/sealed-alertmanager-slack-secret.yaml
```

The generated SealedSecret must be committed before syncing the Prometheus stack application. The webhook URL is mounted at `/etc/alertmanager/secrets/alertmanager-slack/slack-webhook-url` and is referenced by Alertmanager through `api_url_file`.
