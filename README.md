# authenticate-google-artifact-registry-action

GitHub Action which allows use of private npm repositories in Google Artifact Registry.

See: [Enabling keyless authentication from GitHub Actions](https://cloud.google.com/blog/products/identity-security/enabling-keyless-authentication-from-github-actions)

Example:

```yaml
  - name: Authenticate Google Artifact Registry
    uses: sknups/authenticate-google-artifact-registry-action@v1
    with:
      workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
      service_account: 'my-service-account@my-project.iam.gserviceaccount.com'
```

Your project should have a `.npmrc` file in the root, e.g.:

```npmrc
@sknups:registry=https://europe-west2-npm.pkg.dev/sknups/npm/
@sknups-internal:registry=https://europe-west2-npm.pkg.dev/sknups/npm-internal/
//europe-west2-npm.pkg.dev/sknups/npm-internal/:always-auth=true
engine-strict=true
```
