# Contributing to MindGuard

First off, thank you for considering contributing to MindGuard! 🎉

MindGuard is an AI-driven mental health early detection system, and we take both code quality and user safety very seriously. This document provides guidelines for contributing to the project.

---

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Process](#development-process)
- [Style Guidelines](#style-guidelines)
- [Commit Messages](#commit-messages)
- [Pull Request Process](#pull-request-process)
- [Community](#community)

---

## 📜 Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to [conduct@mindguard.io](mailto:conduct@mindguard.io).

---

## 🚀 Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** 18+ and npm
- **Python** 3.11+
- **Docker** and Docker Compose
- **Git**

### Setting Up Your Development Environment

1. **Fork the repository**
   ```bash
   # Click the "Fork" button on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR-USERNAME/MindGuard.git
   cd MindGuard
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/kzahhar611/MindGuard.git
   ```

4. **Install dependencies**
   ```bash
   # Frontend
   cd frontend && npm install
   
   # Backend
   cd ../backend && pip install -r requirements.txt
   ```

5. **Start development environment**
   ```bash
   docker-compose up -d  # Start databases
   npm run dev           # Start frontend
   uvicorn main:app --reload  # Start backend
   ```

---

## 🤝 How Can I Contribute?

### 🐛 Reporting Bugs

Before creating bug reports, please check the [existing issues](https://github.com/kzahhar611/MindGuard/issues) to avoid duplicates.

**When reporting a bug, include:**

- Clear, descriptive title
- Steps to reproduce the behavior
- Expected behavior vs actual behavior
- Screenshots if applicable
- Environment details (OS, browser, version)

**Bug Report Template:**
```markdown
**Describe the bug**
A clear description of what the bug is.

**To Reproduce**
1. Go to '...'
2. Click on '...'
3. See error

**Expected behavior**
What you expected to happen.

**Screenshots**
If applicable.

**Environment:**
- OS: [e.g., Windows 11]
- Browser: [e.g., Chrome 120]
- Version: [e.g., 1.0.0]
```

### 💡 Suggesting Features

Feature requests are welcome! Please provide:

- Clear use case and user benefit
- Potential implementation approach
- Any privacy/security considerations

### 🔧 Code Contributions

We welcome contributions in the following areas:

| Area | Description | Skills Needed |
|------|-------------|---------------|
| **Frontend** | Mobile (iOS/Android) and Web UI | Swift, Kotlin, React |
| **Backend** | API and microservices | Python, FastAPI, Node.js |
| **ML/AI** | Model development and training | PyTorch, TensorFlow |
| **DevOps** | CI/CD and infrastructure | Docker, Kubernetes, Terraform |
| **Documentation** | Docs and tutorials | Markdown, technical writing |
| **Testing** | Unit, integration, E2E tests | Jest, pytest, Cypress |

---

## 🔄 Development Process

### Branching Strategy

We use **GitFlow** with the following branches:

```
main          ← Production-ready code
  └── develop ← Integration branch
        └── feature/* ← New features
        └── bugfix/*  ← Bug fixes
        └── hotfix/*  ← Urgent production fixes
```

### Creating a Branch

```bash
# Sync with upstream
git fetch upstream
git checkout develop
git merge upstream/develop

# Create feature branch
git checkout -b feature/your-feature-name
```

### Branch Naming Convention

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/short-description` | `feature/voice-analysis-ui` |
| Bug Fix | `bugfix/issue-number-description` | `bugfix/123-login-error` |
| Hotfix | `hotfix/description` | `hotfix/security-patch` |

---

## 📝 Style Guidelines

### Python (Backend)

- Follow [PEP 8](https://pep8.org/)
- Use type hints
- Maximum line length: 100 characters
- Use `black` for formatting, `flake8` for linting

```python
# Good
def calculate_risk_score(
    text_features: TextFeatures,
    voice_features: VoiceFeatures | None = None,
) -> RiskScore:
    """Calculate multi-modal risk score."""
    ...
```

### TypeScript/JavaScript (Frontend)

- Follow [Airbnb Style Guide](https://github.com/airbnb/javascript)
- Use ESLint and Prettier
- Prefer functional components with hooks

```typescript
// Good
const RiskDashboard: React.FC<Props> = ({ score, trend }) => {
  const [isLoading, setIsLoading] = useState(false);
  // ...
};
```

### Swift (iOS)

- Follow [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- Use SwiftLint

### Kotlin (Android)

- Follow [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Use ktlint

---

## 💬 Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types

| Type | Description |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Code style (formatting, semicolons, etc.) |
| `refactor` | Code refactoring |
| `perf` | Performance improvement |
| `test` | Adding/fixing tests |
| `chore` | Maintenance tasks |
| `ci` | CI/CD changes |

### Examples

```bash
feat(nlp): add cognitive distortion detection model

fix(auth): resolve JWT token expiration issue (#123)

docs(readme): update installation instructions

refactor(risk): extract score calculation to separate module
```

---

## 🔀 Pull Request Process

### Before Submitting

- [ ] Code follows style guidelines
- [ ] All tests pass (`npm test`, `pytest`)
- [ ] New code has tests
- [ ] Documentation is updated
- [ ] No sensitive data in commits
- [ ] Branch is up-to-date with `develop`

### PR Template

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
Describe testing done.

## Checklist
- [ ] Tests pass
- [ ] Docs updated
- [ ] No sensitive data
```

### Review Process

1. Create PR against `develop` branch
2. Automated checks run (lint, tests, security)
3. Code review by at least 1 maintainer
4. Address feedback
5. Squash and merge

---

## 🔒 Security Considerations

Given the sensitive nature of mental health data:

- **Never** commit real user data
- **Never** log sensitive information
- **Always** use parameterized queries
- **Always** validate and sanitize inputs
- Report security vulnerabilities to [security@mindguard.io](mailto:security@mindguard.io)

---

## 🏆 Recognition

Contributors are recognized in:

- README.md contributors section
- Release notes
- Annual contributor spotlight

---

## 📞 Community

- **Discord**: [Join our server](https://discord.gg/mindguard)
- **Discussions**: [GitHub Discussions](https://github.com/kzahhar611/MindGuard/discussions)
- **Email**: [contributors@mindguard.io](mailto:contributors@mindguard.io)

---

Thank you for contributing to MindGuard! Together, we're making mental health care more proactive and accessible. 💚

---

*Last updated: December 2024*
