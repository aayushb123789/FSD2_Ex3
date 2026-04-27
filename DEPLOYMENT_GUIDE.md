# Deployment & Configuration Guide

## 🚀 Production Build

### Build Optimization
```bash
npm run build
```

This creates an optimized production build in the `build/` directory.

### Build Output
```
build/
├── static/
│   ├── css/
│   │   └── main.xxxxx.css      # Minified CSS
│   ├── js/
│   │   ├── main.xxxxx.js       # Minified JS
│   │   └── runtime.xxxxx.js    # React runtime
│   └── media/
│       └── images/              # Optimized images
├── index.html                   # HTML entry point
└── favicon.ico                  # Favicon
```

---

## 🌐 Environment Setup

### Create .env File (Production)
```bash
# .env.production
REACT_APP_API_URL=https://api.example.com
REACT_APP_ENV=production
```

### Create .env File (Development)
```bash
# .env.development
REACT_APP_API_URL=http://localhost:3001
REACT_APP_ENV=development
```

### Usage in Code
```jsx
const API_URL = process.env.REACT_APP_API_URL;
```

---

## 📦 Deployment Options

### 1. Deploy to Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

**vercel.json Configuration:**
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "build",
  "env": {
    "REACT_APP_API_URL": "@api_url"
  }
}
```

### 2. Deploy to Netlify

```bash
# Install Netlify CLI
npm i -g netlify-cli

# Deploy
netlify deploy
```

**netlify.toml Configuration:**
```toml
[build]
command = "npm run build"
publish = "build"

[context.production.environment]
REACT_APP_API_URL = "https://api.example.com"
```

### 3. Deploy to GitHub Pages

```bash
# Update package.json
"homepage": "https://username.github.io/repo-name",
"deploy": "npm run build && gh-pages -d build"

# Install gh-pages
npm install --save-dev gh-pages

# Deploy
npm run deploy
```

### 4. Deploy to Docker

**Dockerfile:**
```dockerfile
# Build stage
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
RUN npm install -g serve
COPY --from=build /app/build ./build
EXPOSE 3000
CMD ["serve", "-s", "build", "-l", "3000"]
```

**Build & Run:**
```bash
docker build -t responsive-ui .
docker run -p 3000:3000 responsive-ui
```

### 5. Deploy to AWS S3 + CloudFront

```bash
# Build project
npm run build

# Install AWS CLI
pip install awscli

# Upload to S3
aws s3 sync build/ s3://my-bucket/

# Invalidate CloudFront
aws cloudfront create-invalidation --distribution-id XXXXX --paths "/*"
```

---

## 🔒 Security Configuration

### Content Security Policy Header
```html
<!-- public/index.html -->
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline' fonts.googleapis.com; font-src fonts.gstatic.com"
/>
```

### CORS Configuration
```jsx
// If using API proxy
fetch('/api/data', {
  method: 'GET',
  credentials: 'include',
  headers: {
    'Content-Type': 'application/json',
  }
})
```

### Environment Variables Best Practices
```
❌ DO NOT commit .env files
✅ DO gitignore .env files
✅ DO use .env.example as template
✅ DO set vars in production dashboard
```

---

## 🔄 CI/CD Pipeline

### GitHub Actions Example

**.github/workflows/deploy.yml:**
```yaml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      
      - name: Install dependencies
        run: npm install
      
      - name: Run tests
        run: npm test
      
      - name: Build
        run: npm run build
      
      - name: Deploy to Netlify
        uses: nwtgck/actions-netlify@v1
        with:
          publish-dir: './build'
          production-branch: main
          github-token: ${{ secrets.GITHUB_TOKEN }}
          netlify-auth-token: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          netlify-site-id: ${{ secrets.NETLIFY_SITE_ID }}
```

---

## 📊 Performance Monitoring

### Lighthouse Audit
```bash
# Install lighthouse CLI
npm install -g lighthouse

