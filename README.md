## Docker Setup (8.2HD — Individual Task READ ME FILE / SET UP)

This section documents how to run the fully containerised version of the KFZ-Legal application using Docker.

---

### 1. Clone the Repository

```bash
git clone https://github.com/zaradanziger/KZF-Legal-8.2hd.git
cd KZF-Legal-8.2hd
```
(This is my individual fork of the group task)

---

### 2. Environment Configuration (Required)

This application requires a `.env` file in the root directory containing sensitive API keys and configuration values. This file is **not committed to the repository** for security reasons.

**Step 1:** Copy the provided example file to create your `.env`:

```bash
cp .env.example .env
```

**Step 2:** Open the `.env` file and fill in the secret values.

The actual secret values (API keys, JWT secret) are provided in the **OnTrack submission 8.2hd pdf**. Copy them exactly as provided into your `.env` file.

Your completed `.env` file should contain the following keys:
THEY ARE IN THE 8.2HD TASK SUBMISSION PDF

---

### 3. Build and Start the Application

From the root of the project, run:

```bash
docker compose up --build
```

This command will:
- Build the Node.js application image
- Pull and start a MongoDB container
- Start the Express server on port 3000
- Connect the app to MongoDB automatically

Wait until you see the following in the logs:
MongoDB connected under URI: mongodb://mongo:27017/kfz-legal
Server running on port 3000

---

### 4. Access the Application

Once running, open your browser and navigate to:

| URL | Description |
|-----|-------------|
| `http://localhost:3000` | Main application frontend |
| `http://localhost:3000/api/health` | Health check endpoint |
| `http://localhost:3000/api/student` | Student identification endpoint |

---

### 5. Student Identification Endpoint

As required by the 8.2HD task, the following endpoint identifies this submission:

**GET** `http://localhost:3000/api/student`

Expected response:
```json
{
  "name": "Zara Danziger",
  "studentId": "s223468285"
}
```

---

### 6. Stopping the Application

To stop and remove the containers:

```bash
docker compose down
```

To also remove the stored database volume:

```bash
docker compose down -v
```

---

### Architecture Overview

The Dockerised application consists of two containers managed by Docker Compose:

| Container | Image | Port |
|-----------|-------|------|
| `app` | Custom Node.js 22 image | 3000 |
| `mongo` | `mongo:6` | 27017 |

The app container waits for MongoDB to pass a health check before starting, ensuring reliable database connectivity.