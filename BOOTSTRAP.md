# Bootstrap: How to set up the public picks repo (one-time)

This folder is the **template** for a separate, public repo on GitHub that
will host the rebalance picks files with tamper-evident OpenTimestamps proofs.

## Steps (one-time, ~5 minutes)

### 1. Create a new public repo on GitHub

Go to https://github.com/new

- **Owner:** `doubledown3301` (your account)
- **Name:** `stockexperimentai-picks`
- **Visibility:** **Public**
- Initialize with: nothing (no README, no .gitignore, no license — we'll provide our own)

### 2. Clone it locally (sibling of the main project)

```bash
cd /c/Users/dishn/
git clone https://github.com/doubledown3301/stockexperimentai-picks.git
```

This creates `c:/Users/dishn/stockexperimentai-picks/` (a new directory next to `StockExperimentAI/`).

### 3. Copy template contents into the clone

```bash
cd /c/Users/dishn/stockexperimentai-picks

# Copy everything from this template (including .github/, picks/, README.md, LICENSE, .gitignore)
cp -r /c/Users/dishn/StockExperimentAI/public-picks-template/. .

# Verify
ls -la
ls -la picks/
ls -la .github/workflows/
```

You should see: `README.md`, `LICENSE`, `.gitignore`, `picks/03022026.csv`, `.github/workflows/timestamp.yml`.

### 4. Initial commit and push

```bash
git add .
git commit -m "Initial public picks repo with March 2 archive"
git push
```

### 5. Verify the GitHub Actions workflow runs

Visit https://github.com/doubledown3301/stockexperimentai-picks/actions

The "Timestamp picks files" workflow should trigger on the initial push and add `picks/03022026.csv.ots` to the repo. (Note: this is just a starting anchor for the March 2 file — it doesn't retroactively prove the file existed back on March 2. It just locks the file's bytes from any future tampering.)

### 6. After each rebalance, sync the new picks file

From the main project directory, run:

```bash
cd /c/Users/dishn/StockExperimentAI
python scripts/sync_picks_public.py 04302026
```

The script will copy `BackTests/Live_Scoring/archive/04302026.csv` into the public repo, commit, and push. GitHub Actions will then stamp the file with OpenTimestamps within ~5 minutes of the push, and commit the `.ots` proof back to the repo.

---

## Removing the template after bootstrap

Once you've pushed the public repo successfully, you can leave `public-picks-template/` in the main project repo as-is — it's a small static reference. If you prefer to remove it from the main repo to keep things clean:

```bash
cd /c/Users/dishn/StockExperimentAI
git rm -r public-picks-template
git commit -m "Remove public picks template (now bootstrapped)"
```

The template is also useful for re-bootstrapping if the public repo gets nuked.
