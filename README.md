# Pablo Marketing Site - Cloudflare Pages Deployment

This directory contains a standalone static marketing page that will be deployed to Cloudflare Pages and served at `gopablo.net` and `www.gopablo.net`.

## Files

- `index.html` - Complete standalone marketing page with inline Tailwind CSS

## Deployment Instructions

### Option 1: Deploy via Cloudflare Dashboard (Recommended)

1. **Create GitHub Repository (Optional but Recommended)**
   ```bash
   # From the marketing-site directory
   git init
   git add .
   git commit -m "Initial marketing site"
   gh repo create pablo-marketing --public --source=. --remote=origin --push
   ```

2. **Deploy to Cloudflare Pages**
   - Log into [Cloudflare Dashboard](https://dash.cloudflare.com/)
   - Go to: **Workers & Pages** → **Create application** → **Pages** → **Connect to Git**
   - Select your repository: `pablo-marketing`
   - Configure build settings:
     - **Production branch**: `main`
     - **Build command**: (leave empty - it's static HTML)
     - **Build output directory**: `/`
     - **Root directory**: `/` (or specify if in subfolder)
   - Click **Save and Deploy**

3. **Configure Custom Domain**
   - After deployment, go to: **Custom domains**
   - Click **Set up a custom domain**
   - Enter: `gopablo.net`
   - Cloudflare will auto-configure DNS (CNAME record)
   - Add another custom domain: `www.gopablo.net`
   - Cloudflare handles SSL automatically

### Option 2: Deploy via Wrangler CLI

1. **Install Wrangler**
   ```bash
   npm install -g wrangler
   wrangler login
   ```

2. **Deploy**
   ```bash
   cd marketing-site
   wrangler pages deploy . --project-name=pablo-marketing
   ```

3. **Configure custom domain via dashboard** (same as Option 1, step 3)

### Option 3: Direct Upload (Quick Test)

1. Log into Cloudflare Dashboard
2. Go to: **Workers & Pages** → **Create application** → **Pages** → **Upload assets**
3. Drag and drop `index.html`
4. Click **Deploy site**

## DNS Configuration

Once deployed, Cloudflare Pages will provide a URL like `pablo-marketing.pages.dev`.

### Automatic DNS (Recommended)
When you add a custom domain in Cloudflare Pages, it automatically creates the DNS records:
- `gopablo.net` → CNAME to `pablo-marketing.pages.dev`
- `www.gopablo.net` → CNAME to `pablo-marketing.pages.dev`

### Manual DNS (if needed)
If you need to configure manually:

1. Go to: **DNS** → **Records** in Cloudflare Dashboard
2. Add records:
   - **Type**: CNAME
   - **Name**: `@` (for gopablo.net)
   - **Target**: `pablo-marketing.pages.dev`
   - **Proxy status**: Proxied (orange cloud)

   - **Type**: CNAME
   - **Name**: `www`
   - **Target**: `pablo-marketing.pages.dev`
   - **Proxy status**: Proxied (orange cloud)

## Updating the Marketing Site

### If using Git deployment:
```bash
# Make changes to index.html
git add .
git commit -m "Update marketing copy"
git push
# Cloudflare automatically redeploys
```

### If using direct upload:
1. Edit `index.html`
2. Go to Cloudflare Dashboard → **Pablo Marketing** project
3. Click **Create new deployment**
4. Upload the updated `index.html`

## Testing

After deployment:
1. Visit `https://gopablo.net` - should show marketing page
2. Visit `https://www.gopablo.net` - should show marketing page
3. Visit `https://app.gopablo.net` - should show application login
4. Check SSL certificate is active (green padlock in browser)
5. Test on mobile devices

## Costs

- **Cloudflare Pages**: FREE
  - Unlimited requests
  - Unlimited bandwidth
  - Global CDN
  - Automatic SSL
  - 500 builds/month (free tier)

## Rollback

If you need to rollback:
1. Go to Cloudflare Dashboard → **Pablo Marketing** → **Deployments**
2. Find the previous deployment
3. Click **...** → **Rollback to this deployment**

## Performance

Expected performance:
- **Load time**: < 1 second globally
- **Lighthouse score**: 95-100
- **CDN**: Cloudflare's global network (275+ cities)

## SEO Considerations

The static HTML includes:
- ✅ Semantic HTML structure
- ✅ Meta description
- ✅ Mobile-responsive design
- ✅ Fast load times

Consider adding:
- [ ] Open Graph tags for social media sharing
- [ ] Favicon
- [ ] Google Analytics / PostHog tracking
- [ ] robots.txt
- [ ] sitemap.xml

## Next Steps

After deploying the marketing site, you'll need to:

1. **Update the application** to redirect root to login (see main project docs)
2. **Verify DNS propagation**: `dig gopablo.net` and `dig www.gopablo.net`
3. **Test email links**: Ensure signup/login links work correctly
4. **Update any marketing materials** with new URLs

## Troubleshooting

**Issue: DNS not resolving**
- Wait 5-10 minutes for DNS propagation
- Check: `dig gopablo.net CNAME`
- Verify custom domain is configured in Cloudflare Pages

**Issue: SSL certificate not provisioning**
- Ensure Cloudflare proxy is enabled (orange cloud)
- Wait 15 minutes for certificate generation
- Check Cloudflare SSL/TLS settings are set to "Full"

**Issue: Page shows 404**
- Verify file is named exactly `index.html` (lowercase)
- Check build output directory is correct
- Review deployment logs in Cloudflare Dashboard

**Issue: Links to app.gopablo.net broken**
- Verify all links use full URLs: `https://app.gopablo.net/signup`
- Test in incognito mode to avoid cache issues

## Support

For issues with:
- **Cloudflare Pages**: [Cloudflare Support](https://support.cloudflare.com/)
- **DNS Configuration**: [Cloudflare DNS Docs](https://developers.cloudflare.com/dns/)
- **Custom Domains**: [Pages Custom Domains](https://developers.cloudflare.com/pages/configuration/custom-domains/)
