# 🚀 Ionic Project Integration Guide

## Complete Walkthrough: Integrating Automated APK Release Workflow

This guide will walk you through integrating the automated APK release workflow into any Ionic project. Follow these steps carefully to set up automated releases for your Ionic app.

---

## 📋 Prerequisites

Before starting, ensure you have:

- ✅ An Ionic project with Capacitor Android platform
- ✅ A GitHub repository for your project
- ✅ Basic knowledge of GitHub Actions
- ✅ Android keystore for signing (or willing to create one)

---

## 🛠️ Step 1: Project Structure Verification

Ensure your Ionic project has the following structure:

```
your-ionic-project/
├── package.json                 # Must exist
├── src/                        # Ionic source code
├── android/                    # Capacitor Android platform
│   ├── app/
│   │   └── build.gradle       # Must contain versionCode and versionName
│   └── gradlew                # Android wrapper script
├── capacitor.config.ts         # Capacitor configuration
└── .gitignore                 # Should exclude .env files
```

### Verify Android Platform Setup:

```bash
# If android platform doesn't exist, add it:
npx cap add android

# Sync your project:
npx cap sync android

# Open in Android Studio to verify setup:
npx cap open android
```

---

## 🔐 Step 2: Create Android Keystore

### Option A: Create New Keystore

```bash
# Navigate to a secure location (NOT in your project)
cd ~/Documents/keystores

# Generate keystore
keytool -genkey -v -keystore my-app-key.keystore -keyalg RSA -keysize 2048 -validity 10000 -alias my-app-key

# You'll be prompted for:
# - Keystore password (remember this!)
# - Key password (remember this!)
# - Your details (name, organization, etc.)
```

### Option B: Use Existing Keystore

If you already have a keystore, locate the file and note down:

- Keystore file path
- Keystore password
- Key alias name
- Key password

---

## 🔑 Step 3: Configure GitHub Secrets

Go to your GitHub repository and navigate to **Settings → Secrets and variables → Actions**.

### Add Required Secrets:

Click **"New repository secret"** for each:

1. **`RELEASE_KEYSTORE_BASE64`**

   ```bash
   # Convert keystore to base64
   base64 -i path/to/your/keystore.jks | tr -d '\n'
   # Copy the output and paste as secret value
   ```

2. **`RELEASE_STORE_PASSWORD`**

   - Value: Your keystore password

3. **`RELEASE_KEY_ALIAS`**

   - Value: Your key alias name

4. **`RELEASE_KEY_PASSWORD`**
   - Value: Your key password

### Optional Environment Variable Secrets:

If your app uses environment variables, add these secrets:

5. **`API_BASE_URL`** - Your API endpoint
6. **`API_KEY`** - Your API key
7. **`DATABASE_URL`** - Database connection string
8. **`FIREBASE_CONFIG`** - Firebase configuration
9. **`ANALYTICS_ID`** - Analytics tracking ID

_Add any other secrets your app requires._

---

## 📁 Step 4: Add Workflow File

### Create the workflow directory:

```bash
mkdir -p .github/workflows
```

### Copy the workflow file:

Copy the `release-apk.yml` file to `.github/workflows/release-apk.yml` in your project root.

Your structure should now look like:

```
your-ionic-project/
├── .github/
│   └── workflows/
│       └── release-apk.yml    # The workflow file
├── package.json
├── src/
└── android/
```

---

## ⚙️ Step 5: Configure Environment Variables (Optional)

If your Ionic app uses environment variables:

### Edit the workflow file:

Open `.github/workflows/release-apk.yml` and find the "Setup Environment Variables" step.

### Uncomment and modify the .env creation block:

```yaml
# Find this section and uncomment the lines you need:
cat > .env << EOF
# API Configuration
API_BASE_URL=${{ secrets.API_BASE_URL }}
API_KEY=${{ secrets.API_KEY }}

# Firebase Configuration (if using Firebase)
FIREBASE_CONFIG=${{ secrets.FIREBASE_CONFIG }}

# App Configuration
APP_ENV=production
DEBUG=false
EOF
```

### For Ionic/Angular projects, you might need:

```yaml
# For Angular environment files:
cat > src/environments/environment.prod.ts << EOF
export const environment = {
  production: true,
  apiUrl: '${{ secrets.API_BASE_URL }}',
  apiKey: '${{ secrets.API_KEY }}',
  firebaseConfig: JSON.parse('${{ secrets.FIREBASE_CONFIG }}')
};
EOF
```

---

## 📦 Step 6: Update Package.json Scripts

Ensure your `package.json` has the required build script:

