# Hudaia public design preview

This repository hosts a **public static preview** of Hudaia MVP v0.1, copied from the private infrastructure repository's `site/index.html`. No private infrastructure files, credentials, backend services or database are published here.

## View

After GitHub Pages is enabled, the expected URL is:

https://hudaia.github.io/hudaia-pr1/

## Enable GitHub Pages

1. Open **Settings → Pages** in this repository.
2. Under **Build and deployment**, set **Source** to **GitHub Actions**.
3. Open **Actions**, select **Deploy static Hudaia preview**, and run the workflow if it did not start automatically.
4. Wait for a successful deployment, then open the URL above.

## Update the design

Edit `index.html` and push to `main`; GitHub Actions redeploys the preview. Changes in private `hudaia/Infrac` do **not** automatically synchronize here.

This is a static placeholder only, not a live login or database-backed application.
