# authenticate-google-artifact-registry-action

GitHub Action which allows use of private npm repositories in Google Artifact Registry.

See: [Enabling keyless authentication from GitHub Actions](https://cloud.google.com/blog/products/identity-security/enabling-keyless-authentication-from-github-actions)

---

## Basic Usage

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

---

## Credentials file

This action delegates to `google-github-actions/auth@v2` to authenticate with Google Cloud.

That action writes a credentials file to the local filesystem, the location of which is stored in these environment variable:

- `CLOUDSDK_AUTH_CREDENTIAL_FILE_OVERRIDE`
- `GOOGLE_APPLICATION_CREDENTIALS`
- `GOOGLE_GHA_CREDS_PATH`

The credentials file will be deleted at the end of the job.

To delete the credentials file earlier, so it is not available to subsequent steps in the job, you can set `erase_credentials`:

```yaml
  - name: Authenticate Google Artifact Registry
    uses: sknups/authenticate-google-artifact-registry-action@v1
    with:
      workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
      service_account: 'my-service-account@my-project.iam.gserviceaccount.com'
      erase_credentials: true
```

The environment variables will still be set, but their value will be a non-existent file.
