# 🔄 GitHub Workflows Collection

A comprehensive collection of GitHub Actions workflows designed to automate various development tasks, from APK releases to AI-powered code analysis and project management.

## 📁 Repository Structure

### 🚀 APK Release Workflows

Automated Android APK building and release workflows for different package managers:

- **`Apk-Release-Adaptable/`** - Smart APK release workflow with automatic version bumping
- **`Apk-release-npm/`** - APK release workflow optimized for NPM-based projects
- **`RN-Apk-release-yarn/`** - React Native APK release workflow using Yarn package manager

**Key Features:**

- 🎯 Smart release detection via PR labels or titles
- 📈 Automatic version management (major/minor/patch)
- 🔐 Signed APK builds with secure keystore handling
- 📝 Auto-generated release notes

### 🤖 Claude AI Workflows

Intelligent workflows powered by Claude AI for automated code analysis and generation:

- **`claude-audit.yml`** - Comprehensive security & code quality auditing
- **`claude-generate.yml`** - AI-powered code generation from issue descriptions
- **`claude-organize.yml`** - Task organization and markdown beautification
- **`claude-pr-review.yml`** - Intelligent pull request reviews
- **`claude-readme.yml`** - Smart README generation and updates
- **`claude-test.yml`** - Automated test generation with multiple frameworks

**Key Features:**

- 🧠 AI-powered code analysis and suggestions
- 🔍 Security vulnerability scanning
- 📋 Automated documentation generation
- 🧪 Intelligent test creation

### 📝 Templates

Ready-to-use templates for consistent project management:

- **`PULL_REQUEST_TEMPLATE.md`** - Standardized PR format
- **`ISSUE_TEMPLATE/`** - Collection of issue templates:
  - `bug_report.yml` - Bug reporting template
  - `documentation_bug.yml` - Documentation issue template
  - `enhancement_request.yml` - Enhancement request template
  - `feature_request.yml` - Feature request template

## 🚀 Quick Start

1. **Choose your workflow category** based on your project needs
2. **Copy the relevant workflow files** to your project's `.github/workflows/` directory
3. **Configure required secrets** (see individual workflow documentation)
4. **Customize triggers and parameters** as needed

## 📚 Documentation

Each workflow category includes detailed documentation:

- `RELEASE_WORKFLOW.md` - Setup and usage instructions
- `SECRETS_SETUP.md` - Required secrets configuration
- Individual README files with specific details

## 🔧 Requirements

### For APK Release Workflows:

- Android project with Gradle build system
- Release keystore and signing credentials
- GitHub repository secrets configured

### For Claude AI Workflows:

- AWS Bedrock access with Claude AI models
- Package.json file in repository (recommended)
- Appropriate repository permissions

## 🤝 Contributing

Feel free to submit issues, feature requests, or pull requests to improve these workflows. Each workflow is designed to be modular and easily customizable for different project requirements.

## 📄 License

These workflows are provided as-is for educational and development purposes. Please review and test thoroughly before using in production environments.
