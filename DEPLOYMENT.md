# NXT Sentinel Marketing Website Deployment

## Quick Start

### Prerequisites
- Firebase CLI installed (`npm install -g firebase-tools`)
- Firebase project created and linked to GCP billing
- GitHub repository for nxtsentinelweb

---

## Step 1: Update Firebase Project ID

Edit `.firebaserc` and replace `YOUR_FIREBASE_PROJECT_ID` with your actual project ID:

```json
{
  "projects": {
    "default": "nxtsentinel-prod"
  }
}
```

To find your project ID:
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click on your project
3. Project ID is shown in Project Settings

---

## Step 2: Manual Deployment (First Time)

```bash
# Login to Firebase
firebase login

# Deploy to Firebase Hosting
firebase deploy --only hosting
```

You should see output like:
```
✔ Deploy complete!

Hosting URL: https://YOUR_PROJECT_ID.web.app
```

---

## Step 3: Set Up GitHub Actions (Automated Deployment)

### 3.1 Generate Firebase Service Account

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select your project
3. Click ⚙️ (gear icon) > **Project Settings**
4. Go to **Service Accounts** tab
5. Click **Generate new private key**
6. Save the downloaded JSON file

### 3.2 Add GitHub Secrets

1. Go to your GitHub repository
2. Click **Settings** > **Secrets and variables** > **Actions**
3. Click **New repository secret**
4. Add the following secrets:

| Secret Name | Value |
|-------------|-------|
| `FIREBASE_SERVICE_ACCOUNT` | Entire contents of the downloaded JSON file |
| `FIREBASE_PROJECT_ID` | Your Firebase project ID (e.g., `nxtsentinel-prod`) |

### 3.3 Test the Workflow

1. Push a commit to the `main` branch
2. Go to **Actions** tab in GitHub
3. Watch the deployment workflow run
4. Once complete, your site is live!

---

## Step 4: Connect Custom Domain

### 4.1 Add Domain in Firebase

1. Go to [Firebase Console](https://console.firebase.google.com)
2. Select your project
3. Go to **Hosting** in the left sidebar
4. Click **Add custom domain**
5. Enter `nxtsentinel.com`
6. Follow the verification steps

### 4.2 Update DNS Records

Add these records in your domain registrar (or Cloud DNS):

| Type | Name | Value |
|------|------|-------|
| A | @ | `151.101.1.195` |
| A | @ | `151.101.65.195` |
| CNAME | www | `nxtsentinel.com` |

**Note**: Firebase will provide the exact IP addresses during setup. The IPs above are examples.

### 4.3 Wait for SSL

- SSL certificate is automatically provisioned
- Takes 10-60 minutes after DNS propagation
- Check status in Firebase Console > Hosting

---

## Deployment Commands

### Deploy to Production
```bash
firebase deploy --only hosting
```

### Deploy to Preview Channel
```bash
firebase hosting:channel:deploy preview
```

### List All Channels
```bash
firebase hosting:channel:list
```

### Delete Preview Channel
```bash
firebase hosting:channel:delete preview
```

---

## Rollback

### Via Firebase Console
1. Go to Firebase Console > Hosting
2. Click on your site
3. In **Release history**, find the previous deployment
4. Click the three dots menu > **Rollback**

### Via CLI
```bash
# List recent deployments
firebase hosting:releases:list

# Rollback to specific version
firebase hosting:rollback --version VERSION_ID
```

---

## Monitoring

### Firebase Console
- View traffic and bandwidth usage
- Check deployment history
- Monitor performance metrics

### URL to Monitor
- Production: `https://nxtsentinel.com`
- Firebase URL: `https://YOUR_PROJECT_ID.web.app`

---

## Troubleshooting

### Deployment Failed
```bash
# Check Firebase login status
firebase login:list

# Re-authenticate
firebase login --reauth

# Verify project
firebase projects:list
```

### DNS Not Working
1. Verify DNS records are correct
2. Wait for propagation (up to 48 hours)
3. Check with: `dig nxtsentinel.com`

### SSL Certificate Pending
- Ensure DNS records are correct
- Wait up to 24 hours
- Check Firebase Console for status

---

## File Structure

```
nxtsentinelweb/
├── .firebaserc           # Firebase project config
├── .github/
│   └── workflows/
│       └── deploy.yml    # GitHub Actions workflow
├── firebase.json         # Firebase hosting config
├── DEPLOYMENT.md         # This file
├── index.html            # Homepage
├── pricing.html          # Pricing page with waitlist
├── how-it-works.html     # Technical overview
├── faq.html              # FAQs
├── privacy.html          # Privacy policy
└── terms.html            # Terms of service
```

---

## Related Documentation

- [Firebase Hosting Docs](https://firebase.google.com/docs/hosting)
- [GitHub Actions for Firebase](https://github.com/FirebaseExtended/action-hosting-deploy)
- [Custom Domain Setup](https://firebase.google.com/docs/hosting/custom-domain)

