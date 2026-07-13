# AMOC in Focus — Figure Review Dashboard

A static dashboard for coordinating figure iterations across the report's
6 chapters + the Summary for Policy Makers (SPM).

## Publish on GitHub Pages (one-time setup)

1. Create a new GitHub repository (public, or private with Pages enabled on a paid plan).
2. Push the contents of this folder to the repository root:
   ```
   git init
   git add .
   git commit -m "Initial dashboard"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
3. In the repo: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
4. The dashboard will be live at `https://<you>.github.io/<repo>/`.

## Pushing a new figure version

Drop the new file into `figures/` using the naming scheme:

```
Ch_<chapter>_Figure_<number>[letter]_v<version>.png     e.g. Ch_2_Figure_3_v1.png
Ch_SPM_Figure_<number>_v0.png                           for SPM figures
```

Then commit + push. The included GitHub Action automatically:
- regenerates `figures-data.js` (keeping all titles/captions/contacts you've filled in),
- creates the missing thumbnails in `thumbs/`.

The dashboard also probes for new files client-side, so a new version shows up
even before the Action finishes (it temporarily uses the full-resolution file
as its own thumbnail).

## Editing titles, captions, contacts

Edit `figures-data.js` directly — each figure has `title`, `caption`, `notes`,
`contact` fields. The Action never overwrites text you've entered.

You can also run the update locally: `python3 scripts/update_figures.py`
(needs `pip install Pillow` for thumbnails).

## Passphrase

The access gate accepts the passphrase defined in `index.html`
(search for `tryGate` — currently `amoc`). It is a light client-side gate:
it deters casual visitors but is not real security; the image URLs remain
technically public.

## Author comments — how delivery works

Each "Send feedback" click emails the comment to the coordinator
via formsubmit.co (using an alias code, so the address never appears in the page source), with the figure name as
the subject line (e.g. "Fig. 4.2"). No backend or account is needed, but:

- **One-time activation:** the very first submission triggers a confirmation
  email from formsubmit.co to the coordinator — click the activation link in
  it once, and all subsequent feedback is delivered normally.
- The comment also stays visible in the sender's own browser (localStorage),
  so authors can see what they already sent. Other authors do not see each
  other's comments — the email to the coordinator is the single source of truth.
- To change the destination address, replace the formsubmit alias code in
  index.html (search for "formsubmitCode").
