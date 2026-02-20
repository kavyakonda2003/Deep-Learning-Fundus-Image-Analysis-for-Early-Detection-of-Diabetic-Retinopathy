# Quick Start Guide

Get the Diabetic Retinopathy Detection System up and running in minutes!

## 🚀 Quick Setup (5 Minutes)

### Prerequisites Check
✅ Python 3.8+ installed  
✅ Anaconda or pip available  
✅ 2GB free disk space  
✅ Internet connection  

---

## Step 1: Install Dependencies (2 minutes)

Open Anaconda Prompt or Terminal as Administrator:

```bash
# Navigate to project folder
cd f:\SmartinterProject

# Install all requirements
pip install -r requirements.txt
```

**Expected output:**
```
Successfully installed Flask-2.3.0 tensorflow-2.3.2 keras-2.3.1 ...
```

---

## Step 2: Train or Download Model (30-60 minutes OR instant)

### Option A: Download Pre-trained Model (Recommended for Quick Start)
If a pre-trained model is available:
1. Download `Updated-Xception-diabetic-retinopathy.h5`
2. Place it in the `model/` folder

### Option B: Train Your Own Model
1. Open Google Colab: https://colab.research.google.com/
2. Upload `model/Xception_Diabetic_retinopathy.ipynb`
3. Follow notebook instructions:
   - Upload kaggle.json (get from Kaggle Account → API)
   - Run all cells (takes 30-60 minutes on Colab GPU)
   - Download the generated `.h5` file
4. Place downloaded model in `model/` folder

---

## Step 3: Configure Application (1 minute)

### Basic Configuration (No Database)
The app works without Cloudant! Just run it:

```bash
python app.py
```

### With Cloudant Database (Optional)
1. Follow [CLOUDANT_SETUP.md](CLOUDANT_SETUP.md) to get credentials
2. Edit `app.py` lines 28-31:
```python
CLOUDANT_USERNAME = "your-username-here"
CLOUDANT_API_KEY = "your-api-key-here"
CLOUDANT_URL = "your-url-here"
```
3. Uncomment line in `app.py` (around line 268):
```python
init_cloudant()  # Remove the # at the start
```

---

## Step 4: Run Application (30 seconds)

```bash
python app.py
```

**Expected output:**
```
Model loaded successfully!
 * Running on http://0.0.0.0:5000
 * Running on http://127.0.0.1:5000
```

---

## Step 5: Access Application

Open your browser:
- **Local URL**: http://localhost:5000
- **Register**: http://localhost:5000/register
- **Login**: http://localhost:5000/login

### First Time Use
1. **Register** a new account (any username/password for demo)
2. **Login** with your credentials
3. **Upload** a retinal image
4. **View** prediction results!

---

## 📁 File Checklist

Before running, ensure these files exist:

```
SmartinterProject/
├── ✅ app.py
├── ✅ requirements.txt
├── ✅ README.md
├── ✅ model/
│   └── ⚠️ Updated-Xception-diabetic-retinopathy.h5  (REQUIRED)
├── ✅ static/
│   └── ✅ css/
│       └── ✅ style.css
├── ✅ templates/
│   ├── ✅ index.html
│   ├── ✅ login.html
│   ├── ✅ register.html
│   ├── ✅ prediction.html
│   ├── ✅ history.html
│   └── ✅ about.html
└── ✅ uploads/  (created automatically)
```

**⚠️ Critical**: `Updated-Xception-diabetic-retinopathy.h5` must exist in `model/` folder!

---

## 🎯 Test Dataset

Need sample retinal images for testing?

1. **Kaggle Dataset**: https://www.kaggle.com/datasets/arbethi/diabetic-retinopathy-level-detection
2. **Download** a few test images
3. **Upload** through the application

---

## 🐛 Quick Troubleshooting

### ❌ "Model file not found"
**Solution**: Place `Updated-Xception-diabetic-retinopathy.h5` in `model/` folder

