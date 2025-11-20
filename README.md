# Liv24 UAT APK Hosting

Firebase Hosting project for serving APK download page.

## ⚠️ Important: Firebase Spark Plan Limitation

Firebase Hosting on the **Spark (free) plan** does not allow executable files like APK files.

**Solution:** Host the APK file on **Firebase Storage** instead.

## Setup Instructions

### 1. Upload APK to Firebase Storage

```bash
# Option 1: Using Firebase CLI
firebase storage:upload public/app-uat-latest.apk apk/app-uat-latest.apk

# Option 2: Using gsutil
gsutil cp public/app-uat-latest.apk gs://osp-uat-apk.appspot.com/apk/app-uat-latest.apk

# Option 3: Using Firebase Console
# Go to: https://console.firebase.google.com/project/osp-uat-apk/storage
# Create folder "apk" and upload app-uat-latest.apk
```

### 2. Get the Download URL

After uploading, get the download URL:

**Option A: From Firebase Console**

1. Go to [Firebase Storage Console](https://console.firebase.google.com/project/osp-uat-apk/storage)
2. Click on the APK file
3. Copy the download URL

**Option B: Using gsutil**

```bash
gsutil signurl -d 10y gs://osp-uat-apk.appspot.com/apk/app-uat-latest.apk
```

**Option C: Make file publicly accessible**

1. In Firebase Console, go to Storage
2. Click on the APK file
3. Click "Get Link" or make it public
4. The URL format will be:
   ```
   https://firebasestorage.googleapis.com/v0/b/osp-uat-apk.appspot.com/o/apk%2Fapp-uat-latest.apk?alt=media
   ```

### 3. Update the Download URL in HTML

Edit `public/index.html` and update the `APK_DOWNLOAD_URL` constant in the script section:

```javascript
const APK_DOWNLOAD_URL = "YOUR_STORAGE_URL_HERE";
```

Or set it as a global variable before the page loads (for easier updates).

### 4. Deploy to Firebase Hosting

```bash
firebase deploy --only hosting
```

The HTML page will be deployed, and the APK download will work from Firebase Storage.

## File Structure

```
/apk-osp/
├── firebase.json          # Firebase config (APK excluded from hosting)
├── public/
│   ├── index.html        # Download page
│   └── app-uat-latest.apk # APK file (not deployed, upload to Storage instead)
└── upload-apk-to-storage.sh # Helper script
```

## Alternative Solutions

If you need to host APK files directly:

1. **Upgrade to Blaze Plan** (pay-as-you-go, allows executable files)
2. **Use GitHub Releases** (free, good for public APKs)
3. **Use Google Drive** (free, shareable links)
4. **Use Cloudflare Pages** (free, allows binary files)

## Testing Locally

```bash
# Use the Node.js server
node server.js

# Then visit: http://localhost:5500
```

Make sure to update the `APK_DOWNLOAD_URL` in `index.html` to point to your Storage URL even for local testing.
