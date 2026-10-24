# How This Site Is Published

## Overview

This site is built with [Hugo][hugo] from the [smit-io/docs][docs-repo] repo. The
rendered HTML is **not** committed there — a CI workflow builds it and force-pushes
the output as a single fresh commit to [smit-io/smit-io.github.io][pages-repo],
which GitHub Pages serves at smitchoksi.com. No build-artifact history ever
accumulates: the Pages repo always holds exactly one commit.

```
docs repo (source) ──hugo build──▶ public/ ──force-push──▶ smit-io.github.io ──▶ GitHub Pages
```

The same workflow (`.github/workflows/publish.yml`) runs two ways:

| Mode | Trigger | Cost |
|---|---|---|
| Local CI | `make local-ci-run` (via [act][act], Docker) | Free, uses your `gh` CLI token |
| GitHub CI | Push to `main`, daily cron (09:00 UTC), or `make gh-ci-run` | Free on public repos |

Both are always available — enabling hosted CI does not disable the local path.

## Scheduled publishing

Hugo skips posts whose `date` is in the future when building for production, while
the drafts build (`make build-drafts`, `-F` flag) includes them. Combined with the
daily cron trigger on the hosted workflow, front matter alone controls a post's
lifecycle:

| Front matter | State | Visible in |
|---|---|---|
| `draft: true` | Draft | Drafts build only |
| `draft: false`, future `date` | Scheduled | Drafts build only |
| `draft: false`, past `date` | Live | Everywhere |

To schedule a post: set `draft: false` and give it the intended publish date
(e.g. `date: 2026-09-15T09:00:00Z`). It goes live on the first build after that
moment — the next morning's cron run, or any push to `main` in between. No flag
flipping, no extra workflow: the cron run is an ordinary rebuild of `main`.

GitHub cron caveats: schedules run in **UTC**, may start up to ~15 minutes late,
and GitHub auto-disables them after 60 days without repo activity (re-enable with
`make gh-ci-enable`).

## Hosted CI setup

Hosted runs push to a *different* repo than the one the workflow lives in, so the
default `GITHUB_TOKEN` is not enough — a personal access token is required.

{{% steps %}}

### Create a fine-grained PAT

Go to **github.com → Settings → Developer settings → [Fine-grained tokens][pat-new]**:

- Repository access → **Only select repositories**: `smit-io/docs`,
  `smit-io/hextra`, `smit-io/smit-io.github.io`
- Repository permissions → **Contents: Read and write**

Least privilege: the token can touch these three repos and nothing else.

### Add it as a repo secret

```shell
gh secret set PUBLISH_TOKEN --repo smit-io/docs
```

Paste the token at the prompt — it never lands in shell history or the chat.

### Enable the workflow

```shell
make gh-ci-enable   # pushes to main now trigger hosted publishes
make gh-ci-status   # check state + recent runs
make gh-ci-disable  # back to local-only, secret stays
```

{{% /steps %}}

## How the workflow authenticates

`.gitmodules` references the theme submodule over SSH. The workflow's first step
rewrites SSH URLs to HTTPS with the token:

```shell
git config --global url."https://x-access-token:${PUBLISH_TOKEN}@github.com/".insteadOf "git@github.com:"
```

One token then covers the submodule clone *and* the publish push — on both GitHub
runners and local act runs (where `make local-ci-run` injects your `gh` CLI token
instead of the secret).

## Notes

- Actions minutes are **free and unlimited on public repos** (standard runners).
  If the source repo ever goes private: free plan includes 2,000 min/month and a
  publish run takes ~2 minutes.
- The force-push means the Pages repo history is disposable by design — never
  commit anything there by hand.
- `CNAME` lives in the source repo (`static/CNAME`), so Hugo re-emits it on every
  build and the custom domain survives the force-push.

[hugo]: https://gohugo.io/
[act]: https://github.com/nektos/act
[docs-repo]: https://github.com/smit-io/docs
[pages-repo]: https://github.com/smit-io/smit-io.github.io
[pat-new]: https://github.com/settings/personal-access-tokens/new

