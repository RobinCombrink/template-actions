# template-actions

Template for a new reusable-workflow repo in the `actions-*` fleet. Repos born from this
template are enforced from the first commit: Conventional Commits linting, full-history
secret scanning, automatic semantic releases, and Dependabot propagation with update-type-
gated auto-merge.

## After creating a repo from this template

1. Replace this README (see any fleet repo for the expected shape) and add your reusable
   workflow under `.github/workflows/`.
2. Apply the merge policy and ruleset:

   ```sh
   gh repo edit <owner>/<repo> --enable-squash-merge=false --enable-merge-commit=false \
     --enable-rebase-merge --enable-auto-merge --delete-branch-on-merge
   gh api repos/<owner>/<repo>/rulesets --method POST --input .github/fleet-ruleset.json
   ```

3. Push a `feat:` commit — the first release (v1.0.0) cuts itself.

## What's inside

| File | Purpose |
| --- | --- |
| `.github/workflows/release.yml` | Calls [actions-release](https://github.com/RobinCombrink/actions-release): semantic-release on every push to main. |
| `.github/workflows/conventional-commits.yml` | Calls [actions-conventional-commits](https://github.com/RobinCombrink/actions-conventional-commits): commit-message linting on PRs and pushes. |
| `.github/workflows/gitleaks.yml` | Calls [actions-gitleaks](https://github.com/RobinCombrink/actions-gitleaks): full-history secret scan, SARIF to code scanning. |
| `.github/workflows/automerge.yml` | Calls [actions-automerge](https://github.com/RobinCombrink/actions-automerge): arms auto-merge on patch/minor Dependabot PRs. |
| `.github/dependabot.yml` | Daily action-pin updates with a 3-day cooldown. |
| `.github/fleet-ruleset.json` | Ruleset for `main`: linear history, required checks, no deletion/force-push; repo admins bypass. |
