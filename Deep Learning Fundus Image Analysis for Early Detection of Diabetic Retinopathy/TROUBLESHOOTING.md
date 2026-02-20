# Troubleshooting Guide

Common issues and solutions for the Diabetic Retinopathy Detection System.

## Table of Contents
1. [Installation Issues](#installation-issues)
2. [Model Loading Errors](#model-loading-errors)
3. [Flask Application Errors](#flask-application-errors)
4. [Cloudant Database Issues](#cloudant-database-issues)
5. [Prediction Errors](#prediction-errors)
6. [Performance Issues](#performance-issues)
7. [Browser Issues](#browser-issues)

---

## Installation Issues

### Problem: `pip install` fails with dependency conflicts

**Error:**
```
ERROR: Could not find a version that satisfies the requirement tensorflow==2.3.2
```

**Solutions:**

1. **Update pip first:**
```bash
python -m pip install --upgrade pip
```

2. **Try compatible versions:**
```bash
pip install tensorflow==2.12.0 keras==2.12.0 numpy==1.23.5
```

3. **Use Python 3.8:**
```bash
# Create conda environment with Python 3.8
conda create -n dr_detection python=3.8
conda activate dr_detection
pip install -r requirements.txt
```

### Problem: SSL Certificate verification failed

**Error:**
```
SSL: CERTIFICATE_VERIFY_FAILED
```

**Solution:**
```bash
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org -r requirements.txt
```

### Problem: Permission denied during installation

**Error:**
```
ERROR: Could not install packages due to an OSError: [WinError 5] Access is denied
```

**Solution - Windows:**
- Run Command Prompt/Anaconda Prompt as Administrator
- OR install for user only:
```bash
pip install --user -r requirements.txt
```

**Solution - Linux/Mac:**
```bash
sudo pip install -r requirements.txt
# OR
pip install --user -r requirements.txt
```

---

## Model Loading Errors

### Problem: Model file not found

**Error:**
```
Error loading model: [Errno 2] No such file or directory: 'model/Updated-Xception-diabetic-retinopathy.h5'
```

**Solution:**
1. Ensure the model file exists in `model/` folder
2. Check filename exactly matches: `Updated-Xception-diabetic-retinopathy.h5`
3. Train model using the Jupyter notebook in Google Colab
4. Download the `.h5` file after training

### Problem: Model loading fails with version mismatch

**Error:**
```
ValueError: Unknown layer: DepthwiseConv2D
```

**Solution:**
Update TensorFlow/Keras versions:
```bash
pip install tensorflow==2.12.0 keras==2.12.0
```

### Problem: Model file corrupted

**Error:**
```
OSError: Unable to open file (file signature not found)
```

**Solution:**
1. Re-download or re-train the model
2. Ensure model file is not corrupted during transfer
3. Verify file size (should be ~80-90 MB)

---

## Flask Application Errors

### Problem: Port 5000 already in use

**Error:**
```
OSError: [WinError 10048] Only one usage of each socket address is normally permitted
```

**Solution 1 - Change port in `app.py`:**
```python
app.run(debug=True, host='0.0.0.0', port=5001)  # Changed port
```

**Solution 2 - Kill process using port 5000:**

Windows:
```bash
netstat -ano | findstr :5000
taskkill /PID <PID_NUMBER> /F
```

Linux/Mac:
```bash
lsof -i :5000
kill -9 <PID>
```

### Problem: Template not found

**Error:**
```
jinja2.exceptions.TemplateNotFound: index.html
```

**Solution:**
1. Ensure `templates/` folder exists
2. Verify all HTML files are in `templates/` folder
3. Check file names match exactly (case-sensitive on Linux)

### Problem: Static files not loading

**Error:**
CSS not applying, images not showing

**Solution:**
1. Clear browser cache (Ctrl+F5)
2. Verify `static/css/style.css` exists
3. Check Flask is serving static files:
```python
app = Flask(__name__)  # Should find static folder automatically
```

### Problem: Secret key warning

**Error:**
```
WARNING: Using a default secret key
```

**Solution:**
Generate a secure secret key in `app.py`:
```python
import secrets
app.secret_key = secrets.token_hex(16)
# OR
app.secret_key = 'your-very-long-random-secret-key-here'
```

---

## Cloudant Database Issues

### Problem: Cloudant connection fails

**Error:**
```
CloudantException: 401 Unauthorized
```

**Solution:**
1. Verify credentials in `app.py`:
   - CLOUDANT_USERNAME
   - CLOUDANT_API_KEY (not the password!)
   - CLOUDANT_URL

2. Check URL format:
```python
# Correct
CLOUDANT_URL = "https://xxxxx-bluemix.cloudantnosqldb.appdomain.cloud"

# Wrong (missing https://)
CLOUDANT_URL = "xxxxx-bluemix.cloudantnosqldb.appdomain.cloud"
```

3. Regenerate credentials in IBM Cloud dashboard

### Problem: Database not created

**Error:**
```
Database connection error
```

**Solution:**
1. Uncomment initialization in `app.py`:
```python
if __name__ == '__main__':
    os.makedirs(UPLOAD_FOLDER, exist_ok=True)
    init_cloudant()  # Make sure this line is uncommented
    app.run(debug=True, host='0.0.0.0', port=5000)
```

2. Check console for error messages
3. Verify IBM Cloud service is active

### Problem: Too many requests

**Error:**
```
429 Too Many Requests
```

**Solution:**
- Lite plan has rate limits (20 reads/sec, 10 writes/sec)
- Add delays between requests
- Consider upgrading to Standard plan

### Problem: Can't connect to Cloudant

**Solution - Run without database:**
Comment out Cloudant initialization:
```python
# init_cloudant()  # Commented out - app works without DB
```

History page won't work, but predictions will still display.

---

## Prediction Errors

### Problem: Image upload fails

**Error:**
```
No file uploaded / No file selected
```

**Solution:**
1. Ensure file is selected in form
2. Check file size (<16MB)
3. Use supported formats: PNG, JPG, JPEG
4. Verify `uploads/` folder exists

### Problem: Invalid file type error

**Error:**
```
Invalid file type! Only PNG, JPG, and JPEG are allowed
```

**Solution:**
1. Convert image to supported format
2. Rename file with correct extension
3. Use image editing software to save as JPG/PNG

### Problem: Prediction returns None

**Error:**
```
Error making prediction. Please try again.
```

**Solution:**
1. Check model is loaded successfully
2. Verify image size and format
3. Look at console for detailed error
4. Try different image

### Problem: Low confidence predictions

**Issue:**
All predictions have confidence <50%

**Solution:**
1. Use clear, high-quality retinal images
2. Ensure proper lighting in images
3. Check if model is properly trained
4. Verify you're using preprocessed dataset for training

---

## Performance Issues

### Problem: Slow predictions (>10 seconds)

**Solution:**

1. **Check CPU usage:**
   - TensorFlow uses CPU by default
   - Normal prediction time: 2-5 seconds on CPU

2. **Use GPU (if available):**
```bash
# Install GPU version
pip install tensorflow-gpu==2.12.0
```

3. **Reduce image size** if preprocessing:
```python
IMG_HEIGHT = 224  # Don't increase, this is optimal for Xception
IMG_WIDTH = 224
```

### Problem: High memory usage

**Solution:**
1. Close other applications
2. Restart Flask application periodically
3. Reduce batch size in model training
4. Use production server (Gunicorn) instead of Flask development server

### Problem: Application crashes with large images

**Solution:**
1. Verify max file size in `app.py`:
```python
app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024  # 16MB
```

2. Add client-side validation
3. Compress images before upload

---

## Browser Issues

### Problem: Page not loading

**Solution:**
1. Clear browser cache (Ctrl+Shift+Delete)
2. Try different browser
3. Check if Flask server is running
4. Verify correct URL: http://localhost:5000

### Problem: CSS not applying

**Solution:**
1. Hard refresh: Ctrl+F5 (Windows) or Cmd+Shift+R (Mac)
2. Check browser console for 404 errors (F12 → Console)
3. Verify `static/css/style.css` exists
4. Check file permissions

### Problem: Image preview not showing

**Solution:**
1. Check JavaScript console for errors (F12)
2. Verify browser supports FileReader API
3. Update to latest browser version
4. Disable ad blockers/extensions temporarily

### Problem: Form submission not working

**Solution:**
1. Check browser JavaScript is enabled
2. Look for errors in console (F12)
3. Verify form action URL is correct
4. Try different browser

---

## Training Issues (Google Colab)

### Problem: Kaggle API not working

**Error:**
```
kaggle: command not found
```

**Solution:**
```python
!pip install kaggle
```

### Problem: Kaggle.json upload error

**Solution:**
1. Go to Kaggle.com → Account → API → Create New API Token
2. Download `kaggle.json`
3. Upload in Colab using file upload widget
4. Run setup commands:
```python
!mkdir -p ~/.kaggle
!cp kaggle.json ~/.kaggle/
!chmod 600 ~/.kaggle/kaggle.json
```

### Problem: Dataset download fails

**Solution:**
1. Verify kaggle.json is configured
2. Accept dataset terms on Kaggle website
3. Check dataset URL is correct
4. Try manual download and upload to Google Drive

### Problem: Out of memory during training

**Error:**
```
ResourceExhaustedError: OOM when allocating tensor
```

**Solution:**
1. Use GPU runtime in Colab (Runtime → Change runtime type → GPU)
2. Reduce batch size:
```python
BATCH_SIZE = 16  # Reduced from 32
```
3. Restart runtime and clear all outputs

### Problem: Colab disconnects during training

**Solution:**
1. Keep Colab tab active
2. Use Colab Pro for longer runtimes
3. Save checkpoints during training:
```python
checkpoint = ModelCheckpoint('model_checkpoint.h5', save_best_only=True)
```

---

## Common Error Messages

### ImportError: No module named 'tensorflow'

**Solution:**
```bash
pip install tensorflow==2.3.2
```

### ImportError: cannot import name 'url_quote' from 'werkzeug.urls'

**Solution:**
```bash
pip install werkzeug==2.3.0
```

### ValueError: Input 0 of layer "model" is incompatible with the layer

**Solution:**
Check input shape matches model requirements (224x224x3)

### RuntimeError: Session is already closed

**Solution:**
Restart Flask application

---

## Debugging Tips

### Enable Debug Mode

```python
# In app.py
app.run(debug=True)  # Shows detailed error messages
```

### Check Logs

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

### Print Debugging

```python
# Add print statements
print(f"Model loaded: {model is not None}")
print(f"Image path: {filepath}")
print(f"Prediction: {prediction}")
```

### Check Python Version

```bash
python --version  # Should be 3.8 or higher
```

### Check Installed Packages

```bash
pip list
pip show tensorflow
pip show flask
```

### Verify File Paths

```python
import os
print(f"Current directory: {os.getcwd()}")
print(f"Model exists: {os.path.exists('model/Updated-Xception-diabetic-retinopathy.h5')}")
```

---

## Getting Help

If problems persist:

1. **Check documentation:**
   - README.md
   - QUICKSTART.md
   - CLOUDANT_SETUP.md

2. **Search for error:**
   - Google the exact error message
   - Check Stack Overflow
   - Search GitHub issues

3. **Gather information:**
   - Python version
   - Operating system
   - Package versions
   - Full error message
   - Steps to reproduce

4. **Ask for help:**
   - Open GitHub issue with details above
   - Contact support
   - Post on relevant forums

---

## Useful Commands Reference

```bash
# Check versions
python --version
pip --version
pip list | grep tensorflow

# Reinstall packages
pip install -r requirements.txt --force-reinstall

# Clear pip cache
pip cache purge

# Update all packages
pip install --upgrade -r requirements.txt

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
venv\Scripts\activate     # Windows

# Run application
python app.py

# Check running processes
# Windows
netstat -ano | findstr :5000
# Linux/Mac
lsof -i :5000
```

---

## Prevention Tips

1. **Use virtual environment** to avoid conflicts
2. **Keep packages updated** regularly
3. **Test on small dataset** first
4. **Make incremental changes** and test
5. **Keep backups** of working configurations
6. **Document your modifications**
7. **Use version control** (Git)

---

**Need more help?** Open an issue with:
- Problem description
- Error message (full text)
- What you've tried
- System information

---

**Last Updated**: February 2026
