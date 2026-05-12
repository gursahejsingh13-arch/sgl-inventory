# 🚀 Signalist Stock Tracker - Complete Setup Guide

This guide will walk you through setting up the Signalist Stock Tracker application from scratch, including all required services, API keys, and configurations.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Quick Start](#quick-start)
3. [Detailed Setup](#detailed-setup)
   - [Step 1: Clone Repository](#step-1-clone-repository)
   - [Step 2: Install Dependencies](#step-2-install-dependencies)
   - [Step 3: MongoDB Setup](#step-3-mongodb-setup)
   - [Step 4: Finnhub API Setup](#step-4-finnhub-api-setup)
   - [Step 5: Gemini AI Setup](#step-5-gemini-ai-setup)
   - [Step 6: Gmail/Nodemailer Setup](#step-6-gmailnodemailer-setup)
   - [Step 7: Better Auth Configuration](#step-7-better-auth-configuration)
   - [Step 8: Inngest Setup](#step-8-inngest-setup)
   - [Step 9: Environment Variables](#step-9-environment-variables)
   - [Step 10: Run Application](#step-10-run-application)
4. [Verification & Testing](#verification--testing)
5. [Troubleshooting](#troubleshooting)
6. [Production Deployment](#production-deployment)

---

## ✅ Prerequisites

Before you begin, make sure you have the following installed:

- **Node.js** (v20.0.0 or higher) - [Download](https://nodejs.org/)
- **npm** (v10.0.0 or higher) - Comes with Node.js
- **Git** - [Download](https://git-scm.com/)
- **Code Editor** (VS Code recommended) - [Download](https://code.visualstudio.com/)

**Required Accounts:**
- MongoDB Atlas (free tier available)
- Finnhub (free API key)
- Google AI Studio (for Gemini API)
- Gmail account (for sending emails)
- Inngest (free tier available)

---

## 🏃 Quick Start

For experienced developers, here's the quick setup:

```bash
# Clone repository
git clone https://github.com/adrianhajdin/signalist_stock-tracker-app.git
cd signalist_stock-tracker-app

# Install dependencies
npm install

# Create .env file (see Step 9 for details)
# Add all required environment variables

# Run development server
npm run dev

# Run Inngest dev server (in separate terminal)
npx inngest-cli@latest dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 📖 Detailed Setup

### Step 1: Clone Repository

**1.1. Open Terminal/Command Prompt**

**1.2. Navigate to your desired directory:**
```bash
cd G:\YT
```

**1.3. Clone the repository:**
```bash
git clone https://github.com/adrianhajdin/signalist_stock-tracker-app.git
```

**1.4. Navigate into project folder:**
```bash
cd signalist_stock-tracker-app
```

**1.5. Open in VS Code (optional):**
```bash
code .
```

---

### Step 2: Install Dependencies

**2.1. Install all Node.js packages:**
```bash
npm install
```

This will install all dependencies listed in `package.json`, including:
- Next.js 15.5.2
- React 19.1.0
- TypeScript
- TailwindCSS
- Better Auth
- Mongoose
- Inngest
- Nodemailer
- And all other dependencies

**2.2. Verify installation:**
```bash
npm list --depth=0
```

You should see a list of all installed packages without errors.

**2.3. Install Shadcn UI components (if not already installed):**
```bash
npx shadcn@latest add button dialog dropdown-menu command input label popover select avatar sonner
```

---

### Step 3: MongoDB Setup

MongoDB will store user accounts, sessions, and watchlist data.

**3.1. Create MongoDB Atlas Account**

1. Go to [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Click "Start Free" or "Try Free"
3. Sign up with email or Google account
4. Complete registration

**3.2. Create a Cluster**

1. After login, click "Build a Database"
2. Choose **FREE** tier (M0)
3. Select **Cloud Provider**: AWS (recommended)
4. Select **Region**: Choose closest to you (e.g., us-east-1)
5. Click "Create"
6. Wait 3-5 minutes for cluster creation

**3.3. Create Database User**

1. In "Security" tab, click "Database Access"
2. Click "Add New Database User"
3. Choose "Password" authentication
4. Set username: `signalist_user` (or your choice)
5. Generate strong password or create your own
6. **SAVE THIS PASSWORD** - you'll need it for connection string
7. Set privileges: "Read and write to any database"
8. Click "Add User"

**3.4. Whitelist IP Address**

1. Go to "Network Access" tab
2. Click "Add IP Address"
3. For development: Click "Allow Access from Anywhere" (0.0.0.0/0)
4. For production: Add specific IP addresses
5. Click "Confirm"

**3.5. Get Connection String**

1. Go to "Database" tab
2. Click "Connect" on your cluster
3. Choose "Connect your application"
4. Driver: **Node.js**
5. Version: **5.5 or later**
6. Copy the connection string:
   ```
   mongodb+srv://signalist_user:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
7. Replace `<password>` with your actual password
8. Add database name after `.net/`: `signalist`
   ```
   mongodb+srv://signalist_user:yourpassword@cluster0.xxxxx.mongodb.net/signalist?retryWrites=true&w=majority
   ```

**3.6. Save for Later**

You'll add this connection string to your `.env` file as `MONGODB_URI`.

---

### Step 4: Finnhub API Setup

Finnhub provides real-time stock market data, news, and company information.

**4.1. Create Finnhub Account**

1. Go to [https://finnhub.io/register](https://finnhub.io/register)
2. Sign up with email
3. Verify your email address
4. Log in to dashboard

**4.2. Get API Key**

1. After login, go to [https://finnhub.io/dashboard](https://finnhub.io/dashboard)
2. Your API Key is displayed on the dashboard
3. Copy the API key (looks like: `c1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6`)

**4.3. Free Tier Limits**

- **60 API calls per minute**
- Data delayed by ~15 minutes
- Upgrade for real-time data

**4.4. Test API Key (Optional)**

Test in browser or Postman:
```
https://finnhub.io/api/v1/quote?symbol=AAPL&token=YOUR_API_KEY
```

You should see JSON response with Apple stock data.

**4.5. Save for Later**

You'll add this to your `.env` file as `NEXT_PUBLIC_FINNHUB_API_KEY`.

---

### Step 5: Gemini AI Setup

Gemini AI generates personalized content for welcome emails and news summaries.

**5.1. Access Google AI Studio**

1. Go to [https://aistudio.google.com](https://aistudio.google.com)
2. Sign in with your Google account
3. Accept terms of service

**5.2. Create API Key**

1. Click "Get API Key" in the top right
2. Click "Create API Key"
3. Choose "Create API key in new project" (or select existing project)
4. Wait a few seconds
5. Copy the API key (looks like: `AIzaSy...`)

**5.3. API Limits (Free Tier)**

- **15 requests per minute**
- **1,500 requests per day**
- Upgrade for higher limits

**5.4. Test API Key (Optional)**

Use this curl command:
```bash
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent?key=YOUR_API_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"contents":[{"parts":[{"text":"Hello"}]}]}'
```

**5.5. Save for Later**

You'll add this to your `.env` file as `GEMINI_API_KEY`.

---

### Step 6: Gmail/Nodemailer Setup

Gmail will be used to send welcome emails and daily news summaries.

**6.1. Prepare Gmail Account**

Use an existing Gmail account or create a new one at [gmail.com](https://gmail.com).

**6.2. Enable 2-Factor Authentication**

1. Go to [https://myaccount.google.com/security](https://myaccount.google.com/security)
2. Click "2-Step Verification"
3. Follow prompts to enable (verify with phone)

**6.3. Generate App Password**

1. Go to [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
2. Sign in if prompted
3. In "Select app" dropdown: Choose "Mail"
4. In "Select device" dropdown: Choose "Other (Custom name)"
5. Enter name: "Signalist App"
6. Click "Generate"
7. Copy the 16-character password (looks like: `abcd efgh ijkl mnop`)
8. **Important:** Remove spaces, use as: `abcdefghijklmnop`

**6.4. Save for Later**

You'll add these to your `.env` file:
- `NODEMAILER_EMAIL` = your-email@gmail.com
- `NODEMAILER_PASSWORD` = app password (without spaces)

**6.5. Alternative Email Services**

If you don't want to use Gmail:
- **SendGrid** - [sendgrid.com](https://sendgrid.com)
- **Mailgun** - [mailgun.com](https://mailgun.com)
- **AWS SES** - [aws.amazon.com/ses](https://aws.amazon.com/ses)

You'll need to modify `lib/nodemailer/index.ts` accordingly.

---

### Step 7: Better Auth Configuration

Better Auth handles user authentication and sessions.

**7.1. Generate Secret Key**

You need a random 32-character string for `BETTER_AUTH_SECRET`.

**Option A: Use Node.js (recommended)**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

**Option B: Use OpenSSL**
```bash
openssl rand -hex 32
```

**Option C: Use Online Generator**
Go to [https://generate-random.org/api-key-generator](https://generate-random.org/api-key-generator) and generate a 32-character key.

Copy the generated string (looks like: `a1b2c3d4e5f6...`)

**7.2. Set Base URL**

For development:
```
BETTER_AUTH_URL=http://localhost:3000
```

For production:
```
BETTER_AUTH_URL=https://yourdomain.com
```

**7.3. Save for Later**

You'll add these to your `.env` file:
- `BETTER_AUTH_SECRET` = your generated secret
- `BETTER_AUTH_URL` = your base URL

---

### Step 8: Inngest Setup

Inngest handles background jobs and event-driven workflows.

**8.1. Create Inngest Account**

1. Go to [https://www.inngest.com](https://www.inngest.com)
2. Click "Sign Up"
3. Sign up with GitHub or email
4. Complete registration

**8.2. Create New App**

1. After login, click "Create App"
2. App name: "Signalist"
3. Click "Create"

**8.3. Development Setup**

For local development, you'll run the Inngest dev server:

**8.4. Install Inngest CLI (if not already installed)**
```bash
npm install -g inngest-cli
```

**8.5. Production Setup**

1. In Inngest dashboard, go to "Apps" → "Signalist"
2. Go to "Webhooks" section
3. Add webhook URL: `https://yourdomain.com/api/inngest`
4. Functions will auto-register on first deployment

**8.6. No API Key Required**

Inngest doesn't require an API key in `.env` for this setup.

---

### Step 9: Environment Variables

**9.1. Create `.env` File**

In the root of your project (`signalist_stock-tracker-app/`), create a file named `.env`.

**9.2. Add All Environment Variables**

Copy and paste this template, replacing values with your actual credentials:

```env
# ===========================================
# ENVIRONMENT
# ===========================================
NODE_ENV=development
NEXT_PUBLIC_BASE_URL=http://localhost:3000


# ===========================================
# FINNHUB API
# ===========================================
NEXT_PUBLIC_FINNHUB_API_KEY=your_finnhub_api_key_here
FINNHUB_BASE_URL=https://finnhub.io/api/v1


# ===========================================
# MONGODB
# ===========================================
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/signalist?retryWrites=true&w=majority


# ===========================================
# BETTER AUTH
# ===========================================
BETTER_AUTH_SECRET=your_32_character_random_secret_here
BETTER_AUTH_URL=http://localhost:3000


# ===========================================
# GEMINI AI
# ===========================================
GEMINI_API_KEY=your_gemini_api_key_here


# ===========================================
# NODEMAILER (GMAIL)
# ===========================================
NODEMAILER_EMAIL=your-email@gmail.com
NODEMAILER_PASSWORD=your_gmail_app_password_here
```

**9.3. Example Filled `.env` File**

```env
NODE_ENV=development
NEXT_PUBLIC_BASE_URL=http://localhost:3000

NEXT_PUBLIC_FINNHUB_API_KEY=c1a2b3c4d5e6f7g8h9i0
FINNHUB_BASE_URL=https://finnhub.io/api/v1

MONGODB_URI=mongodb+srv://signalist_user:MyP@ssw0rd@cluster0.abc123.mongodb.net/signalist?retryWrites=true&w=majority

BETTER_AUTH_SECRET=a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6
BETTER_AUTH_URL=http://localhost:3000

GEMINI_API_KEY=AIzaSyD1E2F3G4H5I6J7K8L9M0N1O2P3

NODEMAILER_EMAIL=myemail@gmail.com
NODEMAILER_PASSWORD=abcdefghijklmnop
```

**9.4. Important Notes**

- **Never commit `.env` to Git** - It's already in `.gitignore`
- **Keep your keys secret** - Don't share publicly
- **Use different keys for production** - Separate dev and prod environments

---

### Step 10: Run Application

**10.1. Test Database Connection (Optional)**

```bash
npm run test:db
```

Expected output:
```
Connected to database development - mongodb+srv://...
Database connection test successful!
```

**10.2. Start Development Server**

Open your first terminal:
```bash
npm run dev
```

Expected output:
```
▲ Next.js 15.5.2 (turbopack)
- Local:        http://localhost:3000
- Network:      http://192.168.1.x:3000

✓ Ready in 2.3s
```

**10.3. Start Inngest Dev Server**

Open a **second terminal** (keep first one running):
```bash
npx inngest-cli@latest dev
```

Expected output:
```
Inngest dev server running!
- URL:     http://127.0.0.1:8288
- Webhook: http://localhost:3000/api/inngest
```

**10.4. Open Application**

Open your browser and go to:
```
http://localhost:3000
```

You should see the Signalist sign-in page!

---

## ✅ Verification & Testing

### Test 1: Sign Up

1. Go to [http://localhost:3000/sign-up](http://localhost:3000/sign-up)
2. Fill in all fields:
   - Full Name
   - Email
   - Password (minimum 8 characters)
   - Country
   - Investment Goals
   - Risk Tolerance
   - Preferred Industry
3. Click "Start Your Investing Journey"
4. Should redirect to dashboard
5. Check your email - you should receive a welcome email (may take 1-2 minutes)

### Test 2: Stock Search

1. On the dashboard, press `Ctrl+K` (or `Cmd+K` on Mac)
2. Type "AAPL" in the search box
3. Results should appear showing Apple Inc.
4. Click on a result to go to stock detail page

### Test 3: Watchlist

1. On a stock detail page (e.g., `/stocks/AAPL`)
2. Click "Add to Watchlist" button
3. Should see success toast notification
4. Button should change to "Remove from Watchlist"

### Test 4: Dashboard Widgets

1. Go back to dashboard (`/`)
2. Verify all TradingView widgets load:
   - Market Overview
   - Stock Heatmap
   - Timeline (news)
   - Market Quotes

### Test 5: Sign Out & Sign In

1. Click your profile avatar (top right)
2. Click "Sign Out"
3. Should redirect to `/sign-in`
4. Sign in with your credentials
5. Should redirect back to dashboard

### Test 6: Inngest Events (Optional)

1. Go to Inngest dev server: [http://127.0.0.1:8288](http://127.0.0.1:8288)
2. Check "Events" tab
3. Should see `app/user.created` event from your sign-up
4. Check "Functions" tab
5. Should see `sign-up-email` function execution

### Test 7: Daily News Email (Manual Trigger)

In a third terminal:
```bash
curl -X POST http://localhost:3000/api/inngest \
  -H "Content-Type: application/json" \
  -d '{"name":"app/send.daily.news","data":{}}'
```

Check your email - you should receive a daily news summary.

---

## 🐛 Troubleshooting

### Issue: "Cannot find module" errors

**Solution:**
```bash
rm -rf node_modules package-lock.json
npm install
```

### Issue: MongoDB connection failed

**Possible Causes:**
1. Incorrect connection string
2. Wrong username/password
3. IP address not whitelisted

**Solutions:**
- Verify `MONGODB_URI` in `.env`
- Check username and password (no special characters unencoded)
- In MongoDB Atlas, go to "Network Access" and add your IP or allow all (0.0.0.0/0)
- Test connection: `npm run test:db`

### Issue: Finnhub API "401 Unauthorized"

**Solution:**
- Verify `NEXT_PUBLIC_FINNHUB_API_KEY` in `.env`
- Check if key is correct in Finnhub dashboard
- Test API key: `curl "https://finnhub.io/api/v1/quote?symbol=AAPL&token=YOUR_KEY"`

### Issue: No welcome email received

**Possible Causes:**
1. Incorrect Gmail credentials
2. App password not generated
3. Inngest dev server not running
4. Gemini API rate limit

**Solutions:**
- Check `NODEMAILER_EMAIL` and `NODEMAILER_PASSWORD` in `.env`
- Ensure Inngest dev server is running: `npx inngest-cli@latest dev`
- Check spam/junk folder
- Check Inngest dev UI for errors: [http://127.0.0.1:8288](http://127.0.0.1:8288)

### Issue: TradingView widgets not loading

**Solutions:**
- Refresh the page
- Check browser console for JavaScript errors
- Clear browser cache
- Disable ad blockers

### Issue: "Session cookie not found" redirect loop

**Solutions:**
- Clear browser cookies
- Check `BETTER_AUTH_SECRET` in `.env`
- Check `BETTER_AUTH_URL` matches your current URL
- Restart dev server

### Issue: TypeScript errors during build

**Solutions:**
- Check `tsconfig.json` is correct
- Install missing type definitions: `npm install --save-dev @types/node @types/react`
- Verify all imports are correct

### Issue: Port 3000 already in use

**Solutions:**
- Kill process using port 3000:
  - Windows: `netstat -ano | findstr :3000` then `taskkill /PID <PID> /F`
  - Mac/Linux: `lsof -ti:3000 | xargs kill -9`
- Use different port: `PORT=3001 npm run dev`

---

## 🚀 Production Deployment

### Deploy to Vercel (Recommended)

**Step 1: Push to GitHub**
```bash
git add .
git commit -m "Initial commit"
git push origin main
```

**Step 2: Import to Vercel**
1. Go to [https://vercel.com](https://vercel.com)
2. Sign up/login with GitHub
3. Click "New Project"
4. Import `signalist_stock-tracker-app` repository
5. Click "Deploy"

**Step 3: Add Environment Variables**
1. In Vercel project settings, go to "Environment Variables"
2. Add all variables from your `.env` file:
   - `NODE_ENV` = `production`
   - `NEXT_PUBLIC_BASE_URL` = `https://your-project.vercel.app`
   - `NEXT_PUBLIC_FINNHUB_API_KEY` = your key
   - `MONGODB_URI` = your connection string
   - `BETTER_AUTH_SECRET` = your secret
   - `BETTER_AUTH_URL` = `https://your-project.vercel.app`
   - `GEMINI_API_KEY` = your key
   - `NODEMAILER_EMAIL` = your email
   - `NODEMAILER_PASSWORD` = your app password

**Step 4: Configure Inngest**
1. In Inngest dashboard, add webhook: `https://your-project.vercel.app/api/inngest`
2. Deploy will auto-register functions

**Step 5: Redeploy**
After adding environment variables, trigger a redeployment:
1. Go to "Deployments" tab
2. Click "Redeploy" on latest deployment

**Step 6: Test Production**
- Open `https://your-project.vercel.app`
- Test all features (sign-up, search, watchlist)

### Alternative Platforms

**Netlify:**
- Similar process to Vercel
- Import from GitHub, add environment variables, deploy

**Railway:**
- Add `railway.json` configuration
- Connect GitHub repository
- Add environment variables

**AWS Amplify:**
- Add `amplify.yml` configuration
- Connect GitHub repository
- Configure build settings

---

## 📞 Support & Resources

**Documentation:**
- Full project documentation: `PROJECT_DOCUMENTATION.md`
- Package requirements: `requirements.txt`

**Video Tutorial:**
- YouTube: [https://youtu.be/gu4pafNCXng](https://youtu.be/gu4pafNCXng)

**Community:**
- Discord (50k+ members): [https://discord.com/invite/n6EdbFJ](https://discord.com/invite/n6EdbFJ)

**Source Code:**
- GitHub: [https://github.com/adrianhajdin/signalist_stock-tracker-app](https://github.com/adrianhajdin/signalist_stock-tracker-app)

**Official Documentation:**
- Next.js: [https://nextjs.org/docs](https://nextjs.org/docs)
- MongoDB: [https://www.mongodb.com/docs](https://www.mongodb.com/docs)
- Finnhub: [https://finnhub.io/docs/api](https://finnhub.io/docs/api)
- Better Auth: [https://www.better-auth.com/docs](https://www.better-auth.com/docs)
- Inngest: [https://www.inngest.com/docs](https://www.inngest.com/docs)

---

## ✨ Next Steps

After successful setup:

1. **Explore the Code**
   - Review `PROJECT_DOCUMENTATION.md` for architecture details
   - Study component structure in `components/`
   - Review server actions in `lib/actions/`

2. **Customize**
   - Modify colors in `app/globals.css`
   - Change TradingView widget settings in `lib/constants.ts`
   - Add new features or pages

3. **Add More Features**
   - Implement full watchlist page
   - Add price alerts
   - Create admin dashboard
   - Add more chart types

4. **Production Checklist**
   - Enable proper error logging (Sentry)
   - Add analytics (Vercel Analytics)
   - Configure proper MongoDB indexes
   - Set up automated backups
   - Enable rate limiting

---

**Congratulations! 🎉**

You've successfully set up the Signalist Stock Tracker application. Happy coding!

---

**Last Updated:** January 2025  
**Version:** 1.0.0  
**Created By:** JavaScript Mastery / Adrian Hajdin
