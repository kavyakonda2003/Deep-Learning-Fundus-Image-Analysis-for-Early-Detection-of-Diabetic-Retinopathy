# Diabetic Retinopathy Detection System

An AI-powered web application for early detection of diabetic retinopathy using deep learning. The system uses the Xception architecture with transfer learning to classify retinal images into five categories: No DR, Mild, Moderate, Severe, and Proliferative DR.

![Python](https://img.shields.io/badge/Python-3.8-blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.3.2-orange)
![Flask](https://img.shields.io/badge/Flask-2.3.0-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

## 📋 Table of Contents
- [Features](#features)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Model Training](#model-training)
- [Cloudant Database Setup](#cloudant-database-setup)
- [Running the Application](#running-the-application)
- [Usage](#usage)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

- **AI-Powered Detection**: Uses Xception deep learning model for accurate diabetic retinopathy classification
- **Five-Class Classification**: Detects No DR, Mild, Moderate, Severe, and Proliferative DR
- **User Authentication**: Secure login and registration system
- **Image Upload**: Easy-to-use interface for uploading retinal images
- **Real-time Analysis**: Instant prediction results with confidence scores
- **History Tracking**: View all previous predictions with IBM Cloudant integration
- **Responsive Design**: Works seamlessly on desktop and mobile devices
- **Professional UI/UX**: Modern, intuitive interface built with HTML5, CSS3, and JavaScript

## 📁 Project Structure

```
SmartinterProject/
│
├── model/
│   ├── Xception_Diabetic_retinopathy.ipynb
│   └── Updated-Xception-diabetic-retinopathy.h5  (generated after training)
│
├── static/
│   ├── css/
│   │   └── style.css
│   └── images/
│
├── templates/
│   ├── index.html
│   ├── login.html
│   ├── register.html
│   ├── prediction.html
│   ├── history.html
│   └── about.html
│
├── uploads/  (created automatically for uploaded images)
│
├── app.py
├── requirements.txt
└── README.md
```

## 🔧 Prerequisites

Before starting, ensure you have the following installed:

### Software Requirements
- **Python 3.8 or higher**
- **Anaconda Navigator** (recommended) or **pip**
- **PyCharm** or **Spyder** IDE (optional but recommended)

### Prior Knowledge
- Basic understanding of Python programming
- Familiarity with deep learning concepts (CNN, Transfer Learning)
- Basic knowledge of Flask web framework
- Understanding of HTML/CSS (for customization)

### Learning Resources
- **CNN Basics**: [Towards Data Science - CNN](https://towardsdatascience.com/basics-of-the-classic-cnn-a3dce1225add)
- **Xception Architecture**: [PyImageSearch - Xception](https://pyimagesearch.com/2017/03/20/imagenet-vggnet-resnet-inception-xception-keras/)
- **Flask Framework**: [Flask Tutorial](https://www.youtube.com/watch?v=lj4I_CvBnt0)

## 💻 Installation

### Step 1: Install Anaconda Navigator

**PyCharm Setup**: [Watch Tutorial](https://youtu.be/1ra4zH2G4o0)
**Spyder Setup**: [Watch Tutorial](https://youtu.be/5mDYijMfSzs)

### Step 2: Clone or Download the Project

```bash
git clone <repository-url>
cd SmartinterProject
```

### Step 3: Create Virtual Environment (Optional but Recommended)

```bash
# Using Anaconda
conda create -n diabetic_retinopathy python=3.8
conda activate diabetic_retinopathy

# OR using venv
python -m venv venv
# On Windows
venv\Scripts\activate
# On Linux/Mac
source venv/bin/activate
```

### Step 4: Install Required Packages

Open Anaconda Prompt as Administrator and run:

```bash
pip install numpy
pip install pandas
pip install tensorflow==2.3.2
pip install keras==2.3.1
pip install Flask
pip install Pillow
pip install cloudant
pip install python-dotenv
```

**OR** install all at once using requirements.txt:

```bash
pip install -r requirements.txt
```

## 🧠 Model Training

### Dataset Preparation

1. **Download Dataset**: [Kaggle - Diabetic Retinopathy Dataset](https://www.kaggle.com/datasets/arbethi/diabetic-retinopathy-level-detection?select=preprocessed+dataset)

2. **Google Colab Setup**:
   - Upload the `Xception_Diabetic_retinopathy.ipynb` notebook to Google Colab
   - Follow the instructions in the notebook to:
     - Set up Kaggle API credentials
     - Download and unzip the dataset
     - Train the model

### Training Process

The notebook includes the following steps:

1. **Data Collection**: Clone Kaggle dataset directly in Colab
2. **Data Preprocessing**: Configure ImageDataGenerator for augmentation
3. **Model Building**: 
   - Load pre-trained Xception model (ImageNet weights)
   - Add custom dense layers
   - Compile the model
4. **Training**: 
   - Phase 1: Train with frozen base layers
   - Phase 2: Fine-tuning with unfrozen layers
5. **Evaluation**: Test on validation set
6. **Save Model**: Export as `.h5` file

### Download Trained Model

After training completes:
1. Download `Updated-Xception-diabetic-retinopathy.h5` from Colab
2. Place it in the `model/` folder of your project

## 🗄️ Cloudant Database Setup

### Create IBM Cloudant Instance

1. **Register**: Sign up at [IBM Cloud](https://cloud.ibm.com/)
2. **Create Service**:
   - Navigate to Catalog → Databases → Cloudant
   - Choose a plan (Lite plan is free)
   - Create the service instance

3. **Get Credentials**:
   - Go to Service Credentials
   - Click "New Credential"
   - Copy the credentials

4. **Configure app.py**:
   ```python
   CLOUDANT_USERNAME = "your-cloudant-username"
   CLOUDANT_API_KEY = "your-cloudant-api-key"
   CLOUDANT_URL = "your-cloudant-url"
   ```

5. **Launch Cloudant**:
   - Open Cloudant Dashboard
   - Database will be created automatically on first run

## 🚀 Running the Application

### Step 1: Ensure Model File Exists

Make sure `Updated-Xception-diabetic-retinopathy.h5` is in the `model/` folder.

### Step 2: Configure Cloudant (Optional)

If using Cloudant, update credentials in `app.py` and uncomment:
```python
init_cloudant()
```

### Step 3: Run Flask Application

```bash
python app.py
```

The application will start at: `http://localhost:5000`

### Step 4: Access the Application

Open your web browser and navigate to:
- **Home**: `http://localhost:5000`
- **Login**: `http://localhost:5000/login`
- **Register**: `http://localhost:5000/register`

## 📖 Usage

### 1. Register & Login
- Create a new account using the registration page
- Login with your credentials

### 2. Upload Retinal Image
- Navigate to the home page
- Click or drag-and-drop a retinal image
- Supported formats: PNG, JPG, JPEG
- Maximum file size: 16MB

### 3. View Results
- System analyzes the image using AI
- Displays diagnosis (No DR, Mild, Moderate, Severe, or Proliferative DR)
- Shows confidence percentage
- Provides severity information and recommendations

### 4. Check History
- View all previous predictions
- See timestamps, diagnoses, and confidence scores
- Track your scanning history

## 🛠️ Technology Stack

### Backend
- **Flask 2.3.0**: Web framework
- **TensorFlow 2.3.2**: Deep learning framework
- **Keras 2.3.1**: Neural network API
- **NumPy & Pandas**: Data processing

### Frontend
- **HTML5**: Structure
- **CSS3**: Styling with custom design
- **JavaScript**: Interactivity and image preview
- **Font Awesome**: Icons

### Database
- **IBM Cloudant**: NoSQL cloud database for storing predictions

### AI Model
- **Xception**: Pre-trained on ImageNet
- **Transfer Learning**: Custom layers for diabetic retinopathy classification
- **Input Size**: 224x224 pixels
- **Classes**: 5 (No DR, Mild, Moderate, Severe, Proliferative DR)

## 🏗️ Architecture

```
User → Flask App → Xception Model → Prediction
                ↓
           Cloudant DB (Store Results)
```

### Model Architecture
1. **Base Model**: Xception (pre-trained on ImageNet)
2. **Global Average Pooling**: Dimensionality reduction
3. **Dense Layer**: 512 neurons with ReLU activation
4. **Dropout**: 0.5 for regularization
5. **Dense Layer**: 256 neurons with ReLU activation
6. **Dropout**: 0.3 for regularization
7. **Output Layer**: 5 neurons with Softmax activation

### Data Flow
1. User uploads retinal image
2. Image is preprocessed (resized to 224x224, normalized)
3. Model predicts class probabilities
4. Highest probability determines the diagnosis
5. Results displayed to user
6. Prediction saved to Cloudant database

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/improvement`)
3. Make your changes
4. Commit your changes (`git commit -am 'Add new feature'`)
5. Push to the branch (`git push origin feature/improvement`)
6. Create a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## ⚠️ Disclaimer

**Important**: This system is designed as a screening tool to assist in early detection of diabetic retinopathy. It should **NOT** be used as a substitute for professional medical diagnosis or treatment. 

- The AI model's predictions are based solely on image analysis
- Results may not account for all clinical factors
- Always consult qualified healthcare professionals and ophthalmologists
- Regular eye examinations by medical professionals remain essential

## 📞 Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Contact: [your-email@example.com]

## 🙏 Acknowledgments

- Dataset: [Kaggle - Diabetic Retinopathy Dataset](https://www.kaggle.com/datasets/arbethi/diabetic-retinopathy-level-detection)
- Xception Architecture: François Chollet
- Transfer Learning Resources: PyImageSearch, Towards Data Science
- Flask Framework: Pallets Projects
- IBM Cloudant: IBM Cloud Services

## 📚 References

1. **CNN Fundamentals**: https://towardsdatascience.com/basics-of-the-classic-cnn-a3dce1225add
2. **VGG16 Architecture**: https://medium.com/@mygreatlearning/what-is-vgg16-introduction-to-vgg16-f2d63849f615
3. **ResNet-50**: https://towardsdatascience.com/understanding-and-coding-a-resnet-in-keras-446d7ff84d33
4. **Inception-V3**: https://iq.opengenus.org/inception-v3-model-architecture/
5. **Xception**: https://pyimagesearch.com/2017/03/20/imagenet-vggnet-resnet-inception-xception-keras/
6. **Flask Documentation**: https://flask.palletsprojects.com/
7. **TensorFlow Documentation**: https://www.tensorflow.org/

---

**Built with ❤️ for better healthcare through AI**

**Version**: 1.0.0  
**Last Updated**: February 2026
