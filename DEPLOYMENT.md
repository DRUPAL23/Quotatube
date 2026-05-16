# Deployment Guide for Quotatube

Complete step-by-step guide to deploy Quotatube on Vercel, Netlify, or GitHub Pages.

## Prerequisites

- GitHub repository with code pushed
- Anthropic API key from [console.anthropic.com](https://console.anthropic.com/)

---

## Option 1: Vercel Deployment (Recommended)

Vercel is the creator of Next.js and works perfectly with Vite + React.

### Steps

1. **Go to [vercel.com](https://vercel.com/dashboard)**
2. **Sign up/Login with GitHub**
3. **Click "Add New" → "Project"**
4. **Select your `Quotatube` repository**
5. **Configure Project Settings:**
   - Framework Preset: `Vite`
   - Build Command: `npm run build`
   - Output Directory: `dist`
6. **Add Environment Variables:**
   - Click "Environment Variables"
   - Add: `VITE_ANTHROPIC_API_KEY` = your API key
7. **Click "Deploy"**

### After Deployment

- Your app is live at `quotatube.vercel.app`
- Every push to `main` auto-deploys
- Preview deployments for each PR

---

## Option 2: Netlify Deployment

Netlify also works great with Vite and has excellent developer experience.

### Steps

1. **Go to [netlify.com](https://app.netlify.com/)**
2. **Sign up/Login with GitHub**
3. **Click "Add new site" → "Import an existing project"**
4. **Select your `Quotatube` repository**
5. **Configure Build Settings:**
   - Build command: `npm run build`
   - Publish directory: `dist`
6. **Click "Deploy site"**
7. **Add Environment Variables:**
   - Go to Site Settings → Build & Deploy → Environment
   - Add: `VITE_ANTHROPIC_API_KEY` = your API key

### After Deployment

- Your app is live at `quotatube-xxx.netlify.app`
- Every push to `main` auto-deploys
- PR preview deployments available

---

## Option 3: GitHub Pages Deployment

Free hosting directly from your GitHub repository (public only).

### Steps

1. **Update `package.json`:**
```json
{
  "homepage": "https://DRUPAL23.github.io/Quotatube"
}
```

2. **Install gh-pages:**
```bash
npm install --save-dev gh-pages
```

3. **Add deploy scripts to `package.json`:**
```json
{
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist"
  }
}
```

4. **Enable GitHub Pages:**
   - Go to your repo → Settings → Pages
   - Source: Deploy from a branch
   - Branch: `gh-pages`, folder: `/`

5. **Deploy:**
```bash
npm run deploy
```

### After Deployment

- Your app is live at `https://DRUPAL23.github.io/Quotatube`
- Manual deploy needed (no auto-deploy like Vercel/Netlify)
- ⚠️ Note: API key visible in browser (not recommended for production)

---

## Environment Variables Setup

### For Vercel

1. Go to your project settings
2. Navigate to "Environment Variables"
3. Add new variable:
   - Name: `VITE_ANTHROPIC_API_KEY`
   - Value: Your API key from anthropic.com
4. Select environments: Production, Preview, Development
5. Save

### For Netlify

1. Go to Site Settings
2. Click "Build & Deploy" → "Environment"
3. Click "Edit variables"
4. Add new variable:
   - Key: `VITE_ANTHROPIC_API_KEY`
   - Value: Your API key from anthropic.com
5. Save

### For GitHub Pages

⚠️ **WARNING:** GitHub Pages doesn't support private environment variables. Your API key will be exposed.

**Solution:** Create a backend proxy server to handle API calls.

---

## GitHub Actions CI/CD

The `.github/workflows/deploy.yml` file automates:

1. **Build on every push to `main`/`develop`**
2. **Run tests (if any)**
3. **Deploy to Vercel** (if build succeeds)
4. **Deploy to Netlify** (if build succeeds)
5. **Deploy to GitHub Pages** (if build succeeds)

### Required GitHub Secrets

Add these secrets to your repository (Settings → Secrets → Actions):

```
VERCEL_TOKEN              # From vercel.com/account/tokens
VERCEL_ORG_ID             # Your Vercel organization ID
VERCEL_PROJECT_ID         # Your Vercel project ID
NETLIFY_AUTH_TOKEN        # From netlify.com/user/applications
NETLIFY_SITE_ID           # Your Netlify site ID
VITE_ANTHROPIC_API_KEY    # Your Anthropic API key
```

### Getting These Values

**Vercel:**
1. Go to [vercel.com/account/tokens](https://vercel.com/account/tokens)
2. Create new token
3. Copy and paste as `VERCEL_TOKEN`
4. Get Org ID and Project ID from your project settings

**Netlify:**
1. Go to [app.netlify.com/user/applications](https://app.netlify.com/user/applications)
2. Create new personal access token
3. Copy and paste as `NETLIFY_AUTH_TOKEN`
4. Get Site ID from Site Settings

---

## Troubleshooting

### Build fails with "Cannot find module 'react'"

```bash
npm install
npm run build
```

### Environment variables not working

Check:
1. Variables are prefixed with `VITE_` (required for Vite)
2. Variables are added to deployment platform settings
3. Redeploy after adding variables

### API key errors at runtime

1. Verify API key is correct in deployment settings
2. Check that environment variable name is `VITE_ANTHROPIC_API_KEY`
3. Ensure API key has proper permissions from Anthropic

### Deploy workflow not triggering

1. Ensure you're pushing to `main` branch
2. Check GitHub Actions are enabled in repo settings
3. Verify secrets are added to repo
4. Check workflow file in `.github/workflows/deploy.yml`

---

## Production Best Practices

### 1. Never commit API keys

Always use environment variables, never hardcode keys.

### 2. Use a backend proxy

For production, create a backend service that:
- Receives requests from frontend
- Adds API key on backend
- Calls Claude API
- Returns response to frontend

Example Node.js backend:
```javascript
app.post('/api/generate', async (req, res) => {
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    headers: { 'x-api-key': process.env.ANTHROPIC_API_KEY }
  });
  res.json(response);
});
```

### 3. Set up monitoring

Use deployment platform's analytics:
- Monitor error rates
- Track performance metrics
- Set up alerts for failures

### 4. Enable auto-scaling

Ensure your hosting can handle traffic spikes:
- Vercel: Auto-scales automatically
- Netlify: Auto-scales automatically
- GitHub Pages: Static files only (no scaling needed)

---

## Custom Domain Setup

### For Vercel

1. Go to project settings
2. Click "Domains"
3. Add your custom domain
4. Follow DNS configuration

### For Netlify

1. Go to Site Settings → Domain management
2. Click "Add custom domain"
3. Follow DNS configuration

---

## Monitoring & Analytics

### Vercel Analytics
- Automatic Web Vitals tracking
- Performance monitoring
- Error tracking

### Netlify Analytics
- Site analytics dashboard
- Build performance tracking
- Function performance

---

## Next Steps

After deployment:

1. ✅ Test all features in production
2. ✅ Share with users
3. ✅ Monitor errors and performance
4. ✅ Plan security improvements (backend API)
5. ✅ Consider adding authentication
6. ✅ Set up custom domain

---

**Happy deploying! 🚀**
