# Streamlit Cloud Deployment Checklist

## Before You Deploy

### ✅ Step 1: Verify Repository Structure

Your GitHub repository should have this structure:

```
your-repository-name/
├── streamlit_app.py              ✅ Main application file
├── requirements.txt              ✅ MUST be in root directory
├── .streamlit/
│   └── config.toml              ✅ App configuration
├── .gitignore                    ✅ Prevent committing data files
├── README.md                     ✅ Repository documentation
├── mls_cama_comparison.py        ⚪ Optional (standalone script)
└── (documentation files)         ⚪ Optional
```

**Critical Files:**
- ✅ `streamlit_app.py` - Main file
- ✅ `requirements.txt` - Dependencies (IN ROOT!)

**Optional Files:**
- ⚪ `.streamlit/config.toml` - Appearance settings
- ⚪ `.gitignore` - Prevents data file commits
- ⚪ `README.md` - Documentation

---

## Step 2: Verify requirements.txt

**Location Check:**
- [ ] File is in **root directory** (not in a subfolder)
- [ ] File name is exactly `requirements.txt` (lowercase)

**Content Check:**
```
pandas==2.1.4
numpy==1.26.3
openpyxl==3.1.2
streamlit==1.29.0
```

**What NOT to do:**
- ❌ Don't put requirements.txt in `.streamlit/` folder
- ❌ Don't name it `Requirements.txt` (wrong case)
- ❌ Don't use vague versions like `openpyxl>=3.0`

---

## Step 3: Verify Files are Committed

In your terminal:

```bash
# Check what files are tracked
git status

# Should show:
# On branch main
# nothing to commit, working tree clean

# If there are untracked files:
git add streamlit_app.py
git add requirements.txt
git add .streamlit/config.toml
git add .gitignore
git add README.md

# Commit
git commit -m "Deploy MLS CAMA comparison app"

# Push to GitHub
git push origin main
```

---

## Step 4: Deploy to Streamlit Cloud

### 4.1: Sign in to Streamlit Cloud
1. Go to https://streamlit.io/cloud
2. Sign in with GitHub
3. Authorize Streamlit to access your repositories

### 4.2: Create New App
1. Click **"New app"**
2. Select **"From existing repo"**
3. Choose your repository
4. Set **Branch**: `main`
5. Set **Main file path**: `streamlit_app.py`
6. Click **"Deploy"**

### 4.3: Wait for Build
- Initial deployment takes 2-5 minutes
- Watch the build logs for errors
- Look for: `Successfully installed openpyxl-3.1.2`

---

## Step 5: Verify Deployment

### Check Build Logs

**Good deployment looks like:**
```
Cloning repository...
Installing dependencies from requirements.txt
Collecting pandas==2.1.4
  Downloading pandas-2.1.4-cp311-cp311-manylinux_2_17_x86_64.whl
Collecting numpy==1.26.3
  Downloading numpy-1.26.3-cp311-cp311-manylinux_2_17_x86_64.whl
Collecting openpyxl==3.1.2      ✅ This line should appear!
  Downloading openpyxl-3.1.2-py2.py3-none-any.whl
Collecting streamlit==1.29.0
  Downloading streamlit-1.29.0-py2.py3-none-any.whl
Successfully installed openpyxl-3.1.2 pandas-2.1.4 numpy-1.26.3 streamlit-1.29.0
```

**Bad deployment looks like:**
```
ERROR: Could not find requirements.txt     ❌ File not in root!
ERROR: No matching distribution found      ❌ Version conflict!
ModuleNotFoundError: No module named 'openpyxl'  ❌ Not installed!
```

### Test the App

Once deployed:

1. **Load Test:**
   - [ ] App opens without errors
   - [ ] No red error boxes
   - [ ] Sidebar loads correctly

2. **Function Test:**
   - [ ] Upload MLS file (test file)
   - [ ] Upload CAMA file (test file)
   - [ ] Click "Run Comparison"
   - [ ] Results display correctly
   - [ ] Statistics show up

3. **Download Test:**
   - [ ] Click download buttons
   - [ ] Excel files download
   - [ ] Open Excel files
   - [ ] Hyperlinks work

---

## Common Issues and Quick Fixes

### Issue: "requirements.txt not found"
**Fix:** Move requirements.txt to root directory
```bash
# If it's in the wrong place:
mv subfolder/requirements.txt ./requirements.txt
git add requirements.txt
git commit -m "Fix requirements.txt location"
git push
```

### Issue: "ModuleNotFoundError: openpyxl"
**Fix:** Update requirements.txt with exact versions
```bash
# Replace contents with:
echo "pandas==2.1.4
numpy==1.26.3
openpyxl==3.1.2
streamlit==1.29.0" > requirements.txt

git add requirements.txt
git commit -m "Pin dependency versions"
git push
```

### Issue: Changes Not Taking Effect
**Fix:** Clear cache and reboot
1. Go to Streamlit Cloud
2. Click your app
3. Click menu (⋮)
4. Select "Reboot app"
5. Wait 2-3 minutes

### Issue: App Keeps Crashing
**Fix:** Check the logs
1. Click "Manage app" (bottom right)
2. Read the error messages
3. Look for specific error details
4. Fix the identified issue

---

## Deployment Verification Checklist

Before marking as complete, verify:

- [ ] ✅ Repository is public (required for free tier)
- [ ] ✅ `streamlit_app.py` in repository root
- [ ] ✅ `requirements.txt` in repository root with exact versions
- [ ] ✅ All files committed and pushed to GitHub
- [ ] ✅ App deployed to Streamlit Cloud
- [ ] ✅ Build logs show successful openpyxl installation
- [ ] ✅ App loads without errors
- [ ] ✅ File upload works
- [ ] ✅ Comparison runs successfully
- [ ] ✅ Statistics display correctly
- [ ] ✅ Excel downloads work
- [ ] ✅ Hyperlinks in Excel work

---

## Success!

If all checklist items are complete:

✅ **Your app is live!**

**Share your app:**
- Your app URL: `https://your-app-name.streamlit.app`
- Share with your team
- Bookmark for easy access

**Next steps:**
- Test with real data
- Gather user feedback
- Make improvements as needed

---

## Need Help?

If you're stuck on any step:

1. **Review the detailed guide:** `TROUBLESHOOTING_DEPLOYMENT.md`
2. **Check Streamlit docs:** https://docs.streamlit.io/streamlit-cloud/get-started/deploy-an-app
3. **Ask for help:** https://discuss.streamlit.io

---

## Quick Command Reference

```bash
# Check repository status
git status

# Add all files
git add .

# Commit changes
git commit -m "Your message"

# Push to GitHub
git push origin main

# View commit history
git log --oneline

# Check remote URL
git remote -v
```

---

## Emergency Rollback

If new deployment breaks:

1. Find last working commit: `git log --oneline`
2. Revert: `git revert [commit-hash]`
3. Push: `git push origin main`
4. Streamlit auto-redeploys to working version

---

**Good luck with your deployment!** 🚀