```json
{
  "scripts": {
    "build": "ionic build --prod",
    "build:android": "ionic build --prod && npx cap sync android"
  }
}
```

---

## 🏗️ Step 7: Update Android Build Configuration

### Edit `android/app/build.gradle`:

Ensure it has proper version configuration:

```gradle
android {
    compileSdkVersion 34

    defaultConfig {
        applicationId "com.yourcompany.yourapp"
        minSdkVersion 21
        targetSdkVersion 34
        versionCode 1          // This will be auto-incremented
        versionName "1.0.0"    // This will be auto-updated
    }

    // Add signing configuration
    signingConfigs {
        release {
            if (project.hasProperty('RELEASE_STORE_FILE')) {
                storeFile file(RELEASE_STORE_FILE)
                storePassword RELEASE_STORE_PASSWORD
                keyAlias RELEASE_KEY_ALIAS
                keyPassword RELEASE_KEY_PASSWORD
            }
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```

---

## 🔄 Step 8: Test Local Build (Optional)

Before pushing, test the build locally:

```bash
# Install dependencies
npm install

# Build the project
npm run build

# Sync with Capacitor
npx cap sync android

# Try building APK locally (optional)
cd android
./gradlew assembleDebug
```

---

## 🚀 Step 9: Commit and Push

```bash
# Add all files
git add .

# Commit changes
git commit -m "Add automated APK release workflow"

# Push to main branch
git push origin main
```

---

## 🎯 Step 10: Trigger Your First Release

### Method 1: Using Release Label

1. Create a Pull Request with your changes
2. Add the label `release` to the PR
3. Merge the PR → Workflow will run automatically

### Method 2: Using Title Convention

1. Create a PR with title containing `[release]`
   - Example: `[release] Add new features and bug fixes`
2. Merge the PR → Workflow will run automatically

### Method 3: Add Label to Merged PR

1. Merge any PR to main
2. Add the `release` label to the merged PR
3. Workflow will trigger immediately

---

## 📊 Step 11: Monitor the Release Process

### Watch the workflow:

1. Go to your repository → **Actions** tab
2. Click on the running workflow
3. Monitor each step's progress

### The workflow will:

- ✅ Detect package manager (npm/yarn/pnpm)
- ✅ Install dependencies
- ✅ Set up environment variables
- ✅ Build your Ionic app
- ✅ Sync with Capacitor
- ✅ Build signed Android APK
- ✅ Create GitHub release with APK attached
- ✅ Auto-increment version numbers

---

## 🎉 Step 12: Download Your APK

Once the workflow completes successfully:

1. Go to your repository → **Releases**
2. Find the latest release (e.g., `v1.0.1`)
3. Download the attached APK file
4. Install on Android device for testing

---

## 🔧 Troubleshooting Common Issues

### Issue: "versionCode not found"

**Solution:** Ensure `android/app/build.gradle` contains:

```gradle
versionCode 1
versionName "1.0.0"
```

### Issue: "Keystore error"

**Solution:**

- Verify base64 encoding: `base64 -i keystore.jks | tr -d '\n'`
- Check secret names match exactly
- Ensure keystore passwords are correct

### Issue: "Build failed"

**Solution:**

- Check `package.json` has `build` script
- Verify all dependencies are in `package.json`
- Test local build first

### Issue: "Permission denied gradlew"

**Solution:** The workflow automatically handles this with `chmod +x gradlew`

### Issue: "Environment variables not working"

**Solution:**

- Verify secrets are added to GitHub repository
- Check secret names match in workflow file
- Ensure .env creation block is uncommented

---

## 🎨 Customization Options

### Change Version Bump Type:

Add to PR title:

- `[major]` - Major version bump (1.0.0 → 2.0.0)
- `[minor]` - Minor version bump (1.0.0 → 1.1.0)
- Default - Patch version bump (1.0.0 → 1.0.1)

### Custom APK Name:

The APK will be named: `{repository-name}-v{version}.apk`

### Multiple Build Variants:

Modify the gradle command in workflow:

```yaml
# For debug build:
./gradlew assembleDebug

# For specific flavor:
./gradlew assembleProdRelease
```

---

## 📚 Additional Resources

- [Ionic Capacitor Android Documentation](https://capacitorjs.com/docs/android)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Android App Signing](https://developer.android.com/studio/publish/app-signing)

---

## 🆘 Support

If you encounter issues:

1. Check the workflow logs in GitHub Actions
2. Verify all prerequisites are met
3. Test local build first
4. Review the troubleshooting section above

---

**🎉 Congratulations!** You now have automated APK releases for your Ionic project. Every time you merge a PR with the `release` label or `[release]` in the title, a new signed APK will be automatically built and released!
