
# Serverless Video Transcoding System

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

### **System Flow Breakdown**
1. **Client Interaction**
   - The **client** requests a signed URL from the server (likely powered by a Node.js backend).
   - The server generates and returns a **signed URL** for an S3 bucket. This URL allows the client to upload the video securely.

2. **Video Upload**
   - The **client uploads the raw video** to an **S3 bucket** using the signed URL.
   - This upload event **triggers a Lambda function**.

3. **Lambda Function**
   - The Lambda function extracts **video metadata** (e.g., file name, format, size).
   - It sends this metadata to the server for further processing.

4. **Redis Queue Management**
   - The server uses **Redis** as a temporary storage for the metadata (or jobs).
   - If the number of jobs reaches a specific threshold (e.g., 5), the metadata is pushed to a **Job Queue** (e.g., an SQS queue or Redis-based queue).
   - If there are fewer than 5 jobs, the job is passed directly to a **Consumer** for processing.

5. **Consumer**
   - The consumer fetches the job from the queue and processes it.
   - It downloads the raw video from the **temporary S3 bucket**.

6. **Video Transcoding (AWS ECS)**
   - The consumer interacts with **AWS ECS** (Elastic Container Service), where Docker containers running **FFmpeg** perform the actual video transcoding.
   - The video is transcoded into multiple resolutions (e.g., 360p, 480p, 720p).

7. **Uploading Transcoded Videos**
   - Once transcoding is complete, the transcoded video files are uploaded to a **final S3 bucket**.
   - These files are organized for the client to download.

8. **Client Fetches Transcoded Videos**
   - The client can now download the transcoded videos from the **final S3 bucket**.

---

### **Key Components**
1. **AWS S3**: Used for temporary and final storage of video files.
2. **AWS Lambda**: Serverless compute that processes video metadata upon S3 events.
3. **Redis**: Acts as a job scheduler and buffer for handling metadata efficiently.
4. **Job Queue**: Ensures proper load management for video transcoding jobs.
5. **AWS ECS with FFmpeg**: Highly scalable containerized service for video transcoding.
6. **Signed URLs**: Provide secure, temporary access to upload and download files.

---

### **Why Serverless?**
- **Scalability**: Serverless components like Lambda and S3 handle dynamic workloads.
- **Cost Efficiency**: Resources (e.g., ECS, Lambda) only run when required.
- **Modularity**: Each component (uploading, metadata processing, transcoding) is independently scalable.

---

### **Strengths of this Design**
1. **Efficient Resource Utilization**: The consumer and ECS only work when enough jobs are queued.
2. **Scalable Transcoding**: ECS allows scaling FFmpeg containers for high-resolution videos or multiple formats.
3. **Reliability**: Redis buffers jobs, ensuring no data loss during peak loads.
4. **Separation of Concerns**: Clear responsibilities for each component (upload, process, transcode, deliver).

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
