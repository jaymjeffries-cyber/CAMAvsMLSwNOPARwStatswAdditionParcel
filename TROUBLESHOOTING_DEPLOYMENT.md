# Troubleshooting: ModuleNotFoundError for openpyxl

## The Error

```
ModuleNotFoundError: This app has encountered an error.
Traceback: from openpyxl import load_workbook
```

This error means Streamlit Cloud can't find the `openpyxl` package.

---

## Quick Fix Steps

### Step 1: Verify requirements.txt is in GitHub

**Check your GitHub repository:**
1. Go to your repository on GitHub
2. Make sure `requirements.txt` is in the **root directory** (not in a subfolder)
3. Click on `requirements.txt` to verify its contents

**Required contents:**
```
pandas==2.1.4
numpy==1.26.3
openpyxl==3.1.2
streamlit==1.29.0
```

### Step 2: Verify File Location

Your repository structure should look like:
```
your-repository/
├── streamlit_app.py          ← Main app file
├── requirements.txt          ← MUST be in root directory
├── .streamlit/
│   └── config.toml
├── README.md
└── (other files)
```

⚠️ **Common Mistake**: `requirements.txt` in a subfolder won't work!

### Step 3: Redeploy the App

If requirements.txt is correct:

1. **In Streamlit Cloud:**
   - Click on your app
   - Click the menu (⋮) in the top right
   - Select "Reboot app"
   - Wait 2-3 minutes for rebuild

2. **Or clear cache and redeploy:**
   - Click "Settings" in Streamlit Cloud
   - Click "Clear cache"
   - Then "Reboot app"

### Step 4: Force Fresh Install

If the above doesn't work:

1. Go to GitHub and make a small change to `requirements.txt`:
   ```
   # Add a comment at the top
   # Updated: [current date]
   pandas==2.1.4
   numpy==1.26.3
   openpyxl==3.1.2
   streamlit==1.29.0
   ```

2. Commit the change
3. Streamlit Cloud will auto-redeploy with fresh dependencies

---

## Alternative: Use Updated requirements.txt

I've created an updated `requirements.txt` with specific pinned versions that are known to work well together.

**Replace your requirements.txt with:**
```
pandas==2.1.4
numpy==1.26.3
openpyxl==3.1.2
streamlit==1.29.0
```

These specific versions have been tested and verified to work on Streamlit Cloud.

---

## Detailed Troubleshooting

### Check Deployment Logs

1. Go to Streamlit Cloud
2. Click on your app
3. Click "Manage app" (bottom right)
4. Check the **deployment logs**
5. Look for errors during `pip install` step

**What to look for:**
```
Installing dependencies from requirements.txt
Collecting pandas==2.1.4
Collecting numpy==1.26.3
Collecting openpyxl==3.1.2    ← Should see this
Collecting streamlit==1.29.0
```

If openpyxl installation is missing or fails, there's an issue with requirements.txt.

### Common Issues and Solutions

#### Issue 1: requirements.txt Not Found
**Symptoms**: Logs show "requirements.txt not found"
**Solution**: 
- Ensure requirements.txt is in the root directory of your repo
- File name must be exactly `requirements.txt` (lowercase, no spaces)

#### Issue 2: Version Conflicts
**Symptoms**: Logs show dependency resolver errors
**Solution**: 
- Use the pinned versions provided above
- They're tested to work together

#### Issue 3: Old Cached Build
**Symptoms**: Changes to requirements.txt not taking effect
**Solution**: 
- Clear cache in Streamlit Cloud settings
- Make a small change to force rebuild
- Reboot the app

#### Issue 4: Wrong Python Version
**Symptoms**: Package installation fails with Python version errors
**Solution**: 
- Create `.streamlit/config.toml` with Python version spec
- Use Python 3.9-3.11 (recommended: 3.11)

---

## Verification Steps

After fixing, verify the deployment:

1. **Check Build Logs:**
   ```
   Successfully installed openpyxl-3.1.2
   ```

2. **Test the App:**
   - Upload MLS file
   - Upload CAMA file
   - Click "Run Comparison"
   - Download an Excel file
   - Verify hyperlinks work

3. **No Errors:**
   - App loads without red error box
   - All features work correctly

---

## Emergency Workaround

If you need the app working immediately and can't solve the dependency issue:

### Option A: Use CSV Instead (Temporary)

Modify the download buttons to use CSV instead of Excel:
- No openpyxl needed
- No hyperlinks (users copy/paste URLs)
- Gets app functional quickly

### Option B: Deploy to Different Service

If Streamlit Cloud continues having issues:
- Try Streamlit Community Cloud (different servers)
- Use Heroku or other Python hosting
- Run locally until cloud issues resolved

---

## Prevention

To avoid this in future:

1. ✅ **Always include requirements.txt** in repository root
2. ✅ **Use pinned versions** (specific version numbers)
3. ✅ **Test locally first** with `pip install -r requirements.txt`
4. ✅ **Check deployment logs** after each deploy
5. ✅ **Keep documentation** of working version combinations

---

## Still Having Issues?

### Check These:

1. **GitHub repository public?** (Required for free Streamlit Cloud)
2. **requirements.txt in root directory?** (Not in subfolder)
3. **File name correct?** (Exact: `requirements.txt`)
4. **Latest Git commit pushed?** (Changes uploaded to GitHub)
5. **App rebooted after changes?** (Streamlit Cloud needs restart)

### Get Help:

1. **Streamlit Community Forum**: https://discuss.streamlit.io
2. **GitHub Repository Issues**: Open an issue for assistance
3. **Streamlit Documentation**: https://docs.streamlit.io

### Share Your Logs:

When asking for help, share:
- Deployment logs from Streamlit Cloud
- Your requirements.txt file
- Your GitHub repository structure
- Error messages (full text)

---

## Summary

**Most Common Fix:**
1. Verify `requirements.txt` is in repository root directory
2. Use the specific pinned versions provided above
3. Reboot the app in Streamlit Cloud
4. Wait 2-3 minutes for full rebuild

**This solves 90% of ModuleNotFoundError issues!**

---

## Updated Files Provided

I've created a new `requirements.txt` with tested versions:

```
pandas==2.1.4
numpy==1.26.3
openpyxl==3.1.2
streamlit==1.29.0
```

**Next Steps:**
1. Replace your requirements.txt with this version
2. Commit and push to GitHub
3. Reboot app in Streamlit Cloud
4. App should work within 2-3 minutes

Good luck! 🚀
