# D'Verse Supabase Contract

This document records the shared cloud backend created for D'Verse apps.

## Supabase Project

- Project name: dverse-production
- Project ref: gmwieijbrrztukqpfwkg
- API URL: https://gmwieijbrrztukqpfwkg.supabase.co
- Region: ap-south-1
- Database: Postgres 17

## Shared Auth

Use Supabase Auth as the common D'Verse ID provider. Apps should use the same Supabase project URL and publishable anon key, then enable email sign-in and Google OAuth in the Supabase dashboard.

Required public environment variables for browser/mobile clients:

```env
VITE_SUPABASE_URL=https://gmwieijbrrztukqpfwkg.supabase.co
VITE_SUPABASE_ANON_KEY=<publishable anon key from Supabase dashboard>
```

For non-Vite apps, use the equivalent public env names already used by the framework.

## App Data Tables

- profiles: shared D'Verse profile, one row per auth user.
- homepage_state: D'Verse homepage preferences and recents.
- ai_chats, ai_messages: D'Ai chat persistence.
- webboard_boards, webboard_collaborators, webboard_snapshots: WebBoard cloud boards and sharing.
- dtunes_tracks, dtunes_history, dtunes_likes, dtunes_playlists, dtunes_playlist_items: D'Tunes history, likes, and playlists.
- dquest_progress: D'Quest progress and stats.

## Storage Buckets

- avatars: public avatar files; write paths must start with the user id.
- webboard-assets: private board assets and exports.
- dtunes-assets: private D'Tunes artwork or playlist assets.
- dai-attachments: private D'Ai attachments.

Private bucket object paths must start with the authenticated user id.

## Security

RLS is enabled on all public tables. Users can only access their own data, except explicit WebBoard collaborator access and public D'Tunes playlists.

Security advisors are clean as of the initial setup. Performance advisors still include expected warnings for a new database, mostly unused indexes until real traffic exists and optional RLS init-plan optimizations.

## Google OAuth Setup

In Google Cloud Console, create OAuth clients for the D'Verse web apps and mobile app if needed. In Supabase Auth provider settings, enable Google and use the Supabase callback URL shown in the provider setup screen for project gmwieijbrrztukqpfwkg.