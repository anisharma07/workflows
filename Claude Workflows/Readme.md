# Claude Workflows Documentation

This repository contains several Claude AI-powered GitHub workflows designed to automate various development tasks. Each workflow leverages Claude AI through AWS Bedrock to provide intelligent automation capabilities.

NOTE: GITHUB ACTIONS WORK WELL WITH REPO CONTAINING PACKAGE.json and its lock file

## � Workflow Architecture Overview

```mermaid
graph TB
    subgraph "Manual Workflows"
        A[claude-audit.yml] --> A1[Code Analysis]
        B[claude-readme.yml] --> B1[README Generation]
        C[claude-test.yml] --> C1[Test Generation]
    end

    subgraph "Automated Workflows"
        D[claude-generate.yml] --> D1[Code Generation]
        E[claude-pr-review.yml] --> E1[PR Review]
        F[claude-organize.yml] --> F1[Content Organization]
    end

    subgraph "External Triggers"
        G[Manual Dispatch] --> A
        G --> B
        G --> C
        H[Issues/Comments] --> D
        I[Pull Requests] --> E
        J[Comments with @claude organize] --> F
    end

    subgraph "AWS Integration"
        K[AWS Bedrock] --> L[Claude AI Models]
        L --> M[Intelligent Processing]
    end

    A1 --> K
    B1 --> K
    C1 --> K
    D1 --> K
    E1 --> K
    F1 --> K

    M --> N[Generate Pull Requests]
    M --> O[Update Comments]
    M --> P[Create Branches]
```

## �📋 Overview

