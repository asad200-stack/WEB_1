# Railway Variables Setup - IMPORTANT FIX

## ⚠️ Current Issue

You've added variables to the **Postgres** service, but they need to be in the **WEB_1** service!

## ✅ Correct Steps

### Step 1: Get the Real DATABASE_URL

1. In Railway, click on **Postgres** service
2. Go to **Variables** tab
3. Look for `DATABASE_URL` or `POSTGRES_URL` 
4. **Copy the actual connection string** (it should look like: `postgresql://postgres:password@host:port/railway`)

OR Railway automatically provides it - look for a variable that starts with `postgresql://`

### Step 2: Add Variables to WEB_1 Service (NOT Postgres!)

1. Click on **WEB_1** service (the one with GitHub icon)
2. Go to **Variables** tab
3. Click **"+ New Variable"**
4. Add these variables one by one:

```
DATABASE_URL=<paste-the-real-connection-string-from-postgres>
JWT_SECRET=QPGscfSCt6Lt7Wcs38zI2g8AMm99Y6EzeMbqSvH2oxk=
JWT_EXPIRES_IN=7d
NODE_ENV=production
MAX_FILE_SIZE=5242880
UPLOAD_DIR=./public/uploads
PLATFORM_NAME=Multi-Store Platform
SUPER_ADMIN_EMAIL=admin@platform.com
SUPER_ADMIN_PASSWORD=admin123456
```

### Step 3: Use Railway's Shared Variables (Easier Method)

Railway can automatically share DATABASE_URL from Postgres to WEB_1:

1. In **WEB_1** service → **Variables** tab
2. Click **"Shared Variable"** button (purple arrow icon)
3. Select **Postgres** service
4. Select **DATABASE_URL** variable
5. This will automatically link it!

Then add the other variables manually.

### Step 4: Redeploy WEB_1

1. Go to **WEB_1** service
2. Click **"Deployments"** tab
3. Click **"Redeploy"** button
4. Wait for build to complete

### Step 5: Run Migrations

After successful build:

1. In **WEB_1** → **Deployments** → Latest deployment
2. Click **"Run Command"**
3. Enter: `npx prisma migrate deploy`
4. Click **"Run"**

### Step 6: Create Super Admin

1. In same "Run Command" interface
2. Enter: `node scripts/create-admin.js admin@platform.com admin123456`
3. Click **"Run"**

## 📝 Summary

- ✅ Variables should be in **WEB_1** service (not Postgres)
- ✅ Use **Shared Variable** to link DATABASE_URL from Postgres
- ✅ Add all 9 variables listed above
- ✅ Redeploy WEB_1 after adding variables
- ✅ Run migrations and create admin

## 🔍 How to Find DATABASE_URL

If you can't find DATABASE_URL in Postgres variables:

1. Go to **Postgres** service → **Database** tab
2. Railway shows connection details there
3. Or check **Variables** tab - Railway usually creates it automatically
4. Format: `postgresql://postgres:PASSWORD@HOST:PORT/railway`

The connection string is automatically created by Railway when you add PostgreSQL!