### ❌ "Module not found" errors
**Solution**: Reinstall requirements
```bash
pip install -r requirements.txt --force-reinstall
```

### ❌ TensorFlow version conflicts
**Solution**: Use compatible versions
```bash
pip install tensorflow==2.3.2 keras==2.3.1 numpy==1.19.5
```

OR upgrade to newer versions:
```bash
pip install tensorflow==2.12.0 keras==2.12.0 numpy==1.23.5
```

### ❌ "Port 5000 already in use"
**Solution**: Change port in `app.py`
```python
app.run(debug=True, host='0.0.0.0', port=5001)  # Changed to 5001
```

### ❌ Cloudant connection errors
**Solution**: App works without Cloudant! Just comment out:
```python
# init_cloudant()
```

---

## 📱 Usage Flow

```
1. Register Account
   ↓
2. Login
   ↓
3. Upload Retinal Image
   ↓
4. View AI Prediction
   ↓
5. Check History (if Cloudant enabled)
```

---

## 🎓 Understanding Results

### Classification Levels

| Level | Meaning | Action |
|-------|---------|--------|
| **No DR** | No diabetic retinopathy | Regular check-ups |
| **Mild** | Early signs detected | Monitor regularly |
| **Moderate** | Noticeable symptoms | Consult doctor |
| **Severe** | Advanced stage | Medical attention needed |
| **Proliferative** | Critical stage | Urgent medical care |

### Confidence Score
- **>90%**: Very confident prediction
- **70-90%**: Good confidence
- **50-70%**: Moderate confidence
- **<50%**: Low confidence (consider retesting)

---

## 🔧 Development Mode

### Enable Debug Mode (Already enabled by default)
```python
app.run(debug=True, ...)  # Auto-reloads on code changes
```

### Disable Debug Mode (For Production)
```python
app.run(debug=False, ...)
```

---

## 📊 Expected Performance

### Model Metrics
- **Training Accuracy**: ~85-95% (depends on dataset)
- **Validation Accuracy**: ~80-90%
- **Test Accuracy**: ~75-88%
- **Inference Time**: 1-3 seconds per image

### System Requirements
- **RAM**: 4GB minimum, 8GB recommended
- **Storage**: 2GB free space
- **GPU**: Optional (CPU works fine for inference)
- **Browser**: Chrome, Firefox, Edge, Safari (latest versions)

---

## 🎉 Success Indicators

✅ No errors in console  
✅ "Model loaded successfully!" message  
✅ Web page loads at http://localhost:5000  
✅ Can upload and get predictions  
✅ Results show classification and confidence  

---

## 📚 Next Steps

1. ✅ **Read README.md** for detailed documentation
2. ✅ **Review CLOUDANT_SETUP.md** for database integration
3. ✅ **Explore code** in `app.py` and templates
4. ✅ **Customize** UI by editing CSS and HTML
5. ✅ **Add features** like user authentication with database

---

## 🆘 Need Help?

1. **Check**: [README.md](README.md) - Comprehensive guide
2. **Check**: [CLOUDANT_SETUP.md](CLOUDANT_SETUP.md) - Database setup
3. **Search**: Error message in Google
4. **Ask**: Open an issue on GitHub
5. **Email**: [Support contact]

---

## 🔗 Useful Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run application
python app.py

# Check Python version
python --version

# Check TensorFlow version
python -c "import tensorflow as tf; print(tf.__version__)"

# Check installed packages
pip list

# Update pip
python -m pip install --upgrade pip
```

---

## ⚡ Pro Tips

1. **Use Colab GPU** for faster training (Runtime → Change runtime type → GPU)
2. **Keep model file safe** - it's large and takes time to train
3. **Test with various images** to understand model behavior
4. **Start without Cloudant** - add database later
5. **Use virtual environment** to avoid package conflicts

---

**You're all set! 🎉**

Run `python app.py` and start detecting diabetic retinopathy!

---

**Documentation Version**: 1.0  
**Last Updated**: February 2026
