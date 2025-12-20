# 🌿 CropShield - Plant Disease Detection System

![CropShield](https://img.shields.io/badge/Plant%20Disease-Detection-green)
![FastAPI](https://img.shields.io/badge/FastAPI-0.110.0-009688?logo=fastapi)
![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.17.1-FF6F00?logo=tensorflow)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0-3178C6?logo=typescript)

A comprehensive AI-powered plant disease detection system that helps farmers and agricultural professionals identify crop diseases quickly and accurately. CropShield uses deep learning with TensorFlow to analyze plant images and provide instant disease diagnosis with confidence scores.

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Usage](#-usage)
- [API Documentation](#-api-documentation)
- [Supported Crops & Diseases](#-supported-crops--diseases)
- [Model Information](#-model-information)
- [Contributing](#-contributing)
- [License](#-license)

## ✨ Features

- **🔍 Real-time Disease Detection**: Upload plant images and get instant disease diagnosis
- **🎯 High Accuracy**: Trained on 38 different plant disease classes
- **📊 Confidence Scoring**: Get confidence percentages for predictions
- **🏆 Top-3 Predictions**: View the most likely diseases with their probabilities
- **🌓 Dark/Light Mode**: User-friendly interface with theme switching
- **🔒 User Authentication**: Secure user registration and login system
- **📱 Responsive Design**: Works seamlessly on desktop and mobile devices
- **📈 Prediction History**: Track and review past predictions (MongoDB integration)
- **🚀 Fast API**: High-performance backend with FastAPI
- **🎨 Modern UI**: Beautiful, animated interface built with Next.js and Tailwind CSS

## 🛠️ Tech Stack

### Backend (Machine Learning API)

- **Framework**: FastAPI 0.110.0
- **ML Library**: TensorFlow 2.17.1
- **Image Processing**: Pillow 10.2.0
- **Server**: Uvicorn/Gunicorn
- **Model**: Custom trained CNN on plant disease dataset
- **Image Size**: 224x224 pixels
- **Classes**: 38 different plant disease categories

### Frontend (Web Application)

- **Framework**: Next.js 15
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui components
- **State Management**: React Hooks
- **Animations**: Framer Motion
- **Theme**: next-themes (Dark/Light mode)
- **Database Models**: MongoDB with Mongoose

### Database

- **MongoDB**: User authentication and prediction history storage
- **Models**: User and Prediction schemas

## 📁 Project Structure

```
CropShield/
├── app/                          # Backend ML API
│   ├── main.py                   # FastAPI application with prediction endpoints
│   ├── model.h5                  # Trained TensorFlow model
│   ├── requirements.txt          # Python dependencies
│   └── start.sh                  # Production startup script
│
└── web/                          # Frontend Next.js application
    ├── components/               # React components
    │   ├── ui/                   # shadcn/ui component library
    │   ├── Alert.tsx            # Alert notification component
    │   ├── Background.tsx       # Animated background component
    │   ├── DarkModeToggle.tsx  # Theme switcher component
    │   ├── FallingLeaves.tsx   # Decorative animation
    │   ├── Footer.tsx          # Footer component
    │   ├── Navbar.tsx          # Navigation bar with auth
    │   └── ThemeProvider.tsx   # Theme context provider
    │
    ├── models/                   # TypeScript data models
    │   ├── Prediction.ts        # Prediction schema for MongoDB
    │   └── User.ts             # User schema for MongoDB
    │
    ├── next.config.mjs          # Next.js configuration
    ├── tailwind.config.ts       # Tailwind CSS configuration
    └── tsconfig.json            # TypeScript configuration
```

## 🚀 Installation

### Prerequisites

- Python 3.8+
- Node.js 18+
- npm or yarn
- MongoDB (for user authentication and history)

### Backend Setup

1. **Navigate to the backend directory**:

   ```bash
   cd app
   ```

2. **Create a virtual environment** (recommended):

   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install Python dependencies**:

   ```bash
   pip install -r requirements.txt
   ```

4. **Ensure model file exists**:

   - Place your trained `model.h5` file in the `app/` directory
   - Model should be trained on 224x224 images with 38 classes

5. **Run the backend server**:

   ```bash
   # Development mode
   python main.py

   # Or using uvicorn directly
   uvicorn main:app --host 0.0.0.0 --port 8000 --reload

   # Production mode
   gunicorn main:app --workers 1 --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
   ```

   The API will be available at `http://localhost:8000`

### Frontend Setup

1. **Navigate to the frontend directory**:

   ```bash
   cd web
   ```

2. **Install dependencies**:

   ```bash
   npm install
   # or
   yarn install
   ```

3. **Set up environment variables**:
   Create a `.env.local` file in the `web/` directory:

   ```env
   MONGODB_URI=your_mongodb_connection_string
   NEXT_PUBLIC_API_URL=http://localhost:8000
   ```

4. **Run the development server**:

   ```bash
   npm run dev
   # or
   yarn dev
   ```

   The web app will be available at `http://localhost:3000`

## 💻 Usage

### Using the Web Interface

1. **Access the application** at `http://localhost:3000`
2. **Register/Login** to your account
3. **Navigate to the prediction page**
4. **Upload a plant image** (JPEG, PNG, etc.)
5. **View results** including:
   - Predicted disease class
   - Confidence score
   - Top 3 possible diseases
6. **Review history** of past predictions

### Using the API Directly

**Health Check**:

```bash
curl http://localhost:8000/health
```

**Make a Prediction**:

```bash
curl -X POST "http://localhost:8000/predict" \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@/path/to/plant/image.jpg"
```

**Get Model Info**:

```bash
curl http://localhost:8000/debug-info
```

## 📚 API Documentation

### Endpoints

#### `GET /`

Welcome endpoint

- **Response**: `{"message": "Welcome to Plant Disease Detection API"}`

#### `GET /health`

Health check endpoint

- **Response**:
  ```json
  {
    "status": "healthy",
    "message": "Service is running"
  }
  ```

#### `POST /predict`

Main prediction endpoint

- **Input**: Multipart form data with image file
- **Response**:
  ```json
  {
    "success": true,
    "prediction": {
      "class_index": 28,
      "class_name": "Tomato___Bacterial_spot",
      "confidence": 0.9876
    },
    "top_predictions": [
      { "class": "Tomato___Bacterial_spot", "confidence": 0.9876 },
      { "class": "Tomato___Early_blight", "confidence": 0.0089 },
      { "class": "Tomato___Leaf_Mold", "confidence": 0.0023 }
    ],
    "console_output": "Predicted Class Index: 28\n..."
  }
  ```

#### `GET /debug-info`

Get model and configuration information

- **Response**:
  ```json
  {
    "model_loaded": true,
    "model_path": "model.h5",
    "image_size": 224,
    "class_count": 38,
    "sample_class_names": {...}
  }
  ```

### Interactive API Documentation

When the backend is running, access the auto-generated API documentation:

- **Swagger UI**: `http://localhost:8000/docs`
- **ReDoc**: `http://localhost:8000/redoc`

## 🌾 Supported Crops & Diseases

CropShield can detect diseases across **38 classes** covering multiple crop types:

### Apple (4 classes)

- Apple Scab
- Black Rot
- Cedar Apple Rust
- Healthy

### Blueberry (1 class)

- Healthy

### Cherry (2 classes)

- Powdery Mildew
- Healthy

### Corn/Maize (4 classes)

- Cercospora Leaf Spot / Gray Leaf Spot
- Common Rust
- Northern Leaf Blight
- Healthy

### Grape (4 classes)

- Black Rot
- Esca (Black Measles)
- Leaf Blight (Isariopsis Leaf Spot)
- Healthy

### Orange (1 class)

- Huanglongbing (Citrus Greening)

### Peach (2 classes)

- Bacterial Spot
- Healthy

### Pepper/Bell (2 classes)

- Bacterial Spot
- Healthy

### Potato (3 classes)

- Early Blight
- Late Blight
- Healthy

### Raspberry (1 class)

- Healthy

### Soybean (1 class)

- Healthy

### Squash (1 class)

- Powdery Mildew

### Strawberry (2 classes)

- Leaf Scorch
- Healthy

### Tomato (10 classes)

- Bacterial Spot
- Early Blight
- Late Blight
- Leaf Mold
- Septoria Leaf Spot
- Spider Mites (Two-spotted Spider Mite)
- Target Spot
- Tomato Yellow Leaf Curl Virus
- Tomato Mosaic Virus
- Healthy

## 🧠 Model Information

### Training Details

- **Architecture**: Convolutional Neural Network (CNN)
- **Input Size**: 224x224x3 RGB images
- **Output**: 38 classes (softmax activation)
- **Preprocessing**:
  - Rescaling: 1/255.0
  - Rotation: ±15 degrees
  - Horizontal flip: Yes
  - Brightness range: 0.8-1.2
- **Reproducibility**: Fixed random seed (42) for deterministic results

### Model Features

- **Transfer Learning**: Built on state-of-the-art CNN architecture
- **Data Augmentation**: Robust training with augmented data
- **High Accuracy**: Trained on extensive plant disease dataset
- **Fast Inference**: Optimized for real-time predictions
- **Float32 Precision**: Standard TensorFlow float precision

### Image Requirements

- **Format**: JPEG, PNG, or other common image formats
- **Mode**: Automatically converts to RGB
- **Size**: Any size (automatically resized to 224x224)
- **Quality**: Higher quality images yield better results

## 🔒 Security Features

- **CORS**: Configured for cross-origin requests
- **Input Validation**: File type verification
- **Error Handling**: Comprehensive exception handling
- **User Authentication**: Secure password storage with MongoDB
- **Session Management**: JWT or session-based authentication

## 🎨 Frontend Features

### Components

- **Alert System**: Success/danger/info notifications
- **Theme Support**: Dark and light mode with smooth transitions
- **Responsive Design**: Mobile-first approach
- **Animations**: Falling leaves and smooth transitions
- **Modern UI**: shadcn/ui component library

### User Experience

- Clean, intuitive interface
- Real-time prediction results
- Image preview before upload
- Confidence visualization
- Prediction history tracking

## 🚢 Deployment

### Backend Deployment

**Using Gunicorn** (Production):

```bash
gunicorn main:app --workers 1 --worker-class uvicorn.workers.UvicornWorker --bind 0.0.0.0:$PORT
```

**Docker** (Optional):

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["gunicorn", "main:app", "--workers", "1", "--worker-class", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:8000"]
```

### Frontend Deployment

**Build for Production**:

```bash
npm run build
npm start
```

**Deploy to Vercel** (Recommended):

```bash
vercel deploy
```

## 🐛 Troubleshooting

### Common Issues

1. **Model Not Loading**:

   - Ensure `model.h5` exists in the `app/` directory
   - Check TensorFlow version compatibility

2. **Import Errors**:

   - Verify all dependencies are installed: `pip install -r requirements.txt`

3. **Port Already in Use**:

   - Change port in startup command: `--port 8001`

4. **CORS Errors**:

   - Update `allow_origins` in `main.py` to include your frontend URL

5. **MongoDB Connection**:
   - Verify MongoDB URI in `.env.local`
   - Ensure MongoDB service is running

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add some AmazingFeature'`
4. Push to the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👨‍💻 Author

**Darshit Sorathiya**

- GitHub: [@DarshitSorathiya](https://github.com/DarshitSorathiya)
- Repository: [CropShield](https://github.com/DarshitSorathiya/CropShield)

## 🙏 Acknowledgments

- Plant disease dataset from Kaggle
- TensorFlow and Keras teams
- FastAPI community
- Next.js and Vercel teams
- shadcn/ui for beautiful components

## 📊 Project Status

🟢 **Active Development** - The project is actively maintained and new features are being added.

## 🔮 Future Enhancements

- [ ] Mobile app (React Native)
- [ ] Real-time camera integration
- [ ] Multi-language support
- [ ] Treatment recommendations
- [ ] Disease progression tracking
- [ ] Expert consultation integration
- [ ] Offline mode support
- [ ] Model fine-tuning with user feedback

---

**Made with ❤️ for farmers and agricultural professionals worldwide**
