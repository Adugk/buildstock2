# BuildStock v2
### Construction Equipment Inventory Manager
**Cross-platform · Multi-user · English & አማርኛ**

---

## What's included

- Real-time sync across all devices via Supabase
- User login / signup (email + password)
- Full inventory management (add, edit, remove products)
- Record sales tied to specific employees
- Sales history with revenue, profit, and margin tracking
- Employee performance dashboard
- English ↔ Amharic (አማርኛ) language toggle
- Installable PWA — works on iPhone, Android, and desktop
- Offline-capable after first load

---

## Step 1 — Create a Supabase project

1. Go to [https://supabase.com](https://supabase.com) and sign up (free)
2. Click **New Project**, give it a name (e.g. `buildstock`), set a database password, choose a region
3. Wait ~2 minutes for it to provision

---

## Step 2 — Create the database tables

1. In your Supabase project, go to **SQL Editor** (left sidebar)
2. Click **New Query**, paste the SQL below, and click **Run**

```sql
-- Products
create table if not exists products (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  category text not null,
  cost numeric not null default 0,
  price numeric not null default 0,
  qty integer not null default 0,
  created_at timestamptz default now()
);

-- Employees
create table if not exists employees (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  role text not null,
  phone text,
  created_at timestamptz default now()
);

-- Sales
create table if not exists sales (
  id uuid primary key default gen_random_uuid(),
  product_id uuid references products(id),
  product_name text,
  employee_id uuid references employees(id),
  qty integer not null,
  revenue numeric not null,
  profit numeric not null,
  notes text,
  created_at timestamptz default now()
);

-- Row Level Security — allow authenticated users full access
alter table products enable row level security;
alter table employees enable row level security;
alter table sales enable row level security;

create policy "auth users" on products for all using (auth.role() = 'authenticated');
create policy "auth users" on employees for all using (auth.role() = 'authenticated');
create policy "auth users" on sales for all using (auth.role() = 'authenticated');
```

---

## Step 3 — Get your API credentials

1. In Supabase, go to **Settings → API**
2. Copy your **Project URL** (looks like `https://xxxxxxxxxxxx.supabase.co`)
3. Copy your **anon / public** key (the long `eyJ…` string)

---

## Step 4 — Host on GitHub Pages

1. Create a new GitHub repository (e.g. `buildstock`)
2. Upload all files from this folder to the repo root (drag & drop or git push)
3. Go to **Settings → Pages**
4. Under **Source**, select `Deploy from a branch` → `main` → `/ (root)` → **Save**
5. Your app will be live at:
   ```
   https://YOUR-USERNAME.github.io/buildstock/
   ```

---

## Step 5 — Connect the app

1. Open your hosted URL
2. On the **Connect to Supabase** screen, paste your Project URL and Anon Key
3. Click **Connect**
4. Sign up with your email and password
5. Done — start adding products and employees!

> Your credentials are saved in the browser. Every device you connect will sync in real time.

---

## Install as a native app (no App Store needed)

### iPhone / iPad
1. Open the app URL in **Safari**
2. Tap the **Share** button (box with arrow)
3. Tap **"Add to Home Screen"** → **Add**

### Samsung / Android
1. Open in **Chrome**
2. Tap **⋮ menu** → **"Install app"** or **"Add to Home Screen"**

### Desktop (Windows / Mac)
1. Open in **Chrome** or **Edge**
2. Click the **install icon** (⊕) in the address bar
3. Or: browser menu → **"Install BuildStock"**

---

## Run locally (for development)

```bash
# Python 3
python3 -m http.server 8080
# then open http://localhost:8080

# Node.js
npx serve .
# then open http://localhost:3000
```

---

## File structure

```
buildstock/
├── index.html          # App shell
├── manifest.json       # PWA manifest
├── sw.js               # Service worker (offline support)
├── _config.yml         # GitHub Pages config
├── css/
│   └── style.css       # All styles (dark industrial theme)
├── js/
│   ├── i18n.js         # English + Amharic translations
│   ├── supabase.js     # Supabase client + all DB operations
│   └── app.js          # App logic, routing, all pages
├── icons/
│   ├── icon-192.png    # PWA icon
│   └── icon-512.png    # PWA icon (large)
└── README.md
```

---

## Language switching

Click **Settings** in the sidebar and tap **English** or **አማርኛ** to switch languages. The setting is saved per device.

---

## Multi-user / multi-device sync

All data lives in your Supabase database. Anyone who logs in with a valid account on any device — phone, tablet, or desktop — sees the same live data. Changes (new sales, product edits, etc.) sync automatically in real time via Supabase Realtime.

To invite team members, go to your Supabase project → **Authentication → Users** → **Invite user**, or have them sign up directly through the app.
