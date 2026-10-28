# GATES deck: password-protected GitHub Page

`src/index.html` is the presentation. On every push to `main`, the workflow in
`.github/workflows/deploy.yml` encrypts it with [StatiCrypt](https://github.com/robinmoisson/staticrypt)
using the `PRESO_PASSWORD` secret and publishes only the encrypted page to GitHub Pages.

## One-time setup

1. **Keep this repository private.** `src/index.html` is the unencrypted deck. If the repo
   is public, anyone can read it here and the password protects nothing.
   (GitHub Pages from a private repo needs a paid plan: Team or Enterprise for an org.
   The published site is still reachable by anyone with the URL and password.)
2. **Add the password:** Settings > Secrets and variables > Actions > **Secrets** tab >
   New repository secret. Name: `PRESO_PASSWORD`. Use a secret, not a "variable":
   variables are shown in plain text in the UI and logs.
3. **Turn on Pages:** Settings > Pages > Build and deployment > Source: **GitHub Actions**.
4. Push to `main` (or run the workflow by hand from the Actions tab). The site URL appears
   in the workflow run and under Settings > Pages, normally
   `https://nebulock-inc.github.io/datadog-detect-GATES-preso/`.

## Updating

- **New deck version:** replace `src/index.html` and push.
- **New password:** update the `PRESO_PASSWORD` secret, then re-run the workflow
  (Actions > Deploy password-protected deck > Run workflow).

## What the password does and doesn't do

The page is encrypted with AES-256 and decrypted in the browser, so there is no server
login. Anyone can download the encrypted file and try passwords offline, so use a long
passphrase (four or more random words) rather than a short password. This is right for
"only people at the talk", not for confidential material.

The prompt has a "Remember me" checkbox. A redeploy uses a new salt, so returning
visitors re-enter the password after each push.
