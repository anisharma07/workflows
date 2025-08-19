# 🚀 APK Release Workflows - Universal Improvements

This document summarizes the improvements made to both APK release workflows to support dynamic repository names and universal package manager detection.

## 🔄 Changes Made

### 1. Dynamic Repository Name Detection

Both workflows now automatically extract the repository name from GitHub context:

```yaml
- name: 🏷️ Get Repository Name
  id: repo-info
  run: |
    REPO_NAME=$(echo "${{ github.repository }}" | cut -d'/' -f2)
    echo "name=$REPO_NAME" >> $GITHUB_OUTPUT
    echo "Repository name: $REPO_NAME"
```

**Benefits:**

- ✅ No more hardcoded `<Repo_Name>` placeholders
- ✅ Works with any repository without manual changes
- ✅ APK files automatically named correctly: `{repo-name}-v{version}.apk`

### 2. Universal Package Manager Support

Automatic detection and support for all major JavaScript package managers:

#### Package Manager Detection Logic:

```yaml
- name: 📦 Detect Package Manager
  id: package-manager
  run: |
    if [ -f "pnpm-lock.yaml" ]; then
      echo "manager=pnpm" >> $GITHUB_OUTPUT
      echo "install_cmd=pnpm install --frozen-lockfile" >> $GITHUB_OUTPUT
    elif [ -f "yarn.lock" ]; then
      echo "manager=yarn" >> $GITHUB_OUTPUT
      echo "install_cmd=yarn install --frozen-lockfile" >> $GITHUB_OUTPUT
    elif [ -f "package-lock.json" ]; then
      echo "manager=npm" >> $GITHUB_OUTPUT
      echo "install_cmd=npm ci" >> $GITHUB_OUTPUT
    else
      echo "manager=npm" >> $GITHUB_OUTPUT
      echo "install_cmd=npm install" >> $GITHUB_OUTPUT
    fi
```

#### Supported Package Managers:

| Package Manager    | Lock File           | Install Command                  | Version Command                        |
| ------------------ | ------------------- | -------------------------------- | -------------------------------------- |
| **PNPM**           | `pnpm-lock.yaml`    | `pnpm install --frozen-lockfile` | `pnpm version {version}`               |
| **Yarn**           | `yarn.lock`         | `yarn install --frozen-lockfile` | `yarn version --new-version {version}` |
| **NPM**            | `package-lock.json` | `npm ci`                         | `npm version {version}`                |
| **NPM (fallback)** | None                | `npm install`                    | `npm version {version}`                |

### 3. Dynamic Caching Configuration

Cache paths and keys now adapt to the detected package manager:

```yaml
- name: 💾 Cache Dependencies
  uses: actions/cache@v4
  with:
    path: |
      node_modules
      ${{ steps.package-manager.outputs.cache_path }}
    key: ${{ runner.os }}-${{ steps.package-manager.outputs.cache_key }}
    restore-keys: |
      ${{ runner.os }}-${{ steps.package-manager.outputs.manager }}-
```

**Cache Paths by Manager:**

- **PNPM**: `~/.pnpm-store`
- **Yarn**: `~/.cache/yarn`
- **NPM**: `~/.npm`

### 4. Enhanced Version Management

Version updates now use the appropriate command for each package manager:

```yaml
# Update package.json using the detected package manager
if [[ "${{ steps.package-manager.outputs.manager }}" == "yarn" ]]; then
  yarn version --new-version $NEW_VERSION --no-git-tag-version
elif [[ "${{ steps.package-manager.outputs.manager }}" == "pnpm" ]]; then
  pnpm version $NEW_VERSION --no-git-tag-version
else
  npm version $NEW_VERSION --no-git-tag-version
fi
```

## 📊 Workflow Comparison

### Before (Manual Configuration Required)

- ❌ Hardcoded repository name (`<Repo_Name>`)
- ❌ Fixed to Yarn package manager only
- ❌ Manual cache configuration
- ❌ Required editing for each repository

### After (Universal & Dynamic)

- ✅ Automatic repository name detection
- ✅ Universal package manager support (PNPM, Yarn, NPM)
- ✅ Dynamic cache configuration
- ✅ Zero configuration for new repositories
- ✅ Intelligent fallback mechanisms

## 🚀 Benefits for Users

### 1. **Zero Configuration**

- Copy workflow to any React Native/Android project
- No manual editing required
- Automatic adaptation to project setup

### 2. **Package Manager Flexibility**

- Use your preferred package manager
- Workflows adapt automatically
- Optimal performance for each manager

### 3. **Consistent Naming**

- APK files automatically named after repository
- Release titles include repository name
- Professional release output

### 4. **Improved Performance**

- Optimized caching for each package manager
- Faster builds with proper cache strategies
- Reduced download times

## 📁 File Structure

```
APK Release/
├── RN-Apk-release/           # React Native focused workflow
│   ├── workflows/
│   │   └── release-apk.yml   # Enhanced with universal support
│   ├── README.md             # Comprehensive documentation
│   ├── RELEASE_WORKFLOW.md   # Usage guide
│   └── SECRETS_SETUP.md      # Security setup
│
└── Apk-Release/              # General Android workflow
    ├── workflows/
    │   └── release-apk.yml   # Enhanced with universal support
    ├── README.md             # Comprehensive documentation
    ├── RELEASE_WORKFLOW.md   # Usage guide
    └── SECRETS_SETUP.md      # Security setup
```

## 🔧 Migration Guide

### For Existing Users:

1. **Update workflow file**: Replace with new universal version
2. **Verify secrets**: Ensure all required secrets are still set
3. **Test with PR**: Create test PR with `release` label
4. **Check output**: Verify APK naming and release creation

### For New Users:

1. **Copy workflow**: Use either RN-Apk-release or Apk-Release
2. **Set up secrets**: Configure Android signing secrets
3. **Add to repository**: Place in `.github/workflows/`
4. **Create release**: Add `release` label to merged PR

## 🎯 Next Steps

1. **Test workflows**: Verify functionality across different projects
2. **Documentation**: Update any existing documentation
3. **Templates**: Consider creating project templates
4. **Monitoring**: Set up monitoring for workflow success rates

The workflows are now truly universal and require zero configuration for new repositories while maintaining all existing functionality and improving performance through intelligent package manager detection.
