# Friendz website (GitHub Pages + Supabase)

This is a static website. It does not need Node.js and does not need to be built. `index.html` already contains your Supabase project URL and publishable key.

## Features wired in the client
- Supabase email/password sign-up and login.
- Profile setup and profile editing.
- Username-based friend requests, accept/decline, and accepted-friend list.
- Public chat for signed-in users.
- Direct messages in a shared room for accepted friends only. The SQL RLS policies also enforce this on the server, not just in the UI.
- Realtime subscriptions for messages, profiles, friend requests, and giveaway entries.

## Required one-time Supabase setup
1. Open your Supabase project and go to **SQL Editor**.
2. Create a new query, paste all of `supabase.sql`, and run it. The script is designed to update the earlier Friendz schema and replace the earlier overly broad message policies.
3. Go to **Authentication → Providers → Email** and enable Email sign-in.
4. In **Authentication → URL Configuration**, set the Site URL to your GitHub Pages site URL and add that same URL under Redirect URLs. Example: `https://YOUR-USERNAME.github.io/friendz/` (use your real URL).
5. If you want users to start chatting immediately after sign-up, you may turn off email confirmation in Supabase's Email provider settings. Otherwise, users must click the confirmation link in their email before logging in.
6. In **Database → Publications** (if needed), confirm `messages`, `profiles`, `friend_requests`, and `giveaway_entries` are included in `supabase_realtime`. The SQL tries to add them automatically.

## Publish on GitHub Pages
1. Create a public repository named `friendz`.
2. Upload `index.html`, `supabase.sql`, and `README.md` to the repository root (not inside another folder).
3. Go to **Settings → Pages** → choose **Deploy from a branch**, select `main` and `/(root)`, then save.
4. Wait for GitHub Pages to publish the URL. Add that URL to Supabase's URL Configuration as described above.

## Important security behavior
- Only signed-in users can read profiles and public chat.
- Friend requests are visible only to their sender and recipient.
- Private-message reads and inserts require both participants to be accepted friends. This is enforced by Row Level Security in Supabase.
- The browser uses a publishable key only. Never put a `service_role` or secret key in a website.

## Notes
- This package has been syntax-checked for JavaScript, but it has not been end-to-end tested against your live Supabase project from this environment.
- Currently watching is shared when a user pastes a YouTube / YouTube Music link; a normal website cannot detect playback in other apps or browser tabs.
- Game collection and accent color are currently client-side UI features and are not synced across devices.
