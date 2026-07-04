# CelebWeave Deployment Guide 🚀

Complete step-by-step instructions for deploying CelebWeave to production.

---

## Table of Contents
- [Vercel (Recommended)](#vercel-recommended)
- [GitHub Pages (Free)](#github-pages-free)
- [Netlify](#netlify)
- [Custom Server](#custom-server)
- [Docker](#docker)
- [Troubleshooting](#troubleshooting)

---

## Vercel (Recommended) ⭐

**Best for:** Production deployments, custom domains, unlimited deploys

### Step 1: Prepare GitHub Repo

```bash
# 1. Create local git repo
cd celebweave
git init

# 2. Add all files
git add .

# 3. Commit
git commit -m "CelebWeave MVP - Initial deployment"

# 4. Create GitHub repo at github.com/new
# Name it: celebweave
# Choose: Public (recommended for portfolio)

# 5. Add remote and push
git remote add origin https://github.com/YOUR_USERNAME/celebweave.git
git branch -M main
git push -u origin main
```

### Step 2: Deploy on Vercel

```bash
# Option A: Web UI (Easiest)
# 1. Go to vercel.com
# 2. Click "New Project"
# 3. Click "Import Git Repository"
# 4. Select your GitHub repo (celebrate)
# 5. Click "Import"
# 6. Keep default settings
# 7. Click "Deploy"
# 8. Wait ~30 seconds
# 9. Visit your live URL (shown in dashboard)

# Option B: CLI
npm i -g vercel          # Install Vercel CLI
cd celebweave
vercel                   # Follow prompts, link GitHub repo
# Done! Live at vercel.com dashboard
```

### Step 3: Add Custom Domain (Optional)

```
In Vercel Dashboard:
1. Go to Settings → Domains
2. Add your domain
3. Update DNS settings at domain registrar
4. Wait for SSL certificate (5-10 mins)
5. Done!
```

### Your Live Site
- Default: `https://celebweave.vercel.app`
- Custom: `https://yourdomainname.com`

---

## GitHub Pages (Free)

**Best for:** Portfolio, free hosting, GitHub-based

### Step 1: Push to GitHub

```bash
cd celebweave
git init
git add .
git commit -m "CelebWeave MVP"
git remote add origin https://github.com/YOUR_USERNAME/celebweave.git
git branch -M main
git push -u origin main
```

### Step 2: Enable GitHub Pages

```
On GitHub:
1. Go to repo Settings
2. Scroll to "Pages" (left sidebar)
3. Under "Source", select: Branch → main
4. Folder: / (root)
5. Click Save
6. Wait 1-2 minutes
7. You'll see: "Your site is live at https://YOUR_USERNAME.github.io/celebweave"
```

### Step 3: View Your Site

```
Visit: https://YOUR_USERNAME.github.io/celebweave
```

### Make Changes

```bash
# Edit celebweave-premium-full.html locally
# Then:
git add .
git commit -m "Update features"
git push origin main

# GitHub Pages auto-updates in 1-2 minutes!
```

---

## Netlify

**Best for:** Fast deployments, form handling, Jamstack features

### Step 1: Push to GitHub (See above)

### Step 2: Connect to Netlify

```
On netlify.com:
1. Click "New site from Git"
2. Select "GitHub"
3. Authorize Netlify
4. Select repo "celebweave"
5. Keep default build settings
6. Click "Deploy site"
7. Wait for green checkmark
8. Your URL: https://celebweave-xxx.netlify.app
```

### Step 3: Custom Domain (Optional)

```
In Netlify:
1. Domain settings
2. Add custom domain
3. Update DNS
4. Done!
```

### Auto-Deploy on Push

Every time you push to GitHub, Netlify auto-deploys! ✨

---

## Custom Server

**Best for:** Full control, existing hosting

### Step 1: Download Files

```bash
# Option A: Git clone
git clone https://github.com/YOUR_USERNAME/celebweave.git

# Option B: Download ZIP from GitHub
# Click "Code" → "Download ZIP" → Extract
```

### Step 2: Upload to Server

```bash
# Via FTP/SFTP:
# 1. Connect to your server
# 2. Upload files to public_html/ or www/
# 3. Make sure celebweave-premium-full.html is in root

# Via Terminal:
scp -r celebweave/ user@yourserver.com:/public_html/
```

### Step 3: Set Permissions

```bash
# SSH into server
ssh user@yourserver.com

# Navigate to site folder
cd /public_html/celebweave/

# Set proper permissions
chmod 644 *.html *.css *.js
chmod 755 ./

# Verify index.html exists or rename:
# If main file isn't called index.html:
ln -s celebweave-premium-full.html index.html
```

### Step 4: Enable HTTPS (Recommended)

```bash
# Most hosting providers offer free SSL via Let's Encrypt
# cPanel: AutoSSL feature
# Linode: Install Certbot
# DigitalOcean: Use Nginx/Apache with Let's Encrypt

# After SSL:
# Your site: https://yourdomain.com
```

### Step 5: Visit Your Site

```
https://yourdomain.com/celebweave
# or if in root:
https://yourdomain.com
```

---

## Docker

**Best for:** Containerized deployments, scaling

### Step 1: Create Dockerfile

```dockerfile
# Dockerfile
FROM nginx:alpine

# Copy HTML file to nginx
COPY celebweave-premium-full.html /usr/share/nginx/html/index.html

# Copy assets if you have any
COPY assets/ /usr/share/nginx/html/assets/

# Expose port
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
```

### Step 2: Build Image

```bash
docker build -t celebweave:latest .
```

### Step 3: Run Container

```bash
docker run -p 80:80 celebweave:latest
# Visit: http://localhost
```

### Step 4: Push to Docker Hub (Optional)

```bash
docker tag celebweave YOUR_USERNAME/celebweave
docker push YOUR_USERNAME/celebweave

# Now anyone can run:
docker run -p 80:80 YOUR_USERNAME/celebweave
```

---

## Deployment Comparison

| Feature | Vercel | GitHub Pages | Netlify | Custom Server |
|---------|--------|--------------|---------|---------------|
| **Price** | Free | Free | Free | Varies |
| **Speed** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Ease** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Custom Domain** | Yes | Yes | Yes | Yes |
| **SSL/HTTPS** | Auto | Auto | Auto | Manual |
| **Analytics** | Built-in | No | Built-in | Via GA |
| **Best For** | Production | Portfolio | Jamstack | Full Control |

---

## Domain Setup

### Buy a Domain
```
Options:
- Namecheap.com
- GoDaddy.com
- Google Domains (google.com/domains)
- Name.com
```

### Point Domain to Vercel
```
1. Buy domain (e.g., celebweave.com)
2. In Vercel dashboard → Settings → Domains
3. Add your domain: celebweave.com
4. Vercel will show nameservers
5. Update at your domain registrar
6. Wait 5-15 minutes for DNS propagation
7. Your site is now at https://celebweave.com
```

### Point Domain to GitHub Pages
```
1. In repo Settings → Pages
2. Under "Custom domain", enter: celebweave.com
3. GitHub shows DNS settings
4. Update at domain registrar
5. Done!
```

---

## CI/CD (Automatic Deploys)

### GitHub Actions (Free)

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
```

Now every push to `main` = automatic deploy! 🚀

---

## Environment Variables

### For Backend (When Built)

Create `.env` file:
```
DATABASE_URL=mongodb://...
STRIPE_KEY=sk_live_...
JWT_SECRET=your-secret-key
EMAIL_FROM=noreply@celebweave.com
```

### In Vercel Dashboard
```
1. Project Settings → Environment Variables
2. Add each variable
3. Values are encrypted
4. Redeploy to apply
```

---

## Monitoring & Logs

### Vercel Analytics
```
In Vercel Dashboard:
- Analytics tab shows: page views, response times, etc.
- Logs show: build status, deploy history
- Errors show: 404s, crashes, performance issues
```

### Google Analytics
```html
<!-- Add to bottom of celebweave-premium-full.html -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_ID');
</script>
```

---

## Troubleshooting

### Site Not Loading
```
1. Check domain DNS propagation: whatsmydns.net
2. Clear browser cache (Ctrl+Shift+Del)
3. Try incognito/private window
4. Check file permissions (chmod 644)
5. View server logs
```

### Slow Performance
```
1. Enable Gzip compression
2. Minify CSS/JS
3. Lazy-load images
4. Use CDN for assets (already done with Tailwind)
5. Check Lighthouse score in DevTools
```

### 404 Errors
```
1. Make sure index.html exists in root
2. If file is celebweave-premium-full.html, rename it
3. Check file upload was complete
4. Verify file permissions
```

### HTTPS Issues
```
1. Wait 24-48 hours for SSL certificate
2. Clear browser cache
3. Use https:// in URLs (not http)
4. Check certificate in browser (🔒 icon)
```

### After Deploy, Changes Not Showing
```
1. Hard refresh: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
2. Clear browser cache
3. Check deployment completed (green checkmark)
4. Wait 5-10 minutes for CDN cache to clear
```

---

## Performance Tips

### Before Deployment
1. **Minify CSS/JS** (for production)
2. **Optimize images** (use WebP, compress)
3. **Enable Gzip** compression on server
4. **Set cache headers** (1 year for assets)

### After Deployment
1. **Monitor Lighthouse** score (aim for 90+)
2. **Check Core Web Vitals** (Google PageSpeed Insights)
3. **Set up CDN** for faster global delivery
4. **Use analytics** to track real user performance

---

## Rollback / Undo Deploy

### Vercel
```
In Dashboard → Deployments → Select old version → Redeploy
```

### GitHub Pages
```
git revert COMMIT_HASH
git push origin main
# Auto-redeploys
```

### Netlify
```
Deploy log → Select previous deploy → Publish
```

---

## Testing Before Deploy

### Local Testing
```bash
# 1. Open in browser locally
open celebweave-premium-full.html

# 2. Test all links work
# 3. Check forms (no validation needed yet)
# 4. Test mobile responsiveness
#    Ctrl+Shift+I → Device Toolbar
# 5. Test on different devices:
#    - iPhone, iPad, Android, Desktop
```

### Lighthouse Audit
```
In Chrome DevTools:
1. Right-click → Inspect
2. Click "Lighthouse" tab
3. Click "Generate report"
4. Check: Performance, Accessibility, Best Practices
5. Aim for 90+ on all metrics
```

### Cross-Browser Testing
```
Test on:
- Chrome/Chromium ✓
- Firefox ✓
- Safari ✓
- Edge ✓
- Mobile Safari (iOS) ✓
- Chrome (Android) ✓
```

---

## Production Checklist

Before going live, verify:

- [ ] All links work
- [ ] Mobile responsive tested
- [ ] Forms ready for backend
- [ ] Images load correctly
- [ ] No console errors (F12 → Console)
- [ ] Lighthouse score 90+
- [ ] SSL/HTTPS enabled
- [ ] Domain points correctly
- [ ] Analytics tracking set up
- [ ] Backup copy saved locally
- [ ] Documentation complete
- [ ] Team notified of launch

---

## Post-Deployment

### Tell the World!
```
- Share on Twitter: @your_handle
- LinkedIn post
- Share in communities
- Add to portfolio
- Send to clients
```

### Monitor Performance
```
1. Check Lighthouse daily for 1 week
2. Monitor error logs
3. Watch analytics
4. Fix any broken links
5. Update with feedback
```

### Next Steps
```
1. Gather client feedback
2. Plan v1.1 features (backend)
3. Set up development environment
4. Start backend API development
5. Plan mobile app
```

---

## Need Help?

- 📧 **Email:** support@bodrock.io
- 📚 **Docs:** Check README.md
- 🐛 **Issues:** GitHub Issues
- 💬 **Community:** Discord/WhatsApp

---

**Happy Deploying! 🎉**

Your CelebWeave instance will be live and production-ready within minutes!

**Last Updated:** January 2025
