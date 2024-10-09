
# Serverless Distributed Video Transcoding System

This project implements a robust, distributed video transcoding system using **Node.js**, **AWS services**, **Redis**, and **ffmpeg**. By utilizing a leaky bucket rate-limiting algorithm, it efficiently converts uploaded videos into multiple resolutions, enhancing accessibility and offering optimized viewing experiences for users. The system supports automated transcoding for multiple video qualities and allows users to preview and download files in different resolutions.

## 🚀 Key Features

- **Automated Video Transcoding**: Converts uploaded videos into various resolutions (360p, 480p, 720p, 1080p) using **ffmpeg**.
- **Leaky Bucket Rate Limiting**: Manages video transcoding requests, ensuring a maximum of 5 concurrent jobs for optimal resource use.
- **AWS Integration**: Integrates **S3**, **Lambda**, **EventBridge**, **ECS**, and **MongoDB** for storage, event-driven architecture, and database management.
- **Queue Management**: Uses **Redis** for task queuing to ensure efficient and scalable job execution.
- **Dockerized Architecture**: Docker containers are deployed on ECS for seamless video transcoding.
- **Secure Uploads & Downloads**: Utilizes signed URLs to securely handle video uploads and downloads via **AWS S3**.
- **Webhook Notifications**: Sends real-time notifications upon task completion or failure using webhook calls.

---
---

## 🏗️ System Architecture
![Video Transcoder Service System Design](https://github.com/user-attachments/assets/14cbd7d1-3a13-4f71-a768-37f2d6d380e9)

 This system uses a serverless and containerized architecture for seamless scalability. The design involves several core components:

- **Frontend (Next.js)**: Enables video upload, previews, and download functionality.
- **Backend (Node.js + Express)**: Manages video transcoding, and queues, and handles RESTful APIs for rate-limited jobs.
- **AWS S3**: Secure storage for video files.
- **Redis**: Queue management for video transcoding tasks.
- **AWS ECS & Docker**: Video transcoding containers running on ECS clusters.
- **AWS Lambda & EventBridge**: Triggers video transcoding workflows when a video upload event occurs in S3.

![System Architecture](https://github.com/AbhishekCS3459/Video-Transcoder-Service/assets/14cbd7d1-3a13-4f71-a768-37f2d6d380e9)

---

## 🛠️ Tech Stack

### Frontend:
- **Next.js**: For the user interface, enabling video uploads and previews.

### Backend:
- **Node.js + Express**: Main backend for handling APIs and video transcoding logic.

### Database & Queue:
- **MongoDB**: Stores video metadata and transcoding statuses.
- **Redis**: Manages the job queue for video transcoding.

### AWS Services:
- **S3**: Storage for videos and transcoded files.
- **Lambda**: Serverless processing for backend logic.
- **ECS**: Container orchestration for transcoding tasks.
- **EventBridge**: Event-driven architecture to initiate workflows.

### Tools & Frameworks:
- **Docker**: Containerized transcoding process.
- **ffmpeg**: Video transcoding engine.
- **Serverless Framework**: Simplifies the deployment of Lambda functions.

---

## ⚙️ Project Structure

The project is divided into several components:

- **ecs-task**: Manages the transcoding process via Docker containers running on ECS.
- **upload-trigger-api**: Lambda function triggered by S3 events to initiate transcoding jobs.
- **video-transcoder-client**: Next.js-based frontend for video uploads, previews, and downloads.
- **video-transcoder-server**: Serverless backend for managing transcoding tasks, queues, and webhook notifications.

---

## 🧩 Setup Instructions

1. **Clone the Repository**:  
   ```
   git clone https://github.com/AbhishekCS3459/Video-Transcoder-Service.git
   ```

2. **Install Dependencies**:
   - Navigate to each component (`frontend`, `backend`, `ecs-task`) and run:
     ```
     npm install
     ```

3. **Environment Variables**:
   - Configure environment variables for AWS (S3, ECS, Lambda), Redis, MongoDB, etc.

4. **Deploy Backend**:
   - Deploy Lambda functions and ECS tasks using **Serverless Framework**:
     ```
     serverless deploy
     ```

5. **Launch Frontend**:
   - Build and run the frontend with:
     ```
     npm run build
     npm start
     ```

---

## 🚀 How It Works

### Step-by-Step Process:

1. **Upload Video**: Users upload videos through the frontend interface.
2. **Trigger Transcoding**: A Lambda function is triggered upon video upload (via S3 event). This function queues the transcoding job in Redis.
3. **ECS Transcoding**: Docker containers managed by ECS pick up the jobs from Redis, transcode the videos, and save the output to S3.
4. **Webhook & Notifications**: Once the transcoding is done, the system sends webhook notifications (success/failure).
5. **Preview & Download**: Users can preview or download transcoded files in multiple resolutions via the frontend.

---

## 📦 Deployment

To deploy the system:

1. **Setup AWS Services**: 
   - Configure S3 buckets, ECS clusters, Lambda functions, and EventBridge rules.
   
2. **Deploy Serverless Backend**:
   - Use Serverless Framework to deploy backend Lambda functions and ECS tasks.

3. **Build Frontend**:
   - Deploy the Next.js frontend to a hosting platform (Netlify, Vercel, etc.).

4. **Environment Variables**:
   - Configure AWS credentials, Redis, MongoDB, and other required variables.

---

## 📂 Endpoints

Here’s a list of API endpoints:

- `POST /upload` - Uploads a video to the system.
- `GET /transcode/:id/status` - Retrieves the transcoding status for a video.
- `GET /transcode/:id/download` - Downloads a specific transcoded file.

---

## 🛠️ Improvements

Future enhancements may include:

- **Parallel Processing**: Further optimize transcoding by enabling parallel job executions based on available resources.
- **Live Streaming Support**: Add support for transcoding live video streams.
- **Enhanced Error Handling**: Improve error-handling mechanisms and resilience of ECS tasks.

---

## 🤝 Contribution Guidelines

Contributions are always welcome! Feel free to open an issue or submit a pull request. Please ensure your contributions align with our coding standards and guidelines.

---

## 📝 License

This project is licensed under the [MIT License](LICENSE).

---
