# shiftowl-reset

Public landing page for ShiftOwl AI password-reset flow. Hosted on GitHub Pages at https://giuseppepiacenza-builder.github.io/shiftowl-reset/ and wired as the `redirect_to` target on Supabase's recovery email.

## What this is

A single static `index.html` that:

1. Accepts a PKCE `code` (or implicit-flow hash tokens) in the URL from Supabase's `/auth/v1/verify?type=recovery` redirect.
2. Establishes a short-lived recovery session via `supabase.auth.exchangeCodeForSession` (or `setSession` for the implicit fallback).
3. Renders a branded form for a new password.
4. Calls `supabase.auth.updateUser({ password })`, then signs out so the next login in the app uses the new password on a fresh session.
5. Offers an "Open ShiftOwl" button back into the app.

## Why separate repo

The main ShiftAI app repo is private. GitHub Pages on a private repo is also private — users can't hit it anonymously after clicking the email link. This repo is public, exposes only the Supabase anon key (same one shipped in the iOS app bundle), and has no other secrets.

## Deploy

Push to `main`. GitHub Pages must be enabled via repo Settings → Pages → Source: `main` / root.

## Local preview

```bash
python3 -m http.server 8000
# open http://localhost:8000/?code=testfake
```

The form will go to the "expired or already used" error state with a fake code — that's correct behaviour. The full flow can only be tested with a real Supabase recovery email.
