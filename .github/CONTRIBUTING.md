# Contributing

Default contributing notes for repositories in this organization. A repository
that has its own `CONTRIBUTING.md` overrides this file entirely, so anything
here has to be true of every project.

## Preview deployments are opt-in

Pushing a branch won't build a Vercel preview. Previews were about half of all
build minutes across these projects, and almost all of them were on automated
branches nobody ever opened.

Want a preview URL? Put `[preview]` anywhere in the commit message:

```sh
git commit -m "feat(nav): new mobile menu [preview]"
```

If you already pushed, an empty commit does it:

```sh
git commit --allow-empty -m "[preview]"
```

Redeploying from the Vercel dashboard also works, since that skips the check
entirely. The match is a plain substring, so a commit message that just
mentions the marker will build too.

**Production isn't affected on any project.** Merges to the production branch
build and deploy exactly as before. A missing preview URL is this rule, not a
broken project, so check here before you go debugging a deploy.

Two projects currently build previews without the marker, each for a recorded
reason, and both will join the default when that reason clears: `uibrary.com`
and `townofharmony.org`.

### Changing the policy on a project

Find the live surface first, because there are two and they both read back
clean:

```sh
grep ignoreCommand vercel.json
```

If `vercel.json` sets `"ignoreCommand"`, that is the authority and the policy
lives in the script it names, usually `scripts/vercel-ignore-step.sh`. The
Vercel dashboard "Ignored Build Step" field is overridden and editing it does
nothing. On a project with no `ignoreCommand`, the dashboard field is live and
is held automatically, so a hand edit there gets reverted.

Getting this wrong has cost real time twice, in opposite directions: a
dashboard policy that was silently ignored for 20 hours while a repo script ran
instead, and a repo script that was never wired up and documented a policy its
project did not have.

## Commit messages

[Conventional Commits](https://www.conventionalcommits.org/): `feat:`, `fix:`,
`chore:`, `docs:`, `refactor:`, `perf:`, `test:`.

## Security

Don't open a public issue for a vulnerability. See `SECURITY.md` in the
repository, or mail the address listed there.
