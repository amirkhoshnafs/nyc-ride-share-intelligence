# Before publishing this repository

The ZIP is prepared for uploading to GitHub with five schematic report previews, documentation, report-definition source, and the supplied Drive link. Only complete the actions that apply to your public release:

- [ ] Confirm the uploaded PBIX on Google Drive is the **latest file saved after** fixing `Rides With Negative Duration` in Desktop.
- [ ] Open the Google Drive URL in a private/incognito window to confirm an unauthenticated user can download it.
- [ ] In Power BI Desktop export real screenshots of all five pages at a consistent 16:9 size; replace `assets/dashboard/01-...png` through `05-...png` with those exports. The current images are accurate **layout previews** but show schematic charts and `Live KPI` placeholders.
- [ ] Once real captures are in place, revise the image-provenance note in `README.md` and `docs/reproduction.md` to say they are live Desktop screenshots.
- [ ] Check the full PBIX for private connection strings, customer information, or credentials before public distribution.
- [ ] Confirm the published facts and source data license/attribution meet intended release requirements.
- [ ] Upload these files to a public GitHub repository. A source-controlled `.SemanticModel` could be added separately if you later export it and verify it contains no cached data or secrets.

Suggested repository name: `nyc-ride-share-intelligence`.

If using Git on a machine with Git installed:

```text
git init
git branch -M main
git add .
git status --short
git commit -m "Publish NYC Ride-Share Intelligence case study"
git remote add origin https://github.com/amirkhoshnafs/nyc-ride-share-intelligence.git
git push -u origin main
```

Create the empty destination repository first; replace the remote URL if you choose another repository name. No video or GIF is included.
