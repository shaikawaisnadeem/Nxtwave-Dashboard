# Supabase Cloud Setup Instructions

This guide will help you set up the NxtWave Dashboard database schema in your cloud Supabase instance.

## Prerequisites

1. A Supabase account and project (https://supabase.com)
2. Your Supabase project URL and anon key
3. Access to the Supabase SQL Editor

## Step 1: Get Your Supabase Credentials

1. Go to your Supabase project dashboard
2. Navigate to **Settings** → **API**
3. Copy the following:
   - **Project URL** (e.g., `https://xxxxx.supabase.co`)
   - **anon/public key** (starts with `eyJ...`)

## Step 2: Update Environment Variables

Create a `.env` file in the project root (or update your existing one):

```env
VITE_SUPABASE_URL=https://your-project-id.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key-here
```

## Step 3: Run SQL Scripts in Supabase

Open the Supabase SQL Editor (https://supabase.com/dashboard/project/_/sql) and run the scripts in this order:

### 3.1. Create Schema

Copy and paste the contents of `supabase/schema.sql` into the SQL Editor and click **Run**.

This will create:
- `roles` table
- `products` table
- `departments` table
- `users` table
- `employee_contributions` table
- `contribution_status` enum
- `contribution_details` view
- `get_top_contributors` function

### 3.2. Enable Row Level Security

Copy and paste the contents of `supabase/policies.sql` into the SQL Editor and click **Run**.

This will:
- Enable RLS on all tables
- Create policies for role-based access control

### 3.3. Seed Initial Data

Copy and paste the contents of `supabase/seed.sql` into the SQL Editor and click **Run**.

This will insert:
- 3 products (NIAT, Intensive, Academy)
- 6 departments (2 per product)

## Step 4: Verify Setup

1. Go to **Table Editor** in Supabase dashboard
2. Verify these tables exist:
   - `roles`
   - `products`
   - `departments`
   - `users`
   - `employee_contributions`

3. Check that `products` table has 3 rows
4. Check that `departments` table has 6 rows

## Step 5: Test the Application

1. Start the development server:
   ```bash
   npm run dev
   ```

2. Navigate to `http://localhost:5173`
3. Try signing up a new user
4. Complete the onboarding flow
5. Test creating a contribution (as an employee)

## Troubleshooting

### Error: "relation does not exist"
- Make sure you ran `schema.sql` first
- Check that you're connected to the correct Supabase project

### Error: "permission denied"
- Make sure you ran `policies.sql` after `schema.sql`
- Check that RLS is enabled on the tables

### Error: "duplicate key value"
- The seed data might already exist
- This is okay - the `ON CONFLICT DO NOTHING` clause prevents errors

### Authentication Issues
- Verify your `.env` file has the correct `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`
- Make sure there are no extra spaces or quotes in the values
- Restart your dev server after updating `.env`

## Next Steps

After setup, you can:
1. Create user accounts through the signup flow
2. Assign roles during onboarding
3. Start using the dashboard features

For production deployment, make sure to:
- Use environment variables in your hosting platform
- Set up proper CORS settings in Supabase
- Configure email templates for authentication
- Set up backup schedules

