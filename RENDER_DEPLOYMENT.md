# Render.com Deployment Guide

## Quick Start

This guide walks you through deploying Quilo-Quilo Laundry Management System to Render.com.

## Prerequisites

- A Render.com account (free tier available)
- SendGrid account (for email notifications)
- Twilio account (for SMS notifications)
- Your GitHub repository with this code

## Step 1: Create Environment Variables on Render.com

When creating a new web service on Render.com:

1. Go to your service dashboard
2. Navigate to **Settings** → **Environment**
3. Add the following environment variables:

### Required Environment Variables

```
PORT=3000
NODE_ENV=production
DATABASE_URL=./database/database.db
SENDGRID_API_KEY=SG.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_TOKEN=your_twilio_auth_token_here
TWILIO_PHONE=+1234567890
DEFAULT_COUNTRY_CODE=US
LOG_LEVEL=info
```

## Step 2: Get Your API Keys

### SendGrid API Key
1. Visit https://app.sendgrid.com/settings/api_keys
2. Create a new API key
3. Copy the key (you can only view it once)
4. Paste as `SENDGRID_API_KEY` in Render.com

### Twilio Credentials
1. Visit https://www.twilio.com/console
2. Find your Account SID and Auth Token
3. Copy both values:
   - Paste Account SID as `TWILIO_SID`
   - Paste Auth Token as `TWILIO_TOKEN`
4. Find your Twilio phone number and paste as `TWILIO_PHONE`

## Step 3: Build Configuration

Render.com should auto-detect this as a Node.js application.

**Build Command:**
```bash
npm install
npm run build
```

**Start Command:**
```bash
npm start
```

## Step 4: Deploy

1. Connect your GitHub repository to Render.com
2. Select the branch to deploy (usually `main`)
3. Click "Deploy"
4. Render will build and start your app

Your app will be available at: `https://your-service-name.onrender.com`

## Features Enabled on Render.com

### Phone Verification
- Uses libphonenumber-js to validate phone numbers before API calls
- Prevents crashes from invalid customer phone entries
- Automatically detects country code and digit count

### Atomic Database Updates
- Uses SQLite transactions to prevent race conditions
- Two concurrent requests at the same millisecond won't result in "phantom stock"
- Ensures inventory accuracy for high-concurrency scenarios

### Inventory Dashboard with Alerts
- **Stock Alerts**: Real-time dashboard showing `daysRemaining`
- **Red Warnings**: Items with less than 3 days remaining are highlighted in red
- **Predictions**: Linear regression forecasts when stock will run out
- **Mobile Ready**: Fully responsive design works on all devices

## Monitoring

1. Use Render.com's built-in logs:
   - Go to **Logs** tab to view real-time logs
   - Check for any errors or warnings

2. Monitor your services:
   - SendGrid: Check delivery reports in SendGrid dashboard
   - Twilio: Monitor SMS delivery in Twilio console

## Environment Variable Security

⚠️ **IMPORTANT**: Never commit `.env` to GitHub
- The `.env` file contains sensitive API keys
- Use `.env.example` as a template instead
- Render.com provides secure environment variable storage

## Scaling

As your business grows:

1. **Database**: Consider migrating from SQLite to PostgreSQL
2. **Storage**: Use Render's PostgreSQL add-on
3. **Backups**: Enable automatic backups in Render.com dashboard

## Troubleshooting

### Build fails
- Check that `package.json` has all dependencies
- Verify Node.js version compatibility
- Check build logs for specific errors

### App crashes after deploy
- Check environment variables are set correctly
- Review Render.com logs for error messages
- Verify API keys are valid (SendGrid, Twilio)

### Phone validation not working
- Ensure libphonenumber-js is installed: `npm install libphonenumber-js`
- Check that request includes valid phone number format

### Inventory alerts not showing
- Verify inventory items have `monthlyUsage` value
- Check that `/api/inventory/predict/:id` endpoint is responding
- Look for JavaScript errors in browser console

## Support

- Render.com Documentation: https://render.com/docs
- SendGrid Support: https://support.sendgrid.com
- Twilio Support: https://www.twilio.com/help
