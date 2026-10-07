# Making the app repo private (about 10 minutes)

Do these in order. Nothing changes for phones: they only ever talk to tiles.tarmacked.com.

## 1. Create the public data repo
1. github.com → **New repository** → name `tarmacked-data` → **Public** → no README → Create.
2. On your PC:
```
cd /d/dev
unzip -o ~/Downloads/"tarmacked-data.zip"
cd tarmacked-data
git init -b main
git add -A && git commit -m "Road data workflow (pipeline code fetched from the app repo)"
git remote add origin https://github.com/0204Killian/tarmacked-data.git
git push -u origin main
```

## 2. Secrets in tarmacked-data
Settings → Secrets and variables → Actions → New repository secret:
- `R2_ACCOUNT_ID`, `R2_ACCESS_KEY_ID`, `R2_SECRET_ACCESS_KEY`: same values as in the app repo (GitHub won't show them again; copy from Cloudflare or ~/.tarmacked-r2 if you need them).
- `APP_REPO_TOKEN`: make it as in README.md (fine-grained, only `tarmacked`, Contents: Read-only).
- `APPLE_TEAM_ID`, `ASC_KEY_ID`, `ASC_ISSUER_ID`, `ASC_KEY_P8`: same as in the app repo (Issuer `17cf9a7d-5e67-4fb9-8b08-6d06a13fcd73`, Key ID `T848Y67JQD`, the .p8 file's contents, your Team ID).

## 3. Take the old workflows out of the app repo
```
cd /d/dev/tarmacked
git rm -r .github/workflows
git commit -m "Build workflows moved to the public tarmacked-data repo" && git push
```
The app repo then has no workflows: it uses no Actions minutes at all. Builds and road data run in tarmacked-data, free.

## 4. Make the app repo private
tarmacked → Settings → General → bottom → **Change repository visibility** → Private.

## 5. Check it works
- tarmacked-data → Actions → **TestFlight** → Run workflow (`main`). The new build arrives in TestFlight with a build number from 1001 up.
- Road data whenever you next need it (regions `ie`); the monthly run on the 2nd now happens here by itself.

If a checkout step says "Repository not found", the token is wrong or missing.

You can delete the secrets from the app repo afterwards (Settings → Secrets): nothing there uses them any more.
