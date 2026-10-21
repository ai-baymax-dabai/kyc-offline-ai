# Repository Guidance

## CI Runner Policy

This repository is public. The organization self-hosted runner group is
intentionally configured with `allows_public_repositories=false`.

- Do not target `self-hosted` runners from this repository.
- Use GitHub-hosted runners for any future CI.
- Do not request that the organization runner group be opened to public
  repositories as part of a normal feature change.
- If self-hosted execution is ever required, stop and require a dedicated security
  review covering fork PRs, secrets, credentials, and machine isolation.
- Network-heavy jobs should still pin tool versions and use bounded timeouts.

Canonical organization runner policy:
`https://github.com/ai-baymax-dabai/PeerToPeer-net/blob/main/docs/ops/ci-runner-policy.md`
