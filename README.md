# foremarket-deployer

Deploys the `staging` branch of `Foremarket/foremarket-web` to Vercel production (`https://foremarket-web.vercel.app`).

Every 5 minutes `deploy.yml` reads the `staging` head and the commit on the latest Vercel production deployment. It deploys only when they differ, or when production was built from uncommitted changes. Vercel builds the app; this repo holds no application code.

## Secrets

| Secret | Value |
| --- | --- |
| `SOURCE_REPO_TOKEN` | Classic GitHub token with `repo` scope that can read `Foremarket/foremarket-web` |
| `VERCEL_TOKEN` | Vercel token for the `foremarket-web` project |
| `VERCEL_ORG_ID` | Vercel team id of the project |
| `VERCEL_PROJECT_ID` | Vercel project id |

Both tokens expire. An expired token turns every run red and sends a failure email; replace the secret.

## Operating

- **Production deploys come only from here.** Do not run `vercel --prod` from a laptop; preview deploys are fine.
- **Deploy now** (after changing a Vercel environment variable, or after a failed build): Actions → deploy → Run workflow → tick `force`.
- **Check the decision without deploying:** Run workflow → tick `dry_run`.
- **Pause:** `gh workflow disable deploy.yml -R rubel-vivasoft/foremarket-deployer`. Resume with `gh workflow enable`.
- **After an Instant Rollback** in the Vercel dashboard, the verify step fails until production is promoted again: `vercel promote <deployment-url>`.
- **A failed build is not retried** on its own. Push a fix to `staging`, or run with `force`.
- Expect 5 to 30 minutes between a push and the deploy; GitHub can delay scheduled runs.
- `keepalive.yml` runs monthly so GitHub does not disable the schedule after 60 quiet days.
