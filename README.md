# ZEIT ONLINE Terraform ``baseproject``connector

---

ℹ️ This Action is used internally by the ZEIT ONLINE organization and probably not useful outside of this specific context.

---

This composite action fetches environment-specific infrastructure infos from a Terraform ``baseproject`` instance. It (optionally) also handles Authentication against Cloud infrastructure, such as GCE, GKE, GCR and Vault.


## Example Usage


### Authenticate against GKE and GAR

Set up authentication against the Cluster that is configured for the project-environment.
Also authenticate against GAR. All following steps will be fully authenticated to K8s/GAR.

```yaml
jobs:
    build:
        # ...
        steps:
            # ...
            - name: Baseproject
              uses: ZeitOnline/gh-action-baseproject@v0
              with:
                project_name: ${{ env.PROJECT }}
                environment: ${{ env.ENVIRONMENT }}
                gke_auth: true
                gar_docker_auth: true
            # ...
```

### Fetch secrets from Vault

The `gh-action-baseproject` Action outputs all information neccessary for authenticating against Vault with the
`jwt` auth method:

```yaml
jobs:
    build:
        # ...
        steps:
            - name: Baseproject
              uses: ZeitOnline/gh-action-baseproject@v0
              with:
                project_name: ${{ env.PROJECT }}
                environment: ${{ env.ENVIRONMENT }}

            - name: Retrieve secrets from Vault
              id: vault-secrets
              uses: hashicorp/vault-action@v2.4.0
              with:
                method: jwt
                url: ${{ steps.baseproject.outputs.vault_addr }}
                path: ${{ steps.baseproject.outputs.gha_vault_path }}
                role: ${{ steps.baseproject.outputs.gha_vault_role }}
                secrets: |
                  zon/v1/path/to/my-secret my-secret;

            - name: Use Vault secret
              run: echo ${{ steps.vault-secrets.outputs.my-secret }}
```


## Reference

Here are all the inputs available through `with`:

| Input                | Description                                                                       | Default | Required |
| -------------------- | --------------------------------------------------------------------------------- | ------- | -------- |
| `project_name`       | The Name (`project_name`) of the ZON baseproject                                  |         | ✔        |
| `environment`        | The Environment in which the workflow runs                                        |         | ✔        |
| `google_auth`        | Authenticate to Google Cloud                                                      | `false` |          |
| `gke_auth`           | Authenticate to GKE (Google Kubernetes Engine)                                    | `false` |          |
| `gcr_auth`           | Authenticate to GCR (Google Container Registry)                                   | `false` |          |
| `gar_docker_auth`    | Authenticate to GAR (Google Artifact Registry) for Docker                         | `false` |          |
| `python_registry`    | Setup dependencies for Python Registry (Google Artifact Registry)                 | `false` |          |
| `vault_export_token` | Get a Vault Token and export it as VAULT_TOKEN                                    | `false` |          |