# Run audit
lighthouse https://your-domain.com --view
```

### Key Metrics to Monitor
- **LCP** (Largest Contentful Paint): < 2.5s
- **FID** (First Input Delay): < 100ms
- **CLS** (Cumulative Layout Shift): < 0.1
- **FCP** (First Contentful Paint): < 1.8s

### React DevTools Profiler
1. Install React DevTools extension
2. Open DevTools → Profiler tab
3. Record component renders
4. Identify performance bottlenecks

---

## 🛠️ Configuration Files

### package.json Scripts
```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "analyze": "source-map-explorer 'build/static/js/*.js'"
  }
}
```

### .eslintrc.json (Optional)
```json
{
  "extends": ["react-app"],
  "rules": {
    "no-unused-vars": "warn",
    "react-hooks/rules-of-hooks": "error"
  }
}
```

### .prettierrc (Optional)
```json
{
  "semi": true,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

---

## 📱 PWA Configuration (Optional)

### Create Web App Manifest
```json
{
  "short_name": "Responsive UI",
  "name": "Responsive Material UI Design",
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/icon-512.png",
      "sizes": "512x512",
      "type": "image/png",
      "purpose": "any maskable"
    }
  ],
  "start_url": "/",
  "background_color": "#ffffff",
  "display": "standalone",
  "scope": "/",
  "theme_color": "#667eea"
}
```

### Add to HTML
```html
<link rel="manifest" href="/manifest.json" />
<meta name="theme-color" content="#667eea" />
```

---

## 🐛 Debugging in Production

### Source Maps
```bash
# Keep source maps for debugging
# build/static/js/main.xxxxx.js.map
```

### Error Tracking with Sentry
```bash
npm install @sentry/react @sentry/tracing
```

```jsx
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "https://key@sentry.io/project",
  environment: process.env.NODE_ENV,
  tracesSampleRate: 1.0,
});

export default Sentry.withProfiler(App);
```

---

## 📈 Monitoring Checklist

- [ ] Set up error tracking (Sentry)
- [ ] Configure analytics (Google Analytics)
- [ ] Monitor performance metrics
- [ ] Set up uptime monitoring
- [ ] Configure log aggregation
- [ ] Set up alerting
- [ ] Monitor bundle size
- [ ] Track user interactions

---

## 🔄 Update Strategy

### Update Dependencies
```bash
# Check for updates
npm outdated

# Update all packages
npm update

# Update specific package
npm install package@latest
```

### Semantic Versioning
```
major.minor.patch
1.2.3
↑ Breaking changes
  ↑ New features
    ↑ Bug fixes
```

---

## 🗂️ Directory Structure in Production

```
/
├── index.html              # Entry point
├── favicon.ico
├── manifest.json
├── static/
│   ├── css/
│   │   └── main.[hash].css
│   ├── js/
│   │   ├── main.[hash].js
│   │   ├── runtime.[hash].js
│   │   └── [chunk].[hash].js
│   └── media/
│       └── [images]
└── robots.txt
```

---

## 📋 Pre-Deployment Checklist

### Code Quality
- [ ] Run eslint: `npm run lint`
- [ ] Run tests: `npm test`
- [ ] Check TypeScript: `npm run type-check`
- [ ] No console.log() in production code
- [ ] No hardcoded URLs
- [ ] Error handling in place

### Performance
- [ ] Lighthouse score > 90
- [ ] Bundle size analyzed
- [ ] Images optimized
- [ ] Lazy loading implemented
- [ ] Code splitting enabled

### Security
- [ ] No secrets in code
- [ ] HTTPS enabled
- [ ] CSP headers configured
- [ ] API authentication secure
- [ ] Input validation implemented

### SEO
- [ ] Meta tags configured
- [ ] sitemap.xml created
- [ ] robots.txt configured
- [ ] Open Graph tags added
- [ ] Canonical URLs set

### Documentation
- [ ] README updated
- [ ] API documentation done
- [ ] Deployment guide written
- [ ] Environment vars documented
- [ ] Troubleshooting guide created

---

## 🚨 Rollback Plan

### If Deployment Fails
```bash
# Check logs
vercel logs

# Rollback to previous version
vercel rollback

# Or redeploy previous build
git revert HEAD
npm run build
# Redeploy
```

---

## 📞 Troubleshooting

### Issue: Blank Page After Deploy
**Solution:** 
- Check browser console for errors
- Verify environment variables
- Check homepage in package.json

### Issue: Assets 404
**Solution:**
- Verify build output
- Check asset paths
- Verify static file serving

### Issue: Slow Load Times
**Solution:**
- Analyze bundle size
- Check network throttling
- Optimize images
- Enable caching

---

## 📚 Resources

- [Create React App Deployment](https://create-react-app.dev/deployment/)
- [Vercel Documentation](https://vercel.com/docs)
- [Netlify Documentation](https://docs.netlify.com/)
- [Docker Documentation](https://docs.docker.com/)
- [AWS S3 Hosting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)

---

**Deployment Ready!** 🎉

Your responsive Material UI application is ready for production deployment. Choose your preferred platform from the options above and follow the setup instructions.
