# ZEIT ONLINE Terraform ``baseproject``connector

---

ℹ️ This Action is used internally by the ZEIT ONLINE organization and probably not useful outside of this specific context.

---

This composite action fetches environment-specific infrastructure infos from a Terraform ``baseproject`` instance. It (optionally) also handles Authentication against Cloud infrastructure, such as GCE, GKE, GCR and Vault.


## Example Usage

```yaml
jobs:
    build:
        # ...
        steps:
            # ...
            - name: Baseproject
              uses: ZeitOnline/gh-action-baseproject@v0.1.0
              with:
                project_name: ${{ env.PROJECT }}
                environment: ${{ env.ENVIRONMENT }}
                gke_auth: true
                gcr_auth: true
                vault_get_secrets: |
                  zon/v1/my/secret my-secret;
                vault_export_token: false
            # ...
```

## Reference

Here are all the inputs available through `with`:

| Input                | Description                                                                       | Default | Required |
| -------------------- | --------------------------------------------------------------------------------- | ------- | -------- |
| `project_name`       | The Name (`project_name`) of the ZON baseproject                                  |         | ✔        |
| `environment`        | The Environment in which the workflow runs                                        |         | ✔        |
| `google_auth`        | Authenticate to Google Cloud                                                      | `false` |          |
| `ghe_auth`           | Authenticate to GKE (Google Kubernetes Engine)                                    | `false` |          |
| `gcr_auth`           | Authenticate to GCR (Googke Container Registry)                                   | `false` |          |
| `vault_get_secrets`  | Vault secrets to get (see: https://github.com/hashicorp/vault-action#key-syntax)  |         |          |
| `vault_export_token` | Get a Vault Token and export it as VAULT_TOKEN                                    | `false` |          |
