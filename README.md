# 🧠 Shodh-a-Code Contest Platform
_A Full-Stack Real-Time Coding Contest Platform_

A full-stack web application that enables students to participate in real-time coding contests.  
The system provides a **containerized backend code judge**, **live leaderboard**, and a **smooth frontend** experience for submissions and results.

![Tech Stack](https://img.shields.io/badge/Backend-Spring%20Boot-green)
![Frontend](https://img.shields.io/badge/Frontend-React%20+%20Vite-blue)
![Database](https://img.shields.io/badge/Database-H2-orange)
![Styling](https://img.shields.io/badge/Styling-Tailwind%20CSS-purple)

---


---

## 📦 Repository Structure

```
shodh-a-code-platform/
├── backend/               # Spring Boot backend (REST APIs + Docker code judge)
├── frontend/              # React + Vite frontend (contest UI, code editor, leaderboard)
├── docker/                # Docker judge environment files
├── docker-compose.yml     # Spins up backend, frontend, and judge containers
└── README.md
```

---

## 🚀 Project Overview
**Shodh-a-Code** is a full-stack coding contest platform where users can join live contests, attempt problems, submit code, and view real-time rankings.  
It simulates a **competitive programming experience** with secure, containerized code execution and an intuitive frontend interface.

---

## ⚙️ Tech Stack

| Layer                | Technology                          |
| -------------------- | ----------------------------------- |
| **Frontend**         | React, Vite, Tailwind CSS           |
| **Backend**          | Spring Boot (Java 17), REST APIs    |
| **Database**         | H2 (in-memory, prepopulated)        |
| **Containerization** | Docker, Docker Compose              |
| **Editor**           | Monaco Editor                       |
| **Async Updates**    | REST Polling (status & leaderboard) |

---

## 🧩 Features

- Join contests using Contest ID and username  
- View problems with inputs, outputs, and constraints  
- Code directly in browser (Monaco editor)  
- Submit solutions asynchronously  
- Real-time status updates (“Running”, “Accepted”, etc.)  
- Live leaderboard with periodic refresh  
- Secure Docker-based code execution  
- Automatic container cleanup and isolation  

---

## 🧱 Backend Overview (Spring Boot)

### Entities
- **Contest** – Represents a coding contest  
- **Problem** – Includes title, description, input/output test cases  
- **User** – Represents a participant  
- **Submission** – Tracks user code and result (Pending, Running, Accepted, etc.)

### Core API Endpoints
| Method | Endpoint                                | Description                              |
| ------ | --------------------------------------- | ---------------------------------------- |
| `GET`  | `/api/contests/{contestId}`             | Fetch contest details with problems      |
| `POST` | `/api/submissions`                      | Accept new submission, queue for judging |
| `GET`  | `/api/submissions/{submissionId}`       | Get submission status and result         |
| `GET`  | `/api/contests/{contestId}/leaderboard` | Fetch live leaderboard                   |

---

## 🔁 Code Execution Flow

1. User submits code → backend saves submission as **Pending**  
2. Judge service triggers Docker run with mounted user code  
3. Container executes the code with problem input via `stdin`  
4. Output is captured and compared to expected output  
5. Status is updated (**Accepted**, **Wrong Answer**, etc.)  
6. Temporary files and containers are cleaned up automatically  

---

## 🖥️ Frontend Overview (React + Vite)

### Pages
1. **Join Page** – Input Contest ID & Username  
2. **Contest Page** – Problem display, code editor, and live leaderboard  

### Async Logic
1. On submit → `POST /api/submissions`  
2. Poll every 2–3 seconds → `GET /api/submissions/{id}`  
3. Display real-time status updates  

---

## 🐳 Docker Setup

### Judge Container
- Based on `openjdk:17`  
- Executes code with memory & CPU limits  
- Input passed via stdin, output captured from stdout  
- Cleans up after each run  

### Docker Compose
Orchestrates:
1. Backend (Spring Boot)  
2. Frontend (React + Vite)  
3. Docker-in-Docker (Judge Environment)

---

## ⚙️ Setup Instructions

### 🧩 Option 1: Docker Setup (Recommended)

#### Step 1: Build the Judge Image
```bash
docker build -t shodh-judge:latest ./docker/judge
```

#### Step 2: Start All Services
```bash
docker-compose up --build
```

Access:
- Backend → `http://localhost:8080`  
- Frontend → `http://localhost:3000`

#### Step 3: Access the Application
1. Open `http://localhost:3000`  
2. Use **Contest ID: 1**  
3. Enter any username to join  

#### Stop Services
```bash
docker-compose down
```

---

### 🧩 Option 2: Local Development Setup

#### Start Backend
```bash
cd backend
mvn spring-boot:run
```

#### Start Frontend
```bash
cd frontend
npm install --legacy-peer-deps
npm run dev
```

Access: `http://localhost:5173`

---

## 🧠 Design Choices & Justifications

1. **Spring Boot + React Architecture** — clean separation, scalable, API-driven  
2. **Polling Instead of WebSockets** — simpler and reliable for prototype  
3. **H2 Database** — lightweight and ideal for local testing  
4. **Dockerized Judge** — ensures secure and isolated code execution  
5. **Local State Management** — minimal complexity, no Redux needed  

---

## 🏗️ Architecture Diagram

```
┌──────────────────────────────┐
│          FRONTEND            │
│ React + Tailwind + Vite      │
│  - Contest Page              │
│  - Code Editor               │
│  - Leaderboard               │
└──────────────┬───────────────┘
               │ REST API Calls
┌──────────────┴───────────────┐
│         BACKEND (Spring)     │
│  /api/contests               │
│  /api/submissions            │
│  /api/leaderboard            │
└──────────────┬───────────────┘
               │ Executes Docker
┌──────────────┴───────────────┐
│        DOCKER JUDGE ENGINE   │
│  - Runs code safely          │
│  - Validates output          │
│  - Cleans up containers      │
└──────────────────────────────┘
```

---

## 📁 Project Structure

```
shodh-a-code/
├── backend/                  
│   ├── src/main/java/com/shodh/backend/
│   │   ├── config/       # Configuration (CORS, Async, Data Init)
│   │   ├── controller/   # REST API endpoints
│   │   ├── dto/          # Data Transfer Objects
│   │   ├── model/        # JPA Entities
│   │   ├── repository/   # Database repositories
│   │   └── service/      # Business logic
│   └── pom.xml
│
├── frontend/                
│   ├── src/
│   │   ├── components/   # Reusable components
│   │   ├── pages/        # Page components
│   │   ├── services/     # API service layer
│   │   └── index.css     # Global styles
│   └── package.json
│
└── README.md
```

---

## 🚧 Limitations & Future Enhancements

### Current Limitations
- Supports **Java submissions only**  
- No authentication or role-based access  

### Planned Enhancements
- Multi-language support (C++, Python, JS)  
- WebSockets for real-time updates  
- Admin dashboard for contest management  
- Persistent data storage (PostgreSQL)  

---

## 💾 Database Schema

### Core Entities
- **User** – Participant info  
- **Contest** – Contest details  
- **Problem** – Problem statements  
- **TestCase** – Input/output test cases  
- **Submission** – Code submissions and results  

### Prepopulated Data
- Contest 1 (Active): 3 Problems (Two Sum, Palindrome Number, FizzBuzz)  
- Contest 2 (Future): 1 Problem  
- Users: `alice`, `bob`, `charlie`

---

## 🔄 Submission Flow

1. User submits code  
2. Backend stores it as `PENDING`  
3. Judge service executes code in Docker  
4. Status changes → `RUNNING`  
5. Backend updates result (`ACCEPTED`, `WRONG_ANSWER`, etc.)  
6. Frontend polls & updates leaderboard  

---

## 🛠️ Technology Stack

### Backend
- Spring Boot 3.5.7  
- Spring Data JPA  
- H2 Database  
- Lombok  
- Maven  

### Frontend
- React 18, Vite  
- Tailwind CSS  
- Monaco Editor  
- Axios, React Router  
- Framer Motion, React Hot Toast  

---
## 🎯 Design Decisions

### Architectural Choices

1.⁠ ⁠*Microservice-Ready Architecture*: Backend and frontend are completely decoupled, communicating only through REST APIs.

2.⁠ ⁠*Asynchronous Processing*: Code judging happens asynchronously to prevent blocking API responses.

3.⁠ ⁠*Polling vs WebSockets*: Chose polling for simplicity and to avoid WebSocket complexity in the prototype.

4.⁠ ⁠*In-Memory Database*: H2 for rapid development and testing without external dependencies.

### Frontend Design

1.⁠ ⁠*Glass Morphism UI*: Modern, visually appealing dark theme with transparency effects.

2.⁠ ⁠*Component-Based Architecture*: Reusable components for maintainability.

3.⁠ ⁠*State Management*: Local component state with props drilling (sufficient for current scope).

### Backend Design

1.⁠ ⁠*Service Layer Pattern*: Clear separation between controllers, services, and repositories.

2.⁠ ⁠*DTO Pattern*: Separate data transfer objects from entities for API flexibility.

3.⁠ ⁠*Repository Pattern*: Abstract database operations for easy switching between databases.

## 🐳 Docker Architecture

	⁠*📚 For detailed Docker setup, see [DOCKER_SETUP.md](DOCKER_SETUP.md)*

### Code Execution Flow with Docker

1.⁠ ⁠*User submits code* → Backend receives submission
2.⁠ ⁠*Backend writes code* to temporary file in ⁠ /tmp/judge ⁠
3.⁠ ⁠*Backend spawns Docker container* (⁠ shodh-judge:latest ⁠) with:
   - User code mounted as volume
   - Memory limit (e.g., 256MB)
   - CPU limit (1 core)
   - Network disabled (security)
   - Time limit enforced
4.⁠ ⁠*Container compiles code* (if needed: Java, C++)
5.⁠ ⁠*Container executes code* against test cases
6.⁠ ⁠*Backend captures stdout* and compares with expected output
7.⁠ ⁠*Container auto-removed* after execution
8.⁠ ⁠*Temp files cleaned up*

### Security Features

•⁠  ⁠*Network Isolation*: Containers run with ⁠ --network none ⁠
•⁠  ⁠*Non-root User*: Code executes as ⁠ coderunner ⁠ user (UID 1000)
•⁠  ⁠*Resource Limits*: Memory and CPU constraints enforced
•⁠  ⁠*Auto-cleanup*: Containers and temp files removed after execution
•⁠  ⁠*Time Limits*: Process killed if exceeds time limit

### Judge Container Contents

The ⁠ shodh-judge:latest ⁠ image includes:

•⁠  ⁠*OpenJDK 17* (Java)
•⁠  ⁠*Python 3*
•⁠  ⁠*GCC/G++* (C++)
•⁠  ⁠*Node.js 18* (JavaScript)

## ⚠️ Current Limitations

1.⁠ ⁠*Authentication*: No user authentication system
2.⁠ ⁠*Database*: Uses in-memory H2 (data lost on restart)
3.⁠ ⁠*Memory Tracking*: Approximate memory usage (requires Docker stats API for precision)

## 🔮 Future Enhancements

•⁠  ⁠[x] Docker containerization for secure code execution ✅
•⁠  ⁠[x] Support for multiple programming languages (Java, Python, C++, JavaScript) ✅
•⁠  ⁠[ ] User authentication and authorization
•⁠  ⁠[ ] WebSocket for real-time updates
•⁠  ⁠[ ] PostgreSQL for production database
•⁠  ⁠[ ] Admin panel for contest management
•⁠  ⁠[ ] Code execution metrics and analytics
•⁠  ⁠[ ] Problem difficulty ratings
•⁠  ⁠[ ] User profiles and statistics
•⁠  ⁠[ ] Advanced Docker resource monitoring with stats API


## 🧪 Testing the Application

1. Join Contest ID `1` with any username  
2. Select a problem  
3. Write code in Monaco Editor  
4. Submit and observe status updates  
5. View leaderboard rankings  

---

### Sample Solutions (Function-Only, LeetCode Style)

#### Two Sum (Java)

⁠ java
public int[] twoSum(int[] nums, int target) {
    java.util.Map<Integer, Integer> map = new java.util.HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[] { map.get(complement), i };
        }
        map.put(nums[i], i);
    }
    return new int[] { 0, 0 };
}
 ⁠

#### Palindrome Number (Java)

⁠ java
public boolean isPalindrome(int x) {
    if (x < 0) return false;
    int original = x, reversed = 0;
    while (x != 0) {
        int digit = x % 10;
        reversed = reversed * 10 + digit;
        x /= 10;
    }
    return reversed == original;
}
 ⁠

#### FizzBuzz (Java)

⁠ java
public java.util.List<String> fizzBuzz(int n) {
    java.util.List<String> result = new java.util.ArrayList<>();
    for (int i = 1; i <= n; i++) {
        if (i % 15 == 0) result.add("FizzBuzz");
        else if (i % 3 == 0) result.add("Fizz");
        else if (i % 5 == 0) result.add("Buzz");
        else result.add(String.valueOf(i));
    }
    return result;
}
 ⁠

#### Two Sum (Python)

⁠ python
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return [0, 0]
 ⁠

#### Two Sum (C++)

⁠ cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (seen.count(complement)) {
            return {seen[complement], i};
        }
        seen[nums[i]] = i;
    }
    return {0, 0};
}
 ⁠

