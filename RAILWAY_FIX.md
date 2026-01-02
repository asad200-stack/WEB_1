# Railway Deployment Fix Guide

## Quick Fix Steps

### 1. Add PostgreSQL Database
- In Railway dashboard, click **"+ New"** → **"Database"** → **"Add PostgreSQL"**
- Railway will automatically create the database
- The `DATABASE_URL` will be available automatically

### 2. Add Environment Variables

Go to **WEB_1** service → **Variables** tab → Click **"+ New Variable"**

Add these variables one by one:

```
DATABASE_URL=<automatically-provided-by-railway>
JWT_SECRET=<generate-random-secret>
JWT_EXPIRES_IN=7d
NODE_ENV=production
MAX_FILE_SIZE=5242880
UPLOAD_DIR=./public/uploads
PLATFORM_NAME=Multi-Store Platform
SUPER_ADMIN_EMAIL=admin@platform.com
SUPER_ADMIN_PASSWORD=admin123456
```

**To generate JWT_SECRET:**
- Use Railway's "Generate" button, OR
- Use: `openssl rand -base64 32` in terminal

### 3. Redeploy

After adding variables:
1. Go to **Deployments** tab
2. Click **"Redeploy"** or push a new commit to trigger rebuild
3. Wait for build to complete

### 4. Run Database Migrations

After successful deployment:

1. Go to **Deployments** → Latest deployment
2. Click **"Run Command"**
3. Enter: `npx prisma migrate deploy`
4. Click **"Run"**

### 5. Create Super Admin

1. In same "Run Command" interface
2. Enter: `node scripts/create-admin.js admin@platform.com admin123456`
3. Click **"Run"**

### 6. Access Your Platform

- Your app URL: Check Railway dashboard for the generated URL
- Super Admin Login: `https://your-app.railway.app/admin/login`
- Credentials:
  - Email: `admin@platform.com`
  - Password: `admin123456`

## Common Issues

### Build Still Fails?
- Check build logs in "Deployments" tab
- Ensure DATABASE_URL is set correctly
- Verify all environment variables are added

### Database Connection Error?
- Verify DATABASE_URL format: `postgresql://user:password@host:port/dbname`
- Check database service is running
- Ensure database is in same project

### Prisma Errors?
- Run: `npx prisma generate` in Railway command
- Then run: `npx prisma migrate deploy`

## Need Help?

Check the full deployment guide in `DEPLOYMENT.md`

