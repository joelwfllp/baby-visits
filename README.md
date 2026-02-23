# Come Meet the Baby! 👶

A simple family visit coordinator — lets family pick dates to visit, and lets you block out dates or see who's coming when.

## Setup (10 minutes)

### 1. Create Supabase Tables

Go to your Supabase project → **SQL Editor** → run this:

```sql
-- Visits table
create table visits (
  id bigint generated always as identity primary key,
  name text not null,
  dates text[] not null,
  notes text,
  created_at timestamptz default now()
);

-- Blocked dates table
create table blocked_dates (
  id bigint generated always as identity primary key,
  date text not null unique,
  created_at timestamptz default now()
);

-- Allow public read/write (this is a family tool, not Fort Knox)
alter table visits enable row level security;
alter table blocked_dates enable row level security;

create policy "Allow all on visits" on visits for all using (true) with check (true);
create policy "Allow all on blocked_dates" on blocked_dates for all using (true) with check (true);
```

Then enable **Realtime** for both tables:
- Go to **Database → Replication**
- Toggle on `visits` and `blocked_dates`

### 2. Add Your Supabase Credentials

Open `index.html` and fill in the two values at the top of the script:

```js
const SUPABASE_URL = 'https://YOUR_PROJECT.supabase.co';
const SUPABASE_ANON_KEY = 'YOUR_ANON_KEY';
```

You can find these in Supabase → **Settings → API**.

### 3. Change the Admin Password

In the same config section:

```js
const ADMIN_PASSWORD = 'baby2026';  // change this
```

### 4. Deploy to GitHub Pages (free hosting)

```bash
cd ~/baby-visits
git init
git add .
git commit -m "Initial commit"
```

Then on GitHub:
1. Create a new repository (e.g., `baby-visits`)
2. Push:
   ```bash
   git remote add origin https://github.com/YOUR_USERNAME/baby-visits.git
   git branch -M main
   git push -u origin main
   ```
3. Go to **Settings → Pages** → set source to `main` branch → Save
4. Your site will be live at `https://YOUR_USERNAME.github.io/baby-visits/`

## How It Works

**For family:**
- Open the link
- Click dates on the calendar
- Enter your name and optional notes
- Hit submit — done!

**For you (admin):**
- Click "Parents' Admin" at the bottom
- Enter the password
- Click dates to block/unblock them (red = blocked)
- Remove visit requests if needed

Everyone sees the same calendar in real-time. Blue dates = someone's visiting. Red = blocked.
