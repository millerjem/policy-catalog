# policy-catalog

Sample policy catalog repo for testing `policyctl` Git-backed inputs.

## Placeholder Variables

This sample catalog intentionally uses placeholders instead of real account-specific values.

- `${AWS_PARTITION}`: AWS partition such as `aws`, `aws-us-gov`, or `aws-cn`
- `${AWS_REGION}`: deployment region such as `us-gov-west-1`
- `${AWS_ACCOUNT_ID}`: target AWS account ID
- `${VERTEX_KMS_KEY_ID}`: KMS key ID or UUID for VerteX-managed encryption
- `${LOG_SHIPPER_ROLE_NAME}`: role name for the log shipping assume-role target
- `${ECS_TASK_ROLE_NAME}`: ECS task role name used by the workload

Before using this catalog in a real environment, replace the placeholders with values that match your account and naming conventions.

## Recommended Variable Management

For real use, the more standard approach is:

- keep policy and preflight files as templates in Git
- keep environment-specific values in a YAML values file
- render concrete JSON and YAML per environment before running `policyctl`

Recommended structure:

```text
policy-catalog/
  values/
    gov.yaml
  templates/
    spectro-vertex-least-privilege.json.tmpl
    preflight.yaml.tmpl
  rendered/
    gov/
      spectro-vertex-least-privilege.json
      preflight.yaml
```

Why this is preferred over a raw `.env` file:

- easier to review in pull requests
- clearer for multi-environment catalogs
- less dependent on shell state
- better for CI pipelines

If you want the lightest-weight option, `.env` plus `envsubst` is still a reasonable fallback, but YAML values files are usually easier to maintain over time.

## Layout

- `environments/gov/preflight.yaml`: live fail-fast IAM preflight checks
- `policies/spectro-vertex-least-privilege.json`: Spectro Cloud VerteX least-privilege example policy bundle

## Test locally

```bash
./policyctl preflight \
  -role-name aws-k8s-role-worker \
  -git-repo /Users/john.miller/Documents/Codex/policyctl/policy-catalog \
  -git-ref main \
  -file environments/gov/preflight.yaml
```

```bash
./policyctl validate \
  -git-repo /Users/john.miller/Documents/Codex/policyctl/policy-catalog \
  -git-ref main \
  -policy-file policies/spectro-vertex-least-privilege.json
```
