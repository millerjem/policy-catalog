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
