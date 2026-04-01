# Full Project Documentation & Journey

## 1. Project Introduction
This project is about building and deploying a containerised web application for project tracking inside a private cloud environment.  

**Objective:** To design a private cloud infrastructure and containerise a sample web application so it can run securely on-premise.  
**Problem solved:** The enterprise needs an internal project tracking tool but cannot use public cloud services due to data sensitivity and regulatory requirements. We simulated a private cloud and used Docker to make the application portable and efficient.

## 2. Tools & Technologies
- Docker (containerisation)
- Flask + SQLAlchemy + Bootstrap (web application)
- Ubuntu 22.04 LTS (private cloud VM)
- Git & GitHub (version control)
- SQLite (persistent database)

## 3. System Requirements
- Operating System: Ubuntu 22.04 LTS
- Minimum RAM: 4 GB (8 GB recommended for Docker)
- Docker Engine installed

## 4. Project Architecture
**Flow:** User → Browser → Private Cloud VM (Compute Node) → Docker Container → Flask Application + SQLite Database  

Containers isolate the application and its dependencies, making it portable and lightweight. The app is served on port 5000 inside the container and exposed only internally on the private cloud network (192.168.100.0/24). Persistent data is stored using a Docker volume so the database survives container restarts.

## 5. Application Development
Since no application was provided by the lecturer, I created a simple project-tracking web application.  
**Features:** Add, view, edit and delete projects, status tracking (In Progress / Completed / On Hold), and persistent storage.  
The app is responsive and uses Bootstrap for a clean interface.

**Folder structure (in Cloud_project_app repo):**
- app.py
- Dockerfile
- requirements.txt
- templates/ (base.html, index.html, project.html)

## 6. Docker Setup
We used a single-stage Dockerfile based on the official `python:3.12-slim` image.  

**Dockerfile explanation:**
- Base image: python:3.12-slim (lightweight and secure)
- Copies application code and installs dependencies
- Exposes port 5000
- Sets up a volume for the database

**Commands used:**
```
docker build -t project-tracker .
```
```
docker run -d -p 5000:5000 -v $(pwd)/data:/data --name project-tracker-app project-tracker
```
7. Docker Compose
Docker Compose was not used. A single container was sufficient for this assignment scope. Using plain Docker commands kept the solution simple and directly matched the requirement to deploy on the private cloud VM.
8. Deployment Process

Installed Docker on the Ubuntu 22.04 private cloud VM
Cloned the application repository
Built the Docker image
Ran the container with persistent volume
Accessed the application in the browser at http://192.168.100.15:5000

9. Testing
The application was tested locally first, then inside the Docker container on the private cloud VM. Full CRUD operations were performed (add, view, edit, delete projects). The app works correctly and data persists after container restart.
10. Screenshots
All important steps are saved in the screenshots/ folder:

Terminal commands and Docker build
Running container (docker ps)
Application running in browser

11. Challenges & Solutions

Volume mount was overwriting application files → Fixed by mounting only the /data directory and updating the database path.
Port 5000 was already in use → Solved using sudo lsof -ti:5000 | xargs kill -9.
Database connection issues inside container → Updated SQLALCHEMY_DATABASE_URI to absolute path.

12. Conclusion
This project successfully delivered a containerised project-tracking application running in a simulated private cloud environment. I learned the importance of proper volume management and internal networking for stateful applications. The final solution fully meets the assignment requirements for data residency and containerised deployment.
13. Future Improvements

Deploy using OpenStack Magnum with Kubernetes orchestration
Replace SQLite with PostgreSQL for better scalability
Add user authentication and role-based access