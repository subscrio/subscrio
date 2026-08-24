# Security

## Protecting Secrets

This repository does not commit:

- **Environment files** (`.env`, `.env.local`, etc.) – use `.env.example` as a template
- **Database config** – `tests/appsettings.json` in [subscrio-dotnet](https://github.com/subscrio/subscrio-dotnet) is gitignored; copy from `appsettings.example.json`
- **Keys/secrets** – `.pfx`, `.pem`, `secrets.json`, `appsettings.Secrets.json` are ignored

## If Credentials Were Committed

If you previously committed real credentials and need to remove them from history:

1. **Rotate the exposed credentials** (change database password, regenerate keys).

2. **Rewrite history** using [git-filter-repo](https://github.com/newren/git-filter-repo):
   ```bash
   pip install git-filter-repo
   echo "EXPOSED_SECRET==>REPLACED" > replacements.txt
   git filter-repo --replace-text replacements.txt
   ```
   Or use [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/).

3. **Force-push** (coordinate with collaborators; this rewrites history):
   ```bash
   git push --force
   ```
