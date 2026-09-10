# A single-line HCL block may hold exactly one argument

ts: 2026-09-10T04:44:09Z
commit: c6bafff0487349dc065b85ab7f1e255e4c3384b8
session: https://claude.ai/code/session_01LvzR5v7WVozJWMPtMG7M1F
status: verified
fact: `variable "region" { type = string  default = "us-east-1" }` is not valid HCL. Terraform's single-line block syntax accepts one argument only, so every compact two-argument block (`variable ... { type = ... default = ... }`, `principals { type = ... identifiers = ... }`, `filter { name = ... values = ... }`, `access_config { authentication_mode = ... bootstrap_... = ... }`) is a parse error, not a style complaint. It reads as ordinary Terraform and survives review; 32 such blocks were written into this repository's planned modules before anything parsed them. `terraform fmt -check` exits 2 rather than reformatting, so format-only CI catches it, but only if the module is actually run through it.
basis: `printf 'variable "region" { type = string  default = "us-east-1" }\n' > bad.tf && terraform fmt -check -no-color bad.tf` printed `Error: Invalid single-argument block definition ... A single-line block definition must end with a closing brace immediately after its single argument definition.` and `fmt-exit=2`, on Terraform v1.15.0-dev. After expanding all 32 blocks, `terraform validate` on each planned module printed `Success! The configuration is valid.` (infra/provision, infra/vm, infra/eks).
re-verify: grep -cE '^variable "[a-z_]+" \{ type = [a-z]+ +default' ~/dev/traverse/docs/plans/2026-09-09-full-build.md
