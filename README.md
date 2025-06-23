# Build CI/CD pipeline


# CI/CD Workflow History and Node.js Project Setup

## 🧠 History of Software Integration

In a typical pre-CI/CD developer workflow, teams worked across multiple branches or feature forks in the same repository. Developers manually ran smoke tests and integration tests locally before pushing changes. This often led to:

- Days or even weeks of delays to test and merge features
- Inconsistent environments
- Delayed feedback and harder debugging

## 🚀 Modern CI/CD Advantages

With automated workflows, integration testing, builds, and deployments now happen within **hours**, enabling **faster, safer merges** into the main branch.

### Continuous Integration Best Practices

- ✅ Automate the build process
- ✅ Run automated tests:
  - Unit tests (minimum requirement)
  - Integration tests
  - UI and end-to-end tests
- ✅ Apply linting for unified code style
- ✅ Perform security checks and scans
  - e.g., [CodeQL](https://github.com/github/codeql) for security scanning

---

## ⚙️ CI for a Node.js Project

- CI should **deploy only to staging**, never directly to production.
- Pull Requests are tested in isolated staging environments before merging into `main`.

### Why Isolated Environments?

In large teams, it's best to **avoid using a shared EC2 instance** for all PRs. Instead:

1. **Spin up a new EC2 instance per PR** using tools like **Terraform**
2. **SSH into the instance**
3. Use `rsync` to copy the build artifacts from the GitHub Actions runner to the EC2 instance
4. Run post-deployment steps like:
   - `npm ci`
   - Restart services (e.g., `pm2`, `nginx`)
   - Run staging tests

---

This setup ensures:
- High environment consistency
- Safe, isolated integration testing
- Smooth transition from PR to production-ready code