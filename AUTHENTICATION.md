# GitHub Authentication Guide

This guide helps you resolve authentication issues when pushing to GitHub repositories.

## The Problem
```
remote: Invalid username or token. Password authentication is not supported for Git operations.
fatal: Authentication failed for 'https://github.com/username/repo.git/'
```

## Solution Options

### 🔑 Option 1: Personal Access Token (Recommended)

1. **Create Token**
   - Go to [GitHub Settings → Tokens](https://github.com/settings/tokens)
   - Click "Generate new token (classic)"
   - Select `repo` scope for full repository access
   - Set expiration date (recommended: 90 days)
   - Click "Generate token"

2. **Copy and Save Token**
   - Copy the generated token immediately
   - Store it securely (you won't see it again)

3. **Use Token**
   - When Git prompts for password, use the token instead
   - Username: your GitHub username
   - Password: the personal access token

### 🔐 Option 2: SSH Key Setup

1. **Generate SSH Key**
   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

2. **Add to SSH Agent**
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Copy Public Key**
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

4. **Add to GitHub**
   - Go to [GitHub SSH Keys](https://github.com/settings/keys)
   - Click "New SSH key"
   - Paste your public key

5. **Change Remote URL**
   ```bash
   git remote set-url origin git@github.com:username/repo.git
   ```

### 💡 Quick Fix Commands

```bash
# Set up Git user (if needed)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Try pushing again (will prompt for credentials)
git push origin main
```

## Testing Authentication

```bash
# Test HTTPS with token
git ls-remote https://github.com/username/repo.git

# Test SSH
ssh -T git@github.com
```

## Credential Storage

### Windows (Git Credential Manager)
```bash
git config --global credential.helper manager-core
```

### macOS (Keychain)
```bash
git config --global credential.helper osxkeychain
```

### Linux (Store)
```bash
git config --global credential.helper store
```

## Troubleshooting

### Clear Stored Credentials
```bash
# Windows
git config --global --unset credential.helper
cmdkey /delete:git:https://github.com

# macOS
git config --global --unset credential.helper
git credential-osxkeychain erase

# Linux
git config --global --unset credential.helper
rm ~/.git-credentials
```

### Check Current Configuration
```bash
git config --list | grep credential
git remote -v
```

## Security Best Practices

1. **Use Personal Access Tokens** with minimal required scopes
2. **Set token expiration** dates
3. **Rotate tokens** regularly
4. **Use SSH keys** for better security
5. **Never commit credentials** to repositories
6. **Use credential managers** to store tokens securely

## Additional Resources

- [GitHub Authentication Documentation](https://docs.github.com/en/authentication)
- [SSH Key Generation Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [Personal Access Token Guide](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
