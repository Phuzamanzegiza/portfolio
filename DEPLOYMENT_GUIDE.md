# Portfolio Development & Deployment Guide

## Dr Francis Ngema - Medical Data Scientist Portfolio

Complete guide for deploying your portfolio to Netlify with GoDaddy custom domains.

---

## 📁 Project Files

```
portfolio/
├── index.html          # Main portfolio website
├── netlify.toml        # Netlify deployment configuration
└── DEPLOYMENT_GUIDE.md # This guide
```

---

## 🚀 Quick Start with Claude Code

### Step 1: Open Terminal and Navigate to Your Folder

```bash
cd /path/to/your/portfolio/folder
```

### Step 2: Start Claude Code

```bash
claude
```

### Step 3: Paste This Prompt

```
I have a portfolio website ready for deployment. Please help me:

1. Initialize git and create initial commit
2. Create a new GitHub repository called "portfolio"
3. Push all files to GitHub
4. Provide the repo URL so I can connect it to Netlify

Files in this folder: index.html, netlify.toml, DEPLOYMENT_GUIDE.md
```

---

## 🌐 Netlify Setup

### Step 1: Create Netlify Account
1. Go to [netlify.com](https://netlify.com)
2. Sign up with your GitHub account (easiest)

### Step 2: Import Your Repository
1. Click **"Add new site"** → **"Import an existing project"**
2. Choose **GitHub**
3. Select your **portfolio** repository
4. Configure build settings:
   - **Build command:** Leave empty
   - **Publish directory:** `.`
5. Click **"Deploy site"**

### Step 3: Note Your Netlify URL
- Netlify assigns a URL like `random-name-123.netlify.app`
- **Write this down** - you'll need it for DNS setup
- Test this URL to ensure your site works

---

## 🔗 GoDaddy + Netlify Domain Setup

### Your Domains
- **francisngema.com** (Primary)
- **francisngema.co.za** (Redirects to .com)

---

### PART A: Add Domains to Netlify

1. In Netlify dashboard, go to **Site settings** → **Domain management**

2. Click **"Add custom domain"** and add these one by one:
   - `francisngema.com` (set as primary)
   - `www.francisngema.com`
   - `francisngema.co.za`
   - `www.francisngema.co.za`

3. Netlify will show **"Awaiting external DNS"** - this is expected

---

### PART B: Configure GoDaddy DNS for francisngema.com

#### Step 1: Access DNS Settings
1. Log into [godaddy.com](https://godaddy.com)
2. Go to **My Products** → find **francisngema.com**
3. Click **"DNS"** or **"Manage DNS"**

#### Step 2: Delete Default Records
Remove these if they exist (GoDaddy parking records):
- Any **A record** with @ pointing to GoDaddy IP
- Any **CNAME** for www pointing to GoDaddy

**⚠️ Keep the NS (nameserver) records - don't delete those!**

#### Step 3: Add Netlify Records

Click **"Add"** and create these records:

**Record 1 - A Record (apex domain)**
| Field | Value |
|-------|-------|
| Type | A |
| Name | @ |
| Value | 75.2.60.5 |
| TTL | 600 seconds |

**Record 2 - CNAME Record (www)**
| Field | Value |
|-------|-------|
| Type | CNAME |
| Name | www |
| Value | `your-site-name.netlify.app` |
| TTL | 600 seconds |

**⚠️ Replace `your-site-name.netlify.app` with your actual Netlify subdomain!**

#### Step 4: Save
Click **"Save"** after adding each record.

---

### PART C: Configure GoDaddy DNS for francisngema.co.za

#### Step 1: Access DNS Settings
1. In GoDaddy, go to **My Products**
2. Find **francisngema.co.za** → click **"DNS"**

#### Step 2: Add Same Records

**Record 1 - A Record**
| Field | Value |
|-------|-------|
| Type | A |
| Name | @ |
| Value | 75.2.60.5 |
| TTL | 600 seconds |

**Record 2 - CNAME Record**
| Field | Value |
|-------|-------|
| Type | CNAME |
| Name | www |
| Value | `your-site-name.netlify.app` |
| TTL | 600 seconds |

---

### PART D: Wait for DNS Propagation

DNS changes take time to spread globally:
- **Typical:** 10-30 minutes
- **Maximum:** 24-48 hours

**Check propagation status:**
- https://dnschecker.org/#A/francisngema.com
- https://whatsmydns.net/#A/francisngema.com

Wait until you see `75.2.60.5` showing globally.

---

### PART E: Enable HTTPS in Netlify

Once DNS has propagated:

1. Go to Netlify → **Site settings** → **Domain management** → **HTTPS**
2. Click **"Verify DNS configuration"**
3. If verified, click **"Provision certificate"**
4. Wait 1-2 minutes for certificate
5. Enable **"Force HTTPS"**

**Note:** Netlify provides free SSL via Let's Encrypt, auto-renewed.

---

### PART F: Set Primary Domain & Redirects

1. In Netlify → **Domain management**
2. Click the **three dots** next to `francisngema.com`
3. Select **"Set as primary domain"**

This means:
- `francisngema.com` = main site
- All other domains redirect to it automatically

---

## 🔒 GoDaddy Security (Free Steps)

Since you're skipping the paid protection:

### 1. Enable Two-Factor Authentication
1. GoDaddy → **Account Settings** → **Login & PIN**
2. Enable **2-Step Verification**
3. Use authenticator app (Google Authenticator or similar)

### 2. Lock Your Domains
1. **My Products** → click on domain
2. Find **"Domain Lock"** → ensure it's **ON**
3. Do this for both domains

### 3. Use Strong Password
- Unique password for GoDaddy
- At least 16 characters
- Use a password manager

---

## ✅ Final Verification Checklist

Test all these URLs after setup:

| URL | Expected Result |
|-----|-----------------|
| https://francisngema.com | ✅ Site loads with padlock |
| https://www.francisngema.com | ✅ Redirects to francisngema.com |
| http://francisngema.com | ✅ Redirects to HTTPS |
| https://francisngema.co.za | ✅ Redirects to francisngema.com |
| https://www.francisngema.co.za | ✅ Redirects to francisngema.com |

---

## 🔄 Making Future Updates

### Quick Update Process

```bash
# Edit your files locally, then:
cd /path/to/portfolio
git add .
git commit -m "Updated projects section"
git push
```

Netlify auto-deploys in ~30 seconds.

### Using Claude Code for Updates

```bash
claude
```

Then say:
```
Please update my portfolio:
- Add a new project about GEMS Analytics Dashboard
- Update the contact email to myreal@email.com
- Commit and push
```

---

## 🛠 Troubleshooting

### "DNS not propagating after 48 hours"
1. Double-check A record value is exactly `75.2.60.5`
2. Ensure no conflicting records exist
3. Contact GoDaddy support

### "HTTPS certificate won't provision"
1. Verify DNS is propagated (use dnschecker.org)
2. Ensure all 4 domains are added in Netlify
3. Try clicking **"Renew certificate"**

### "Site shows old version after push"
1. Netlify dashboard → **Deploys**
2. Click **"Trigger deploy"** → **"Clear cache and deploy site"**

### "ERR_TOO_MANY_REDIRECTS"
1. Ensure you don't have conflicting redirect rules
2. Check that HTTPS is properly provisioned
3. Clear browser cache and cookies

---

## 📊 DNS Records Summary

### francisngema.com
```
Type    Name    Value                           TTL
A       @       75.2.60.5                       600
CNAME   www     your-site-name.netlify.app      600
```

### francisngema.co.za
```
Type    Name    Value                           TTL
A       @       75.2.60.5                       600
CNAME   www     your-site-name.netlify.app      600
```

---

## 📋 Quick Reference Links

| Service | URL |
|---------|-----|
| GoDaddy Login | https://sso.godaddy.com |
| GoDaddy DNS | https://dcc.godaddy.com |
| Netlify Dashboard | https://app.netlify.com |
| DNS Propagation Check | https://dnschecker.org |
| SSL Test | https://www.ssllabs.com/ssltest/ |

---

## 🎯 Post-Launch To-Do

- [ ] Update placeholder email in contact section
- [ ] Add real JMIR AI publication link
- [ ] Update LinkedIn/GitHub URLs to your actual profiles
- [ ] Add a professional headshot (optional)
- [ ] Share your new site on LinkedIn!
- [ ] Set calendar reminder for domain renewal (yearly)

---

## 📞 Support Contacts

| Issue | Contact |
|-------|---------|
| Domain/DNS | GoDaddy: 087 365 0000 (SA) |
| Hosting/Deploy | Netlify: support@netlify.com |
| SSL Issues | Usually self-resolves; wait 24hrs |

---

*Last updated: January 2026*
*Portfolio for Dr Francis Ngema - Medical Data Scientist*
