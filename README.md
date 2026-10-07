# tarmacked-data

tarmacked's build workflows, kept in a public repo so GitHub's runners (Mac ones included) are free:
- **Road data:** builds the road data and map files from OpenStreetMap and publishes them to tiles.tarmacked.com (Cloudflare R2). Every region on the 2nd of each month, or some by hand.
- **TestFlight:** builds the iPhone app and uploads it to App Store Connect. By hand.

This repo only holds the workflows. All the code is in the private app repo (0204Killian/tarmacked), and each run fetches it from there with a read-only token. Run logs here are public; nothing secret is printed, and the built app is never attached to a run.

## Secrets (Settings → Secrets and variables → Actions)
| Name | What |
|---|---|
| `R2_ACCOUNT_ID` | Cloudflare R2 account ID |
| `R2_ACCESS_KEY_ID` | R2 access key |
| `R2_SECRET_ACCESS_KEY` | R2 secret key |
| `APP_REPO_TOKEN` | GitHub token that can read the app repo (below) |
| `APPLE_TEAM_ID` | Apple Team ID (developer.apple.com → Membership) |
| `ASC_KEY_ID` | App Store Connect API key ID |
| `ASC_ISSUER_ID` | its Issuer ID |
| `ASC_KEY_P8` | the contents of the AuthKey_XXXX.p8 file |

## Making APP_REPO_TOKEN
1. github.com → your picture → Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate new token.
2. Name: `tarmacked-data read`. Expiration: 1 year (set a reminder to renew it).
3. Repository access: **Only select repositories** → `0204Killian/tarmacked`.
4. Permissions → Repository permissions → **Contents: Read-only** (nothing else).
5. Generate, copy it, add it here as the `APP_REPO_TOKEN` secret.

## Running by hand
- Road data: Actions → Road data → Run workflow → regions: `ie`, `gb`, `ie,gb` or `all`.
- A TestFlight build: push your changes to the app repo, then Actions → TestFlight → Run workflow (branch `main`).

## Changing the app or the pipeline
Edit and push in the app repo as usual. The next run here uses it. Nothing to copy across.

Road data © OpenStreetMap contributors, ODbL.