#### Two Sum (JavaScript)

⁠ javascript
function twoSum(nums, target) {
  const seen = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (seen.has(complement)) {
      return [seen.get(complement), i];
    }
    seen.set(nums[i], i);
  }
  return [0, 0];
}
 ⁠


## 🐳 Docker Troubleshooting

### Issue: Backend can't spawn Docker containers

*Problem*: Backend is running in Docker but can't execute judge containers.

*Solution*: Ensure Docker socket is mounted in ⁠ docker-compose.yml ⁠:

⁠ yaml
volumes:
  - /var/run/docker.sock:/var/run/docker.sock
 ⁠

### Issue: Permission denied on Docker socket

*Solution*:

⁠ bash
# On Linux, add your user to docker group
sudo usermod -aG docker $USER
# Then log out and back in
 ⁠

### Issue: Judge image not found

*Solution*: Build the judge image first:

⁠ bash
docker build -t shodh-judge:latest ./docker/judge
 ⁠

### Issue: Port already in use

*Solution*: Change ports in ⁠ docker-compose.yml ⁠ or stop conflicting services:

⁠ bash
# Check what's using port 8080
netstat -ano | findstr :8080  # Windows
lsof -i :8080                 # Mac/Linux

---

## 📝 License
This project is built as an assessment for **Shodh AI**.

---

## 🤝 Contributing
This is an assessment project. For any issues or questions, please refer to the documentation above.

---

**Built  using Spring Boot and React**