| Workflow                              | Trigger Type  | Description                                        |
| ------------------------------------- | ------------- | -------------------------------------------------- |
| [claude-audit](#claude-audit)         | Manual        | Comprehensive codebase auditing and analysis       |
| [claude-readme](#claude-readme)       | Manual        | Intelligent README generation and updates          |
| [claude-test](#claude-test)           | Manual        | Automated test generation with multiple frameworks |
| [claude-generate](#claude-generate)   | Comment/Label | AI-powered code generation from issue descriptions |
| [claude-pr-review](#claude-pr-review) | Automatic     | Intelligent pull request review and feedback       |
| [claude-organize](#claude-organize)   | Comment       | Task organization and markdown beautification      |

## 🔧 Manual Workflows

These workflows must be triggered manually from the Actions tab in your GitHub repository.

### claude-audit

**File:** `.github/workflows/claude-audit.yml`  
**Trigger:** Manual (workflow_dispatch)

Performs comprehensive code auditing and analysis of your codebase using Claude AI.

```mermaid
flowchart TD
    A[Manual Trigger] --> B[Check/Delete Existing Branch]
    B --> C[Setup Environment]
    C --> D[Configure AWS Credentials]
    D --> E[Install Dependencies]
    E --> F[Analyze Project Structure]
    F --> G[Run Security Scans]

    subgraph "Security Analysis"
        G --> G1[Node.js Vulnerabilities]
        G --> G2[Python Security - Bandit]
        G --> G3[Static Analysis - Semgrep]
        G --> G4[ESLint Analysis]
        G --> G5[Dead Code Analysis]
        G --> G6[Shell Script Analysis]
    end

    G1 --> H[Generate Audit Report with Claude]
    G2 --> H
    G3 --> H
    G4 --> H
    G5 --> H
    G6 --> H

    H --> I[Create claude-audit Branch]
    I --> J[Commit Audit Results]
    J --> K[Create Pull Request]
    K --> L[Summary Report]
```

**Features:**

- **Audit Scopes:** Choose from `all`, `security`, `performance`, `maintainability`, or `cleanup`
- **Improvement Suggestions:** Optional detailed recommendations
- **Automated Branch Management:** Creates and manages `claude-code-audit` branch
- **Pull Request Creation:** Generates PR with audit findings and suggestions

**How to Use:**

1. Go to Actions tab → Claude AI Code Audit
2. Click "Run workflow"
3. Select audit scope and options
4. Review the generated PR with audit results

---

### claude-readme

**File:** `.github/workflows/claude-readme.yml`  
**Trigger:** Manual (workflow_dispatch)

Generates comprehensive, intelligent README files for your project.

```mermaid
flowchart TD
    A[Manual Trigger] --> B[Check/Delete Existing Branch]
    B --> C[Setup Environment]
    C --> D[Configure AWS Credentials]
    D --> E[Install Dependencies & Tools]
    E --> F[Analyze Project Structure]

    subgraph "Project Analysis"
        F --> F1[Generate File Inventory]
        F --> F2[Create Directory Tree]
        F --> F3[Analyze Config Files]
        F --> F4[Count File Types]
        F --> F5[Examine Source Structure]
    end

    F1 --> G[Generate README with Claude AI]
    F2 --> G
    F3 --> G
    F4 --> G
    F5 --> G

    G --> H[Create claude-readme Branch]
    H --> I[Commit README Changes]
    I --> J[Create Pull Request]
    J --> K[Summary Report]
```

**Features:**

- **Project Structure Analysis:** Detailed breakdown of your codebase
- **Tech Stack Detection:** Automatic identification of technologies used
- **README Updates:** Can update existing README files
- **Branch Management:** Uses `claude-readme` branch for changes

**How to Use:**

1. Go to Actions tab → Claude AI README Generator
2. Click "Run workflow"
3. Configure options (detailed structure, tech analysis, update existing)
4. Review and merge the generated README

---

### claude-test

**File:** `.github/workflows/claude-test.yml`  
**Trigger:** Manual (workflow_dispatch)

Generates comprehensive test suites for your codebase using various testing frameworks.

```mermaid
flowchart TD
    A[Manual Trigger] --> B[Check/Delete Existing Branch]
    B --> C[Setup Environment]
    C --> D[Configure AWS Credentials]
    D --> E[Install Dependencies & Tools]
    E --> F[Analyze Project for Testing]

    subgraph "Test Analysis"
        F --> F1[Find Source Files]
        F --> F2[Identify Existing Tests]
        F --> F3[Analyze Project Structure]
        F --> F4[Check Test Configurations]
        F --> F5[Examine Code Patterns]
    end

    F1 --> G[Generate Tests with Claude AI]
    F2 --> G
    F3 --> G
    F4 --> G
    F5 --> G

    subgraph "Test Generation"
        G --> G1[Unit Tests]
        G --> G2[Integration Tests]
        G --> G3[Component Tests]
        G --> G4[E2E Tests]
        G --> G5[Test Configuration]
        G --> G6[Test Documentation]
    end

    G1 --> H[Create claude-test Branch]
    G2 --> H
    G3 --> H
    G4 --> H
    G5 --> H
    G6 --> H

    H --> I[Commit Test Files]
    I --> J[Create Pull Request]
    J --> K[Summary Report]
```

**Features:**

- **Test Types:** Support for `unit`, `integration`, `e2e`, and `component` tests
- **Framework Support:** Auto-detection or manual selection (Jest, Vitest, Cypress, Playwright)
- **Coverage Targeting:** Configurable coverage percentage goals
- **Test Documentation:** Generates README for test suites

**How to Use:**

1. Go to Actions tab → Claude AI Test Generator
2. Click "Run workflow"
3. Select test type, framework, and coverage target
4. Review generated tests in the PR

## 🤖 Automated Workflows

These workflows trigger automatically based on specific conditions.

### claude-generate

**File:** `.github/workflows/claude-generate.yml`  
**Triggers:**

- Issues with `claude-generate` label
- Comments containing `/claude generate`
- Manual workflow dispatch

Generates code based on issue descriptions or comments using Claude AI.

```mermaid
flowchart TD
    A[Issue with claude-generate label] --> D[Extract Task Description]
    B[Comment with /claude generate] --> D
    C[Manual Workflow Dispatch] --> D

    D --> E[Setup Environment]
    E --> F[Configure AWS Credentials]
    F --> G[Install Dependencies]
    G --> H[Analyze Codebase Structure]

    subgraph "Codebase Analysis"
        H --> H1[Find Source Files]
        H --> H2[Read Project Context]
        H --> H3[Analyze Package.json]
        H --> H4[Examine File Structure]
    end

    H1 --> I[Generate Code with Claude]
    H2 --> I
    H3 --> I
    H4 --> I

    subgraph "Code Generation"
        I --> I1[Analyze Requirements]
        I --> I2[Generate New Files]
        I --> I3[Modify Existing Files]
        I --> I4[Create Instructions]
    end

    I1 --> J[Create Target Branch]
    I2 --> J
    I3 --> J
    I4 --> J

    J --> K[Commit Generated Code]
    K --> L[Create Pull Request]
    L --> M[Add Review Notes]
```

**Features:**

- **Issue-Based Generation:** Creates code from GitHub issue descriptions
- **Comment Commands:** Use `/claude generate` in issue comments
- **Flexible Targeting:** Specify target branch for generated code
- **AWS Integration:** Uses AWS Bedrock for Claude AI access

**How to Use:**

1. **Via Issue Label:** Add `claude-generate` label to any issue
2. **Via Comment:** Type `/claude generate` in an issue comment
3. **Manual Trigger:** Use workflow dispatch with task description

---

### claude-pr-review

**File:** `.github/workflows/claude-pr-review.yml`  
**Triggers:**

- Pull request opened/synchronized/reopened
- Pull request review comments created

Provides intelligent code review using Claude AI for all pull requests.

```mermaid
flowchart TD
    A[PR Opened/Updated] --> C[Checkout Code]
    B[PR Review Comment] --> C

    C --> D[Get PR Changes]
    D --> E[List Changed Files]
    E --> F[Generate Diff Content]
    F --> G[Configure AWS Credentials]
    G --> H[Setup Python Environment]
    H --> I[Install Dependencies]

    I --> J[Analyze with Claude AI]

    subgraph "Claude Analysis"
        J --> J1[Review Code Changes]
        J --> J2[Identify Potential Issues]
        J --> J3[Assess Code Quality]
        J --> J4[Check Security Concerns]
        J --> J5[Evaluate Performance]
        J --> J6[Suggest Improvements]
    end

    J1 --> K[Post PR Review Comment]
    J2 --> K
    J3 --> K
    J4 --> K
    J5 --> K
    J6 --> K

    K --> L[Review Published]
```

**Features:**

- **Automatic Review:** Reviews all PR changes automatically
- **Diff Analysis:** Analyzes code changes and provides feedback
- **Comment Integration:** Responds to review comments
- **Multi-Language Support:** Works with various programming languages

**How to Use:**

- **Automatic:** Reviews trigger automatically on PR events
- **No manual action required**
- Claude will comment directly on your PR with review feedback

---

### claude-organize

**File:** `.github/workflows/claude-organize.yml`  
**Trigger:** Comments containing `@claude organize`

Organizes and beautifies task lists and content using Claude AI with markdown formatting.

```mermaid
flowchart TD
    A[Comment with @claude organize] --> B[Checkout Code]
    B --> C[Configure AWS Credentials]
    C --> D[Setup Python Environment]
    D --> E[Install Dependencies]
    E --> F[Extract Comment Content]

    F --> G[Clean Comment Text]
    G --> H[Remove @claude organize trigger]
    H --> I[Send to Claude AI]

    subgraph "Claude Processing"
        I --> I1[Analyze Content Structure]
        I --> I2[Create Clear Sections]
        I --> I3[Apply Bullet Points]
        I --> I4[Group Related Information]
        I --> I5[Format with Markdown]
        I --> I6[Organize Action Items]
    end

    I1 --> J[Update Original Comment]
    I2 --> J
    I3 --> J
    I4 --> J
    I5 --> J
    I6 --> J

    J --> K[Organized Content Published]
```

**Features:**

- **Task Organization:** Restructures and organizes task lists
- **Markdown Beautification:** Applies proper markdown formatting
- **Comment Enhancement:** Improves readability and structure
- **Real-time Processing:** Processes comments as they're created

**How to Use:**

1. Type `@claude organize` followed by your tasks or content in any issue or PR comment
2. Claude will automatically organize and beautify the content
3. The organized content will be posted as a reply comment

**Example:**

```
@claude organize
- Fix bug in user authentication
- Update documentation
- Add unit tests
- Deploy to staging
```

## 🔐 Required Secrets

All workflows require the following GitHub secrets to be configured:

| Secret                  | Description                                                       |
| ----------------------- | ----------------------------------------------------------------- |
| `AWS_ACCESS_KEY_ID`     | AWS access key for Bedrock access                                 |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key for Bedrock access                                 |
| `AWS_REGION`            | AWS region (e.g., `us-east-1`)                                    |
| `AWS_BEDROCK_MODEL_ID`  | Claude model ID (e.g., `anthropic.claude-3-sonnet-20240229-v1:0`) |
| `GITHUB_TOKEN`          | GitHub personal access token (automatically provided)             |

## 🚀 Getting Started

1. **Configure AWS Bedrock:** Ensure you have access to Claude models in AWS Bedrock
2. **Set Repository Secrets:** Add all required secrets in repository settings
3. **Enable Actions:** Ensure GitHub Actions are enabled for your repository
4. **Grant Permissions:** Workflows need write access to contents, pull requests, and issues

## 📝 Notes

- All workflows create separate branches for their changes to maintain clean git history
- Existing branches are automatically cleaned up before creating new ones
- Pull requests are created automatically for review and merging
- Workflows are designed to be safe and non-destructive to your main codebase

## 🤝 Contributing

When modifying workflows:

1. Test changes in a fork first
2. Ensure all required secrets are properly configured
3. Update this documentation for any new features
4. Follow the existing patterns for consistency
