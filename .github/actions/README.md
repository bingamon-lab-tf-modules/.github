# Shared composite actions

Composite actions shared by every repository in the `bingamon-lab-tf-modules`
organization. Reference them pinned to a commit SHA:

```yaml
- id: setup_devenv
  uses: bingamon-lab-tf-modules/.github/.github/actions/setup-devenv@<sha> # trunk
  with:
    ci_app_id: ${{ secrets.CI_APP_ID }}
    ci_app_private_key: ${{ secrets.CI_APP_PRIVATE_KEY }}
    cachix_cache_name: ${{ vars.CACHIX_CACHE_NAME }}
    cachix_auth_token: ${{ secrets.CACHIX_AUTH_TOKEN }}
```

Dependabot's `github-actions` ecosystem bumps the pin like any other action.

## Actions

| Action | Purpose |
| --- | --- |
| `setup-devenv` | Install Nix, enable Cachix, install devenv. Optional App token for the Nix GitHub access token. |
| `app-token` | Mint a GitHub App installation token with fallback to `GITHUB_TOKEN`. Use for creating pull requests, tags and releases that must trigger downstream workflows. |
| `commit-status` | Report a commit status from a step outcome, replacing the copy-pasted `gh api .../statuses/...` block. |

## Why an App token

Pull requests, tags and releases created with the default `GITHUB_TOKEN` do
not trigger downstream workflows, and runs triggered by `github-actions[bot]`
are held in "workflows awaiting approval". Tokens minted from the
`enterprise-ci` GitHub App (org secrets `CI_APP_ID` / `CI_APP_PRIVATE_KEY`)
have neither problem; each token is scoped to the calling repository at mint
time.
