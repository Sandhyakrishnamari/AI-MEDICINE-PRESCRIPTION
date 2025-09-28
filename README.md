# AI-Driven Medical Report and Prescription Analyzer

A comprehensive FastAPI backend service for analyzing medical reports and prescriptions using AI and OCR technologies.

## Features

### 🔬 Medical Report Analysis
- **OCR Text Extraction**: Uses PaddleOCR for extracting text from PDFs, JPGs, and PNGs
- **Health Value Detection**: Automatically identifies key health metrics (BP, blood sugar, cholesterol, etc.)
- **Abnormal Value Detection**: Compares values against standard medical ranges
- **Medical Report Categorization**: Classifies reports as lab results, prescriptions, or discharge summaries

### 🤖 AI-Powered Analysis
- **Gemini-2.5-Flash Integration**: Uses Google's AI for medical explanations
- **Patient-Friendly Summaries**: Converts medical jargon into understandable language
- **Risk Assessment**: Identifies potential health risks based on abnormal values
- **Medical Term Explanation**: Explains complex medical terms in simple language

### 💊 Medication & Allergy Management
- **Medication Tracking**: Maintains complete medication history with dosages and schedules
- **Allergy Database**: Tracks patient allergies and reactions
- **Drug Interaction Checking**: AI-powered medication interaction analysis
- **Medication Reminders**: Generates structured reminder schedules

### 📅 Appointment Management
- **Consultation Scheduling**: Create and manage doctor appointments
- **Reminder System**: Automated reminders for upcoming appointments
- **Follow-up Tracking**: Track various appointment types (consultations, tests, procedures)

### 📊 Health Timeline & Analytics
- **Chronological Timeline**: Visual timeline of patient's medical history
- **Health Trends Analysis**: Track health metrics over time
- **Risk Pattern Recognition**: Identify concerning trends in health data
- **Export Capabilities**: Export timeline data in JSON/CSV formats

### 💬 AI Medical Chatbot
- **Contextual Q&A**: Answers questions based on patient's medical history
- **Natural Language Processing**: Understands medical queries in plain English
- **Session Management**: Maintains conversation history
- **Suggested Questions**: Provides relevant questions based on patient data

## Tech Stack

- **Backend Framework**: FastAPI with async/await support
- **Database**: MongoDB with Motor (async driver)
- **OCR Engine**: PaddleOCR for text extraction
- **AI/ML**: Google Gemini-2.5-Flash for medical analysis
- **File Processing**: PIL, OpenCV for image preprocessing
- **Authentication**: JWT tokens with passlib
- **Background Tasks**: FastAPI BackgroundTasks for async processing
- **Data Validation**: Pydantic models with comprehensive validation

## API Endpoints

### Patient Management
- `POST /api/v1/patients/` - Create new patient
- `GET /api/v1/patients/{patient_id}` - Get patient details
- `GET /api/v1/patients/{patient_id}/summary` - Get comprehensive patient summary

### File Upload & OCR
- `POST /api/v1/upload/report` - Upload and process medical reports
- `GET /api/v1/upload/report/{report_id}` - Get processed report
- `POST /api/v1/upload/reprocess/{report_id}` - Reprocess existing report

### Medical Analysis
- `POST /api/v1/analysis/summarize/{report_id}` - Generate AI summary
- `POST /api/v1/analysis/explain-terms` - Explain medical terms
- `GET /api/v1/analysis/patient/{patient_id}/timeline` - Get health timeline
- `GET /api/v1/analysis/patient/{patient_id}/trends` - Get health trends

### Medication Management
- `POST /api/v1/medication/` - Add new medication
- `GET /api/v1/medication/patient/{patient_id}` - Get patient medications
- `POST /api/v1/medication/allergy-check` - Check for allergic reactions
- `GET /api/v1/medication/reminders/patient/{patient_id}` - Get medication reminders

### Appointment Management
- `POST /api/v1/appointments/` - Create appointment
- `GET /api/v1/appointments/patient/{patient_id}` - Get patient appointments
- `GET /api/v1/appointments/reminders/due` - Get due appointment reminders

