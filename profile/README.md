# MFE Orchestrator

**The open-source control plane for micro frontends.** It stores every build
you publish, decides which version each environment serves, and hands your host
application the runtime configuration it needs — so releasing one micro
frontend never means rebuilding the shell.

🔗 **Website:** [mfe-orchestrator.dev](https://mfe-orchestrator.dev/) ·
📖 **Documentation:** [mfe-orchestrator.dev/documentation](https://mfe-orchestrator.dev/documentation/) ·
🖥️ **Hosted console:** [console.mfe-orchestrator.dev](https://console.mfe-orchestrator.dev/)

## Why it exists

Micro frontends promise independent deployment. Most setups do not deliver it,
because the host application still decides at build time what to load: the list
of remotes lives in the host repository, releasing one widget means rebuilding
the shell, and a rollback is a revert, a rebuild and a wait.

MFE Orchestrator moves that decision to runtime. Versions live in a registry,
every deployment is an immutable snapshot per environment, the Module
Federation configuration (Vite or Webpack) is generated for you, and rolling
back is re-activating the snapshot that was live before — no rebuild, no
pipeline wait. Artifacts stay wherever you need them: on the hub, in your own
S3 / Azure Blob / Google Cloud Storage bucket, on your CDN, or entirely
on-premise.

## Try it in 60 seconds

**Hosted console — nothing to install.** Start free at
[console.mfe-orchestrator.dev](https://console.mfe-orchestrator.dev/).

**Self-hosted — one container**, with MongoDB, Redis and Nginx bundled:

```bash
docker run -d --name mfe-orchestrator --restart unless-stopped \
  -p 8080:80 -v mfe-data:/data lory1990/mfe-orchestrator:all-in-one
```

Either way, the
[**10-minute quick start**](https://mfe-orchestrator.dev/documentation/docs/quick-start)
takes you from an empty project to a deployed micro frontend — and back with a
one-click rollback.

## The repositories

| Repository | What it is |
| --- | --- |
| [mfe-orchestrator](https://github.com/mfe-orchestrator/mfe-orchestrator) | The platform: backend, console and self-hosting assets (Docker, Compose, Terraform, Helm) |
| [documentation](https://github.com/mfe-orchestrator/documentation) | The documentation site — [mfe-orchestrator.dev/documentation](https://mfe-orchestrator.dev/documentation/) |
| [mfe-orchestrator.github.io](https://github.com/mfe-orchestrator/mfe-orchestrator.github.io) | The public website — [mfe-orchestrator.dev](https://mfe-orchestrator.dev/) |
| [design-system](https://github.com/mfe-orchestrator/design-system) | The shared React design system used by the console, the website and the docs |

**CI/CD integrations** — your pipeline gains one upload step:

| Repository | What it is |
| --- | --- |
| [github-action](https://github.com/mfe-orchestrator/github-action) | GitHub Action ([on the Marketplace](https://github.com/marketplace/actions/microfrontend-orchestrator-uploader)) |
| [gitlab-pipeline-stage](https://github.com/mfe-orchestrator/gitlab-pipeline-stage) | GitLab CI stage |
| [azure-dev-ops-pipeline-stage](https://github.com/mfe-orchestrator/azure-dev-ops-pipeline-stage) | Azure DevOps pipeline stage |
| [template-pipelines](https://github.com/mfe-orchestrator/template-pipelines) | Pipeline templates used by the generated repositories |

**Templates** — what the console scaffolds when you create a micro frontend:

| Repository | What it is |
| --- | --- |
| [template-vite-remote](https://github.com/mfe-orchestrator/template-vite-remote) / [template-vite-host](https://github.com/mfe-orchestrator/template-vite-host) | Vite Module Federation remote and host |
| [template-webpack-react-remote](https://github.com/mfe-orchestrator/template-webpack-react-remote) / [template-webpack-react-host](https://github.com/mfe-orchestrator/template-webpack-react-host) | Webpack Module Federation remote and host |
| [template-webcomponent-remote](https://github.com/mfe-orchestrator/template-webcomponent-remote) | Web Components remote |
| [webcomponent-dynamic-loader](https://github.com/mfe-orchestrator/webcomponent-dynamic-loader) | Runtime loader for Web Components remotes |

## Contribute

MFE Orchestrator is built in the open, and there is more than one way to move it
forward — writing TypeScript is only one of them.

### Spread the word

The project is young, and the hardest problem is being found by the teams who
need it. Star the
[main repository](https://github.com/mfe-orchestrator/mfe-orchestrator), share it
where micro frontend problems get discussed — Reddit, Hacker News, dev.to,
LinkedIn, your company Slack — write a post about the setup you ended up with, or
talk about it at a meetup. If you already run micro frontends in production, a
short honest write-up of how you release them is worth more than anything we can
put on a landing page.

### Write code

Every repository listed above accepts pull requests. Good places to start:

- issues labelled
  [good first issue](https://github.com/mfe-orchestrator/mfe-orchestrator/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22)
  and
  [help wanted](https://github.com/mfe-orchestrator/mfe-orchestrator/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)
  on the platform;
- the [documentation](https://github.com/mfe-orchestrator/documentation) — a page
  that confused you is a page worth fixing;
- the [templates](#the-repositories), if the framework or bundler you use is
  missing one.

The workflow is the usual fork → branch → conventional commit → pull request. The
[contributing section of the main README](https://github.com/mfe-orchestrator/mfe-orchestrator#contributing-)
covers the local development setup and the code quality rules the pipeline
enforces (Biome, commitlint, Lefthook).

### Build a CI/CD adapter

We ship adapters for GitHub Actions, GitLab CI and Azure DevOps. Everything else
— Jenkins, Bitbucket Pipelines, CircleCI, Buildkite, Drone, TeamCity, Woodpecker,
Travis — is still missing, and each one unblocks every team standardised on that
vendor.

An adapter is deliberately thin: take the build output, upload it to the
orchestrator with an API key, and let the console handle versions and
deployments. Start from
[manual upload](https://mfe-orchestrator.dev/documentation/docs/ci-cd/manual-upload)
to see what the API expects, then read the
[GitHub Action](https://github.com/mfe-orchestrator/github-action) or the
[GitLab CI stage](https://github.com/mfe-orchestrator/gitlab-pipeline-stage) as a
reference implementation. Open an issue on
[.github](https://github.com/mfe-orchestrator/.github/issues) before you start so
nobody duplicates the work — we will create the repository for you.

### Test, test, test

The quickest way to make the project better is to use it and report what breaks.
Run the [quick start](https://mfe-orchestrator.dev/documentation/docs/quick-start)
on your own stack, self-host it, push it somewhere it was not designed to go. A
bug report with clear steps to reproduce is a real contribution.

The paths that benefit most from being exercised outside our machines:

- **self-hosting** — Docker Compose, Terraform / OpenTofu, Helm on your cluster,
  SSO against your identity provider;
- **storage backends** — AWS S3, Azure Blob, Google Cloud Storage, your CDN, or
  fully on-premise;
- **hosts and bundlers** — Vite and Webpack Module Federation, Web Components,
  and whatever combination your shell actually uses;
- **canary releases**, still experimental and hungry for real traffic.

Report what you find in the repository concerned, or on
[.github](https://github.com/mfe-orchestrator/.github/issues) if you are not sure
which one that is.

## License

The platform is licensed under
[Apache-2.0](https://github.com/mfe-orchestrator/mfe-orchestrator/blob/main/LICENSE);
the surrounding tooling and templates are MIT. Each repository carries its own
LICENSE file.

---

If MFE Orchestrator is useful to you, a
[⭐ on the main repository](https://github.com/mfe-orchestrator/mfe-orchestrator)
helps the project get found.
