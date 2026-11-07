# ZWOM JARVIS website + Discord backend

This package is prepared for your existing repository:
`https://github.com/bommerakethan/ZWOM-JARVIS.github.io`

## What is included

- `index.html` — refreshed orange/gold holographic JARVIS landing page.
- `JARVIS_Discord_Dashboard.html` — Discord backend configuration and health-check dashboard.
- `backend/` — FastAPI Discord interactions service, slash-command registration, dependencies and environment template.
- `render.yaml` — Render Blueprint configuration.

## IMPORTANT: Preserve your existing legal pages

This package intentionally does **not** include or replace `terms.html` or `privacy.html`.
Keep the versions already in your GitHub repository. The new homepage links to those existing files.

## Upload to your existing GitHub repository

1. Download and extract this ZIP.
2. Open your repository on GitHub and select the `main` branch.
3. Upload/replace `index.html` and `JARVIS_Discord_Dashboard.html` in the repository root.
4. Upload the entire `backend` folder, preserving its folder structure.
5. Upload `render.yaml` and `.gitignore` to the repository root.
6. Commit the changes to `main`.
7. Do not delete or overwrite `terms.html` or `privacy.html`.

Your existing GitHub Pages configuration can remain `main` / `/(root)`.
Wait for Pages deployment, then hard-refresh the website with Ctrl+Shift+R.

## Deploy backend from this SAME repository

GitHub Pages only serves static files; it does not run Python.
The backend remains in the same repository but must run as a separate Render web service.

1. Sign in to Render and choose **New + → Blueprint**.
2. Connect GitHub and select `ZWOM-JARVIS.github.io`.
3. Render should detect `render.yaml` in the repository root.
4. Create the service. In Render's Environment settings, fill in:
   - `DISCORD_PUBLIC_KEY` — Discord Developer Portal → General Information → Public Key
   - `DISCORD_APPLICATION_ID` — Discord Developer Portal → General Information → Application ID
   - `DISCORD_BOT_TOKEN` — Discord Developer Portal → Bot → Token
   - `JARVIS_API_KEY` — your AI provider API key
5. Keep secrets in Render only. Never commit `.env` or real tokens to GitHub.
6. Wait for the service to become Live. Test `https://YOUR-SERVICE.onrender.com/`; it should return JSON with `"status":"online"`.

The Render Blueprint uses `plan: free`; availability and sleep/usage limits depend on Render's current plan rules. You can change the plan in Render if needed.

## Register the Discord slash command

On your Windows PC, open PowerShell in the `backend` folder:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Fill `backend/.env` with the same Discord IDs/token and AI provider settings (do not upload it). Then run:

```powershell
python register_command.py
```

This registers the global `/jarvis prompt:...` command for the application.

## Configure Discord Interactions Endpoint URL

In Discord Developer Portal → General Information, set:

```text
https://YOUR-SERVICE.onrender.com/api/discord/interactions
```

Discord will send a signed PING. The backend verifies the Ed25519 signature and returns `{"type":1}`.

## Important limitations

- The HTML dashboard is a static control panel. It cannot itself receive Discord interactions.
- The backend sample supports the `/jarvis` application command and Discord PING validation. It does not implement Gateway events, buttons, select menus, persistent chat memory, rate limiting, or a full production security review.
- AI API usage may incur provider charges.
- A free Render service may sleep or have usage limits; check Render's current terms.
- Update the Privacy Policy to reflect the actual data processed by your final implementation before public launch.
