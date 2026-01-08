# Firebase Setup Guide for ClinexNotes

> **Complete step-by-step guide to set up Firebase for cloud sync and Google authentication**

---

## 📋 Prerequisites

- Google account
- ClinexNotes application cloned/downloaded
- Code editor (VS Code recommended)
- 10-15 minutes of time

---

## 🎯 What You'll Set Up

1. ✅ Firebase project
2. ✅ Google authentication
3. ✅ Firestore database
4. ✅ Security rules
5. ✅ Application configuration

---

## 🚀 Step-by-Step Setup

### **Step 1: Create Firebase Project**

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"**
3. Enter project name:
   ```
   Project Name: ClinexNotes
   ```
4. Click **"Continue"**
5. Disable Google Analytics (optional, click "Create project")
6. Wait for project to be created (2-3 minutes)
7. Click **"Continue"** when done

✅ **Result:** Firebase project created and ready

---

### **Step 2: Enable Google Authentication**

1. In Firebase Console, go to **Build** (left sidebar)
2. Click **Authentication**
3. Click **"Get started"**
4. Find **"Google"** in the Sign-in method list
5. Click on **Google**
6. Toggle **"Enable"** to ON
7. In the dropdown, select your email as project support email
8. Click **"Save"**

✅ **Result:** Google sign-in enabled

---

### **Step 3: Add Authorized Domains**

1. Still in **Authentication**, go to **Settings** tab
2. Scroll down to **"Authorized domains"**
3. Click **"Add domain"**
4. Add these domains:
   ```
   localhost
   127.0.0.1
   ```
5. Click **"Add"** for each

6. Later, add your deployed domain:
   ```
   your-app.vercel.app
   ```

✅ **Result:** Domains authorized for sign-in

---

### **Step 4: Create Firestore Database**

1. In Firebase Console, go to **Build** > **Firestore Database**
2. Click **"Create database"**
3. Choose location closest to you
4. Click **"Next"**
5. Choose **"Start in production mode"**
6. Click **"Enable"**

⏳ **Wait:** Database initialization (1-2 minutes)

✅ **Result:** Firestore database ready

---

### **Step 5: Set Up Security Rules**

1. In Firestore, go to **Rules** tab
2. Clear the default rules
3. Paste these rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only read/write their own notes
    match /users/{userId}/notes/{noteId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

4. Click **"Publish"**

✅ **Result:** Security rules protecting user data

---

### **Step 6: Get Your Firebase Configuration**

1. In Firebase Console, click the **⚙️ (gear icon)** at top-left
2. Click **"Project settings"**
3. Go to **"Your apps"** section
4. If no app exists, click **"Web"** icon (</>)
5. Register your app with name: `ClinexNotes`
6. Copy the Firebase config object:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyAWK6ciaHflSryBN-H7QvXcvDjOMuYybmM",
    authDomain: "clinexnotes.firebaseapp.com",
    projectId: "clinexnotes",
    storageBucket: "clinexnotes.firebasestorage.app",
    messagingSenderId: "796124369006",
    appId: "1:796124369006:web:93ca4b39caf75928d0167f"
};
```

✅ **Result:** Configuration ready to use

---

### **Step 7: Update Application Configuration**

1. Open your project folder
2. Navigate to `src/firebase-config.js`
3. Replace the config object with your values:

```javascript
// Firebase Configuration
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",          // Replace this
    authDomain: "YOUR_AUTH_DOMAIN",  // Replace this
    projectId: "YOUR_PROJECT_ID",    // Replace this
    storageBucket: "YOUR_STORAGE",   // Replace this
    messagingSenderId: "YOUR_SENDER_ID",  // Replace this
    appId: "YOUR_APP_ID"             // Replace this
};
```

4. Save the file

✅ **Result:** Application connected to Firebase

---

### **Step 8: Test Locally**

1. Open terminal in project folder
2. Run:
   ```bash
   npm install
   npm run dev
   ```

3. Open `http://localhost:5173` in browser
4. Test sign-in:
   - Click "Sign in with Google"
   - Sign in with your Google account
   - Verify your name and photo appear
5. Create a test note
6. Verify it syncs to Firestore

✅ **Result:** Everything working locally!

---

### **Step 9: Deploy to Vercel**

1. Push code to GitHub:
   ```bash
   git add .
   git commit -m "Setup Firebase integration"
   git push origin main
   ```

