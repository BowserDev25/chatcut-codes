# ChatCut Free Codes

Free invite code sharing site for ChatCut / Seedance 2. Stop paying for codes that should be free.

## Project Structure

```
ChatCut/
├── index.html          # Public site — anyone can view/copy/mark codes
├── admin.html          # Protected admin panel — add confirmed codes (DO NOT deploy publicly)
├── config.js           # Supabase credentials (fill in your keys)
├── supabase-schema.sql # Run this in Supabase SQL editor to create tables + policies
└── README.md           # This file
```

## Setup

### 1. Create Supabase Project
1. Go to https://supabase.com and create a free project
2. Copy your **Project URL** and **anon (public) key** and **service_role key** from Settings > API

### 2. Configure Database
1. In Supabase dashboard, go to **SQL Editor**
2. Paste the entire contents of `supabase-schema.sql` and click **Run**
3. This creates the `codes` table with proper Row Level Security policies

### 3. Add Your Keys
1. Open `config.js` and paste your Supabase URL and keys

### 4. Test
1. Open `index.html` in a browser — public site should load
2. Open `admin.html` in a browser — enter admin PIN to add confirmed codes

## Security Model

- **Public site (index.html)**: Uses the `anon` key. Can only:
  - READ all codes
  - INSERT codes with type='community' only
  - UPDATE the `status` field on any code
  - INCREMENT `reports` on community codes via RPC function
  
- **Admin page (admin.html)**: Uses the `service_role` key which bypasses RLS.
  - Can INSERT confirmed codes
  - **⚠️ NEVER deploy this file publicly** — keep it local or behind server auth

- Row Level Security prevents users from:
  - Adding fake "confirmed" codes via the public anon key
  - Deleting codes
  - Modifying code text or type
