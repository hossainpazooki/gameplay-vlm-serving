# No traverse AWS profile exists locally and the default credentials are stale

ts: 2026-09-10T04:45:19Z
commit: c6bafff0487349dc065b85ab7f1e255e4c3384b8
session: https://claude.ai/code/session_01LvzR5v7WVozJWMPtMG7M1F
status: verified
fact: The planned VM lane assumes two local AWS profiles, `traverse-platform` (the operational identity) and `traverse-provision` (the identity that creates IAM roles). Neither exists on this machine: only `default` and `kv-platform-admin` are configured, and `default` no longer authenticates. Nothing in the VM or EKS lane can start until the operator writes both profiles, so the very first planned command after the doc fixes will fail on credentials rather than on anything the plan controls. The operational policy the operator wrote grants `ec2, s3, ecr, ssm, logs, cloudwatch, servicequotas, pricing, ce` and read-only IAM, and grants no `eks:*`, no `tag:GetResources`, and no `elasticloadbalancing:Describe*`; the planned teardown probe therefore falls back to per-service listing and reports the services it cannot read as unevaluable rather than as empty.
basis: `aws configure list-profiles` printed `default` and `kv-platform-admin` only; `aws sts get-caller-identity` printed `An error occurred (InvalidClientTokenId) when calling the GetCallerIdentity operation: The security token included in the request is invalid.`; `aws sts get-caller-identity --profile traverse-platform` printed `The config profile (traverse-platform) could not be found`. The policy contents are quoted from the operator's own message in this session, not read from the account.
re-verify: aws configure list-profiles
