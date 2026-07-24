# The Olive Tree House — website + automatic availability sync

This folder is ready to become a GitHub repository. It contains:

- `dist/` — the live website (exactly what's currently deployed on Netlify)
- `sync/sync_availability.py` — the script that merges your Airbnb + Booking.com
  calendars into `dist/assets/availability.json`
- `.github/workflows/sync-availability.yml` — runs that script automatically
  every 3 hours and commits the result

## Setup steps

1. **Create a GitHub account** at github.com/signup, if you don't have one.

2. **Create a new repository.**
   - Click the `+` in the top right → "New repository"
   - Name it something like `olive-tree-house-site`
   - Set it to **Private**
   - Don't add a README/gitignore/license — leave it empty
   - Click "Create repository"

3. **Upload these files.**
   - On the new (empty) repo's page, click "uploading an existing file"
   - Drag this entire folder's contents in (the `dist`, `sync`, and `.github`
     folders together, not zipped)
   - Commit directly to the `main` branch

4. **Add your two calendar links as secrets** (so they're never visible in
   the code itself):
   - Go to the repo's **Settings → Secrets and variables → Actions**
   - Click "New repository secret"
   - Name: `AIRBNB_ICAL_URL` — value: your Airbnb export link
   - Repeat for `BOOKING_ICAL_URL` with your Booking.com export link

5. **Test it manually once.**
   - Go to the **Actions** tab → "Sync availability calendar" (left side)
   - Click "Run workflow" → "Run workflow" again to confirm
   - Wait ~30 seconds, refresh — it should show a green checkmark
   - This also helps GitHub register the schedule properly for future
     automatic runs

6. **Connect this repo to your existing Netlify site.**
   - In Netlify, go to your site → **Project configuration → Build & deploy
     → Continuous deployment**
   - Select **Link repository**, choose GitHub, authorize, and pick this repo
   - Set **Base directory**: (leave blank)
   - Set **Publish directory**: `dist`
   - Set **Build command**: (leave blank — nothing needs building)
   - Save

From that point on: every ~3 hours, the sync script checks both calendars,
updates the availability file if anything changed, and Netlify automatically
republishes the site with the new data. You shouldn't need to do anything
further — dragging folders into Netlify manually is no longer necessary once
this is connected.

## If something looks wrong

- **Actions tab shows a red X:** click into the failed run to see the error.
  Most likely cause: one of the two secret URLs was mistyped.
- **Site isn't updating:** check Netlify's "Deploys" tab — it should show a
  new deploy each time the Actions workflow pushes a change.
- **Scheduled runs never seem to fire:** this can happen on brand-new
  repositories; re-running the workflow manually once from the Actions tab
  (step 5) usually fixes it.
