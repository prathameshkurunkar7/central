# Moving to this version

This version removes every old migration patch. Central is still pre-1.0. No site runs
it in production. An old site cannot update into this version, because it has no patch
left to carry its data forward.

## What to do

Drop your old site and make a new one.

1. Drop the old site.

   ```bash
   pilot frappe --site your-site.localhost drop-site
   ```

2. Create a new site.

   ```bash
   pilot new-site your-site.localhost
   ```

3. Install Central on it.

   ```bash
   pilot install-app your-site.localhost central
   ```

4. Start the bench.

   ```bash
   pilot start
   ```

Replace `your-site.localhost` with your site's real name.

## Why this is safe

Central has no real customer data yet. Every site is a dev or staging site. A fresh
site gets today's schema straight away, with nothing to migrate and nothing to break.

## If you add a new patch later

Add it under `[pre_model_sync]` or `[post_model_sync]` in `central/patches.txt`, the
same way Frappe always works. This file only explains the one-time cleanup; it does not
change how patches work going forward.
