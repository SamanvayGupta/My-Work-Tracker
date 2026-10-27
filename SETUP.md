# YOS PWA + styled Gmail email update

## Push the PWA to GitHub Pages (main branch)
1. Download and extract this package.
2. In your GitHub repository, open the repository root on the `main` branch.
3. Upload/replace `index.html`, `manifest.webmanifest`, `sw.js`, and the `icons/` folder. Keep these paths and names exactly. Commit the changes to `main`.
4. In GitHub: Settings → Pages, select deploy from branch `main` and folder `/ (root)` (if not already configured). Save.
5. Wait for GitHub Pages deployment to complete, then open the HTTPS site. Refresh once or twice if the previous version is cached. The page registers `sw.js`; installation is offered by supported browsers.
6. Test on the hosted HTTPS URL. PWA install won't work fully from a local `file://` URL.

## Styled Gmail email worker
1. Open the existing Google Apps Script project and replace the contents of `Code.gs` with the `Code.gs` in this package.
2. Do NOT delete Script Properties (`SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`). Do not put these secrets in GitHub or HTML.
3. Save. Run `sendYosTestEmail` to see the new styled YOS email.
4. Existing triggers normally remain when editing code. Check Triggers to confirm `checkYosDeadlineEmails` and `sendYosDailySummaries` still exist. Run `installYosTriggers` only if triggers are missing or need to be recreated; it replaces its own matching triggers to avoid duplicates.
5. Test deadline/start reminders and daily summary with non-critical test data.

## Notes
- This release changes the email HTML presentation while keeping a plain-text fallback.
- It does not require a Supabase schema change.
- PWA shell caching does not make Supabase sync or scheduled email work offline; those need connectivity/server triggers.
- Gmail may categorize messages into Primary, Promotions, or Everything else; the sender cannot force a particular Gmail inbox tab.
