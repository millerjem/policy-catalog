# policy-catalog

Sample policy catalog repo for testing `policyctl` Git-backed inputs.

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
