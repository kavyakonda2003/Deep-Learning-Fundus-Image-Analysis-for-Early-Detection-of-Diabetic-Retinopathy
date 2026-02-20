# IBM Cloudant Database Setup Guide

This guide walks you through setting up IBM Cloudant for the Diabetic Retinopathy Detection System.

## Table of Contents
1. [Create IBM Cloud Account](#create-ibm-cloud-account)
2. [Create Cloudant Service Instance](#create-cloudant-service-instance)
3. [Get Service Credentials](#get-service-credentials)
4. [Configure Application](#configure-application)
5. [Launch Cloudant Dashboard](#launch-cloudant-dashboard)
6. [Verify Connection](#verify-connection)

---

## 1. Create IBM Cloud Account

### Step 1: Register
1. Go to [IBM Cloud](https://cloud.ibm.com/)
2. Click **"Create an account"**
3. Fill in your details:
   - Email address
   - Password
   - First and Last name
   - Country/Region
4. Verify your email address
5. Complete the registration process

### Free Tier Benefits
- **No credit card required** for Lite plan
- **1 GB storage** free
- **20 lookups/sec** and **10 writes/sec**
- Perfect for development and testing

---

## 2. Create Cloudant Service Instance

### Step 1: Navigate to Catalog
1. Login to [IBM Cloud Console](https://cloud.ibm.com/)
2. Click **"Catalog"** in the top menu
3. Search for **"Cloudant"**
4. Click on **"Cloudant"** service

### Step 2: Configure Service
1. **Select a region**: Choose closest to your location
2. **Choose pricing plan**: Select **"Lite"** (Free)
3. **Service name**: `diabetic-retinopathy-cloudant` (or your choice)
4. **Resource group**: Default
5. **Authentication method**: Select **"IAM and legacy credentials"**
6. Click **"Create"**

### Step 3: Wait for Provisioning
- Service will be created in a few seconds
- You'll see a success message

---

## 3. Get Service Credentials

### Step 1: Access Service Credentials
1. From IBM Cloud Dashboard, go to **"Resource list"**
2. Under **"Services and software"**, find your Cloudant instance
3. Click on the service name
4. In the left sidebar, click **"Service credentials"**

### Step 2: Create New Credentials
1. Click **"New credential"** button
2. **Name**: `diabetic-retinopathy-credentials`
3. **Role**: Select **"Manager"** (full access)
4. Click **"Add"**

### Step 3: View and Copy Credentials
1. Click **"View credentials"** (dropdown arrow)
2. You'll see JSON with credentials like:

```json
{
  "apikey": "xxxxx-xxxxxxxxxxxxxxxxxxxxxxx",
  "host": "xxxxx-xxxxx-bluemix.cloudant.com",
  "iam_apikey_description": "Auto-generated for key xxxx",
  "iam_apikey_name": "diabetic-retinopathy-credentials",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/xxxxx",
  "url": "https://xxxxx-xxxxx-bluemix.cloudantnosqldb.appdomain.cloud",
  "username": "xxxxx-xxxxx-bluemix"
}
```

3. Copy the following values:
   - **username**: Your Cloudant username
   - **apikey**: Your IAM API key
   - **url**: Your Cloudant URL

---

## 4. Configure Application

### Method 1: Direct Configuration (Quick Start)

Open `app.py` and update these lines:

```python
# IBM Cloudant Configuration
CLOUDANT_USERNAME = "your-cloudant-username"  # Replace with your username
CLOUDANT_API_KEY = "your-cloudant-api-key"    # Replace with your apikey
CLOUDANT_URL = "your-cloudant-url"            # Replace with your url
```

**Example:**
```python
CLOUDANT_USERNAME = "12345678-90ab-cdef-1234-567890abcdef-bluemix"
CLOUDANT_API_KEY = "abcdefghijklmnopqrstuvwxyz123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
CLOUDANT_URL = "https://12345678-90ab-cdef-1234-567890abcdef-bluemix.cloudantnosqldb.appdomain.cloud"
```

### Method 2: Environment Variables (Recommended for Production)

1. **Create `.env` file** in project root:

```env
CLOUDANT_USERNAME=your-cloudant-username
CLOUDANT_API_KEY=your-cloudant-api-key
CLOUDANT_URL=your-cloudant-url
SECRET_KEY=your-secret-key-here
```

2. **Install python-dotenv**:
```bash
pip install python-dotenv
```

3. **Update `app.py`**:
```python
from dotenv import load_dotenv
import os

load_dotenv()

CLOUDANT_USERNAME = os.getenv('CLOUDANT_USERNAME')
CLOUDANT_API_KEY = os.getenv('CLOUDANT_API_KEY')
CLOUDANT_URL = os.getenv('CLOUDANT_URL')
app.secret_key = os.getenv('SECRET_KEY')
```

### Enable Cloudant in Application

In `app.py`, uncomment this line at the bottom:

```python
if __name__ == '__main__':
    os.makedirs(UPLOAD_FOLDER, exist_ok=True)
    
    # Uncomment the following line after adding your Cloudant credentials
    init_cloudant()  # <-- UNCOMMENT THIS LINE
    
    app.run(debug=True, host='0.0.0.0', port=5000)
```

---

## 5. Launch Cloudant Dashboard

### Access Dashboard
1. From your Cloudant service page in IBM Cloud
2. Click **"Launch Dashboard"** button
3. Cloudant Dashboard opens in a new tab

### Dashboard Overview
- **Databases**: View all databases
- **Replication**: Set up database replication
- **Monitoring**: View metrics and statistics
- **Account**: Manage account settings
- **Documentation**: Access Cloudant docs

---

## 6. Verify Connection

### Step 1: Run Application
```bash
python app.py
```

### Step 2: Check Console Output
You should see:
```
Database 'diabetic_retinopathy_db' created successfully
Model loaded successfully!
 * Running on http://0.0.0.0:5000
```

OR if database already exists:
```
Connected to existing database 'diabetic_retinopathy_db'
```

### Step 3: Test Database Connection

1. **Register** a new user
2. **Login** and **upload** an image
3. **Check Cloudant Dashboard**:
   - Refresh the dashboard
   - You should see `diabetic_retinopathy_db` listed
   - Click on the database name
   - You'll see documents with prediction data

### Step 4: View Prediction Documents

Example document structure:
```json
{
  "_id": "xxxxxxxxxxxxx",
  "_rev": "1-xxxxxxxxxxxxx",
  "username": "john_doe",
  "image_name": "20260208_143022_retinal_image.jpg",
  "prediction": "Moderate",
  "confidence": 87.65,
  "timestamp": "2026-02-08T14:30:22.123456",
  "type": "prediction"
}
```

---

## Database Schema

### Collections/Document Types

#### 1. Prediction Documents
```json
{
  "username": "string",
  "image_name": "string",
  "prediction": "string",
  "confidence": "float",
  "timestamp": "ISO8601 datetime",
  "type": "prediction"
}
```

#### 2. User Documents (Optional - Future Enhancement)
```json
{
  "username": "string",
  "email": "string",
  "password_hash": "string",
  "created_at": "ISO8601 datetime",
  "type": "user"
}
```

---

## Query Examples

### Using Python in Application

```python
# Get all predictions for a user
selector = {
    'username': {'$eq': 'john_doe'},
    'type': {'$eq': 'prediction'}
}
results = database.get_query_result(selector, sort=[{'timestamp': 'desc'}])
predictions = [doc for doc in results]

# Count total predictions
selector = {'type': {'$eq': 'prediction'}}
count = len([doc for doc in database.get_query_result(selector)])

# Get predictions by severity
selector = {
    'prediction': {'$eq': 'Severe'},
    'type': {'$eq': 'prediction'}
}
severe_cases = [doc for doc in database.get_query_result(selector)]
```

---

## Troubleshooting

### Error: "CloudantException: 401 Unauthorized"
**Solution**: Check your credentials
- Verify `CLOUDANT_USERNAME` is correct
- Verify `CLOUDANT_API_KEY` (not the password)
- Ensure `CLOUDANT_URL` includes `https://`

### Error: "Connection timeout"
**Solution**: Check network and URL
- Verify you have internet connection
- Check if Cloudant URL is correct
- Try accessing URL in browser (should show authentication prompt)

### Error: "Database not found"
**Solution**: Run initialization
- Make sure `init_cloudant()` is called
- Check console for error messages
- Database should be created automatically

### Error: "Too many requests"
**Solution**: Rate limiting
- Lite plan has rate limits (20 reads/sec, 10 writes/sec)
- Add delays between requests if testing extensively
- Consider upgrading plan for production

---

## Best Practices

### Security
1. **Never commit credentials** to version control
2. Use **environment variables** for sensitive data
3. Store `.env` file locally only
4. Add `.env` to `.gitignore`

### Performance
1. **Index frequently queried fields**
2. Use **batch operations** when possible
3. **Cache results** when appropriate
4. **Limit query results** with pagination

### Data Management
1. **Regular backups** (Cloudant provides automatic backups)
2. **Monitor storage usage** in dashboard
3. **Clean old data** periodically if needed
4. **Archive historical data** if storage limit reached

---

## Additional Resources

- **Cloudant Documentation**: https://cloud.ibm.com/docs/Cloudant
- **Python Cloudant Library**: https://python-cloudant.readthedocs.io/
- **IBM Cloud Support**: https://cloud.ibm.com/docs
- **Cloudant Blog**: https://blog.cloudant.com/

---

## Support

For issues with Cloudant setup:
1. Check [IBM Cloud Status](https://cloud.ibm.com/status)
2. Review [Cloudant Documentation](https://cloud.ibm.com/docs/Cloudant)
3. Contact IBM Cloud Support
4. Open an issue on the project GitHub repository

---

**Last Updated**: February 2026
