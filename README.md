# PhishLens

**PhishLens** is a web application that detects deepfake or manipulated images using Google's Vision API and Google Gemini. It connects to a user's Google Photos, retrieves images, and analyzes them for possible phishing or face-swap manipulations.

**Live App:** https://phishlens-frontend-1087775975982.us-west1.run.app/
**Source Code:** https://github.com/phishlensweb/PhishLens
**Demo Video:** https://youtu.be/LySRsGC9_S8

---

## How It Works

1. User logs in via Google OAuth 2.0
2. PhishLens connects to Google Photos (or accepts a direct upload)
3. Google Cloud Vision API detects faces and returns bounding boxes + landmarks
4. Gemini receives the Vision data and performs deepfake risk reasoning
5. Results (risk score, manipulation type, explanation) are stored in Firestore and shown on the dashboard

---

## Screenshots

### User Dashboard – Saved Analyses

The dashboard shows all previously analyzed images. Each card displays the image thumbnail, date analyzed, source type, Gemini risk score, and a button to reopen the full analysis.

![User Dashboard](https://github.com/user-attachments/assets/0c236716-c0b6-434e-985a-c4aaa48c1a5e)

---

### Uploading or Importing a New Image

Users can analyze new images by uploading a JPG/PNG directly or by connecting their Google Photos library via OAuth 2.0.

![Application Running](https://github.com/user-attachments/assets/1f3aecb4-3686-4e40-b2cc-f9bb1a5b71c6)

---

### Google Vision API Results (Bounding Boxes)

Google Cloud Vision detects the face region and returns bounding box coordinates, facial landmarks, and confidence values. PhishLens overlays the detection directly on the image.

![Google Vision API Results](https://github.com/user-attachments/assets/81afed7d-9f08-435f-8aeb-c21beb570005)

---

### Gemini Analysis Results (Deepfake Reasoning)

Gemini receives Vision's facial landmark data and performs reasoning to determine a deepfake risk score (0–100), the suspected manipulation type, detailed reasoning, and a recommended action.

![Google Gemini API Results](https://github.com/user-attachments/assets/878d688e-42cd-45ba-8c0f-877c059c3b06)

---

### Firestore Database – Stored Records

All analysis results are saved in Google Firestore. Each image document includes `imageId`, `userId`, `source`, `createdAt`, and `updatedAt`. This enables skipping already-processed images.

![Firestore Database](https://github.com/user-attachments/assets/27155c19-7a5c-4c44-9de0-911e29be8154)

---

### Additional Screenshots

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/f4e98cf9-9c8b-4b11-a285-8d37a7a9bedb" />

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/f74a1fae-48cf-4b5e-b36c-c4438b8daf75" />

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/50db8fb8-d241-4b80-9cd9-582f04ca803c" />

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/f9019812-fe86-4fd0-8087-104af59d826e" />

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/212b3170-a2e7-4286-afc2-17eebad9ad5b" />

<img width="800" height="auto" alt="image" src="https://github.com/user-attachments/assets/6cfd7cd7-5f8d-4887-b804-1ce9cb5034ca" />

---

## Project Structure

### Backend (Node.js + Express)

| File | Description |
|------|-------------|
| `server.js` | Starts the Express server and loads all routes |
| `routes/auth.js` | Handles Google OAuth login and callback |
| `routes/photos.js` | Retrieves the user's Google Photos (metadata + image bytes) |
| `routes/analyze.js` | Core pipeline: Vision API → Gemini → save results to Firestore |
| `services/vision.js` | Sends the image to Google Vision API and returns features |
| `services/gemini.js` | Sends Vision features to Gemini and generates risk scoring + explanation |
| `services/firestore.js` | Stores and retrieves analysis results in Firestore |

### Frontend (Vite + React)

| File | Description |
|------|-------------|
| `src/main.tsx` | React application entry point |
| `src/App.tsx` | Defines routing and high-level layout |
| `src/pages/Auth.tsx` | Starts Google OAuth login flow |
| `src/pages/Upload.tsx` | Allows user to upload/select photos for analysis |
| `src/pages/Analysis.tsx` | Displays Vision + Gemini results |
| `src/pages/Dashboard.tsx` | Shows history of past results |

---

## Database – Google Firestore

- **Database**: Google Cloud Firestore (NoSQL)
- **Project ID**: `phishlens-478600`
- **Library**: `@google-cloud/firestore` v7.10.0
- **Authentication**: Service account JSON via `GOOGLE_APPLICATION_CREDENTIALS`

### Collections

**`users`** – User authentication data
```json
{
  "userId": "123456789",
  "email": "user@example.com",
  "name": "John Doe",
  "googleTokens": {
    "access_token": "...",
    "refresh_token": "..."
  }
}
```

**`images`** – Image metadata
```json
{
  "imageId": "img_1701513330123",
  "userId": "user@example.com",
  "source": "upload",
  "createdAt": "2024-12-02T10:15:30.000Z"
}
```

**`results`** – Analysis results
```json
{
  "imageId": "img_1701513330123",
  "userId": "user@example.com",
  "vision": { "facesCount": 2 },
  "gemini": { "risk": 45, "reason": "..." }
}
```

**`analytics_daily`** – Daily metrics (document ID format: `YYYY-MM-DD`)

---

## Code Style

- Variable and function names follow **camelCase** throughout
- Major functions include clear comments describing their purpose
- Comments are added around Vision API, Gemini API, and Firestore logic
- Key sections are documented; all important functions are clearly explained

---

## Resources

- [Project Proposal (PDF)](https://github.com/user-attachments/files/23940393/Proposal.pdf)
- [Presentation Slides (PDF)](https://github.com/user-attachments/files/23940571/PhishLens.Presentation.pdf)
- [Analytics & Logging Report (PDF)](https://github.com/user-attachments/files/23894676/Analytics.Logging.pdf)
- [Demo Video](https://youtu.be/LySRsGC9_S8)