### AI Chatbot
- `POST /api/v1/chatbot/chat` - Chat with AI assistant
- `GET /api/v1/chatbot/sessions/patient/{patient_id}` - Get chat sessions
- `POST /api/v1/chatbot/quick-questions` - Quick Q&A without sessions

### Health Timeline
- `GET /api/v1/timeline/patient/{patient_id}` - Get comprehensive timeline
- `GET /api/v1/timeline/patient/{patient_id}/health-metrics` - Get health metrics
- `GET /api/v1/timeline/patient/{patient_id}/export` - Export timeline data

## Installation & Setup

1. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Set Environment Variables**:
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Configure Google Gemini API**:
   - Get API key from Google AI Studio
   - Add to `GOOGLE_API_KEY` in .env file

4. **Setup MongoDB**:
   - Install MongoDB locally or use MongoDB Atlas
   - Update `MONGODB_URL` in .env file

5. **Create Upload Directory**:
   ```bash
   mkdir uploads
   ```

6. **Run the Application**:
   ```bash
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
   ```

## Project Structure

```
app/
├── main.py                 # FastAPI application entry point
├── config.py               # Configuration settings
├── database.py             # Database connection and collections
├── models/                 # Pydantic models
│   ├── patient.py         
│   ├── report.py          
│   ├── medication.py      
│   ├── appointment.py     
│   └── chatbot.py         
├── routers/               # API route handlers
│   ├── patient_router.py  
│   ├── upload_router.py   
│   ├── analysis_router.py 
│   ├── medication_router.py
│   ├── appointment_router.py
│   ├── chatbot_router.py  
│   └── timeline_router.py 
├── services/              # Business logic services
│   ├── ocr_service.py     # OCR text extraction
│   ├── medical_analyzer.py # Medical data analysis
│   └── gemini_service.py  # AI integration
└── utils/                 # Utility functions
    └── file_utils.py      # File handling utilities
```

## Key Features Implementation

### OCR Processing
- **Image Preprocessing**: Denoising, thresholding for better OCR accuracy
- **Multi-format Support**: PDF, JPG, PNG files
- **Confidence Scoring**: OCR confidence levels for quality assessment
- **Background Processing**: Async file processing with status tracking

### Medical Analysis
- **Reference Ranges**: Built-in medical reference ranges for common tests
- **Pattern Recognition**: Regex patterns for extracting medical values
- **Severity Classification**: Automatic classification of abnormal values
- **Risk Assessment**: Comprehensive risk analysis based on multiple factors

### AI Integration
- **Context-Aware Responses**: AI responses based on patient's medical history
- **Medical Explanation**: Simplifies complex medical terms
- **Interaction Checking**: Identifies potential drug interactions
- **Personalized Recommendations**: Tailored health advice

### Data Security
- **Input Validation**: Comprehensive data validation using Pydantic
- **File Type Validation**: Restricts uploads to allowed file types
- **Size Limits**: Configurable file size limits
- **Error Handling**: Comprehensive error handling and logging

## API Documentation

Once running, visit:
- **Swagger UI**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc
- **Health Check**: http://localhost:8000/health

## Usage Examples

### Upload a Medical Report
```python
import requests

files = {'file': open('medical_report.pdf', 'rb')}
data = {'patient_id': 'patient_id_here'}
response = requests.post('http://localhost:8000/api/v1/upload/report', 
                        files=files, data=data)
```

### Get AI Summary
```python
report_id = "report_id_here"
response = requests.post(f'http://localhost:8000/api/v1/analysis/summarize/{report_id}')
summary = response.json()
```

### Chat with AI Assistant
```python
chat_data = {
    "patient_id": "patient_id_here",
    "message": "What do my blood test results mean?"
}
response = requests.post('http://localhost:8000/api/v1/chatbot/chat', 
                        json=chat_data)
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License. See LICENSE file for details.

## Support

For issues and questions:
- Create an issue on GitHub
- Check the API documentation at `/docs`
- Review the logs for error details

## Disclaimer

This application is for informational purposes only and should not replace professional medical advice. Always consult with healthcare providers for medical decisions.