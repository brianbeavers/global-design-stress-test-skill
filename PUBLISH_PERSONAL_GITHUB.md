# Publish to GitHub (personal account)

Your skill files live in this folder. Follow these steps to put them on **your personal** GitHub as a public repo.

## Before you start

- **Browser:** logged into your **personal** GitHub at github.com
- **Terminal:** must use the **same personal account** (not a work/enterprise account)

Check the terminal:

```bash
gh auth status
```

If the username is your work account, run `gh auth login` and sign in with your personal account, or `gh auth switch` to select it.

---

## Step 1 — Create an empty repo (browser)

1. Open **https://github.com/new**
2. **Repository name:** `global-design-stress-test-skill`
3. **Public**
4. **Do not** add README, .gitignore, or license (this folder already has them)
5. Click **Create repository**
6. Note your username from the URL: `https://github.com/**username**/global-design-stress-test-skill`

---

## Step 2 — Push from this folder

Replace `username` with yours:

```bash
cd path/to/global-design-stress-test-skill

git remote remove origin 2>/dev/null
git remote add origin https://github.com/username/global-design-stress-test-skill.git

git push -u origin main
```

If `git push` fails with permission errors, run `gh auth login` again with your personal account.

---

## Step 3 — Share with others

```bash
git clone https://github.com/username/global-design-stress-test-skill.git ~/.cursor/skills/global-design-stress-test
```

---

## Step 4 — Install in Cursor

```bash
mkdir -p ~/.cursor/skills
git clone https://github.com/username/global-design-stress-test-skill.git ~/.cursor/skills/global-design-stress-test
```

In Cursor: *"Run global-design-stress-test on [your Figma URL]"*

---

## Updating the repo later

```bash
cd path/to/global-design-stress-test-skill
git add -A
git commit -m "Describe your change"
git push
```

---

## No `gh` CLI?

1. Create the empty repo on github.com (Step 1)
2. Generate a Personal Access Token: GitHub → Settings → Developer settings → Tokens (classic) → scope **repo**
3. Push; when asked for a password, paste the **token**