2. Go to [Vercel](https://vercel.com)
3. Connect your GitHub repository
4. Click **"Deploy"**
5. Wait for deployment (2-3 minutes)

✅ **Result:** App deployed!

---

### **Step 10: Add Deployed Domain to Firebase**

1. After deployment, get your Vercel URL
2. Go to Firebase Console
3. **Authentication** > **Settings**
4. Add your domain:
   ```
   your-app.vercel.app
   ```

5. Click **"Add domain"**

✅ **Result:** Sign-in works on production!

---

## 🔍 Verification Checklist

After setup, verify everything works:

### **Local Testing**
- [ ] `npm run dev` works
- [ ] App loads without errors
- [ ] Notes can be created
- [ ] Sign in with Google works
- [ ] User photo appears in header
- [ ] Notes sync after sign-in
- [ ] Search functions correctly

### **Firebase Console**
- [ ] Project created
- [ ] Google authentication enabled
- [ ] Firestore database created
- [ ] Security rules published
- [ ] Configuration obtained

### **Production Testing**
- [ ] App deployed to Vercel
- [ ] Domain added to Firebase
- [ ] Sign in works on deployed site
- [ ] Notes sync across devices
- [ ] Mobile install works (PWA)

---

## 🆘 Troubleshooting

### **"Sign in failed" Error**

**Problem:** Google sign-in not working

**Solutions:**
1. Check domain is in Firebase authorized domains
2. Verify Google authentication is enabled
3. Clear browser cache and try again
4. Check console for specific error message

### **"Database not found" Error**

**Problem:** Firestore database not created

**Solutions:**
1. Go to Firestore in Firebase Console
2. Click "Create database"
3. Choose production mode
4. Complete the setup

### **"Permission denied" Error**

**Problem:** Security rules not allowing writes

**Solutions:**
1. Go to Firestore > Rules
2. Paste the rules from Step 5 above
3. Click "Publish"
4. Wait a few seconds for rules to apply

### **Notes not syncing**

**Problem:** Notes created but not appearing in Firestore

**Solutions:**
1. Verify you're signed in (check for user photo)
2. Check browser console for errors
3. Verify security rules are published
4. Try creating a new note
5. Check Firestore data tab directly

---

## 📊 Firebase Free Tier Limits

**Safe to use for testing:**
- ✅ 50,000 reads/month
- ✅ 20,000 writes/month
- ✅ 20,000 deletes/month
- ✅ 1 GB storage
- ✅ Unlimited auth

**Estimated usage (100 users):**
- 30 reads/day = 900/month
- 10 writes/day = 300/month
- **Well within free tier!**

---

## 🔐 Security Best Practices

1. **Never share your credentials**
   - Keep `firebaseConfig` private
   - Don't commit to public repos

2. **Use strong security rules**
   - Only allow authorized reads/writes
   - Validate user authentication

3. **Monitor usage**
   - Check Firebase console monthly
   - Watch for unusual activity

4. **Manage users**
   - Delete test accounts
   - Monitor authentication logs

---

## 📚 Useful Firebase Console Links

| Section | Purpose |
|---------|---------|
| **Overview** | Project stats and status |
| **Authentication** | User management |
| **Firestore** | Database management |
| **Storage** | File storage (if needed) |
| **Rules** | Security configuration |
| **Settings** | Project configuration |

---

## 🚀 Next Steps

1. **Test locally** - Verify everything works
2. **Deploy to Vercel** - Share with others
3. **Add domain** - Enable production sign-in
4. **Invite users** - Share the app link
5. **Monitor usage** - Keep an eye on Firebase dashboard

---

## ❓ FAQ

**Q: Do I need Firebase to use ClinexNotes?**
> A: No! The app works perfectly without Firebase. You just won't have cloud sync.

**Q: How do I upgrade from free tier?**
> A: Go to Firebase Console > Upgrade > Select Blaze plan (pay-as-you-go)

**Q: Can I change my project ID later?**
> A: No, but you can create a new project and migrate data.

**Q: Is my data safe?**
> A: Yes! Firebase provides enterprise-grade security and encryption.

**Q: How do I delete my Firebase project?**
> A: Settings > Delete project (requires waiting period)

---

## 📞 Support

**Having issues?** Check:
1. [TROUBLESHOOTING.md](TROUBLESHOOTING.md) - Common issues
2. [Firebase Docs](https://firebase.google.com/docs) - Official documentation
3. Browser console - Look for error messages

---

**Firebase Setup Complete! 🎉**

Your ClinexNotes app is now ready for cloud sync and Google authentication!

Happy note-taking! 📝
