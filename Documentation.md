# Easycrud-docker-rds
my-git--docker--rds
###Documentation###
##1.Purpose##: Deploy  (frontend, backend, database) inside Docker containers, with minimal infrastructure friction.
#Tech stack#
#Frontend#:Node.js + npm
#Backend# : Java + Maven
#Database#- AWS EC2 (t2.medium)

| Layer     | Technology |
|-----------|------------|
| ## Prerequisites
- AWS EC2 instance  
  - Type: *t2.medium*  
  - Storage: *30GB*  
- Docker Engine installed  
- Git installedFrontend  | Node.js, npm |
| Backend   | Java, Maven |
| Database  | MySQL / MariaDB |
| Hosting   | AWS EC2 (t2.medium) |
 

# A Database Set (MySQL / MariaDB)

<img width="1905" height="820" alt="Screenshot 2025-11-05 204035" src="https://github.com/user-attachments/assets/4692ce52-624f-4d93-8f48-b5077498d41d" />
bash
### 1.Run MySQL container
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=<your_password> mysql

2.Enter inside database container
docker exec -it <container_id> bash
3. Login to MySQL
mysql -u root -p

4. Create database
CREATE DATABASE student_db;

5. Backend connection details
DB_USER=<your_username>
DB_PASS=<your_password>
DB_PORT=3306
DB_NAME=student_db
DB_HOST=<database_container_ip>
ME=student_db
## B. Backend Configuration (Java + Maven)
bash
1. Clone project repository
git clone <your_repo_url>
cd EasyCRUD/backend
Update application.properties
Copy and edit:
cp src/main/resources/application.properties ./application.properties
vim application.properties
Update values:

Replace localhost with DB container IP
Set correct DB username & password

3. Backend Dockerfile
Create backend/Dockerfile:

FROM maven:3.8.3-openjdk-17
COPY . /opt/
WORKDIR /opt
COPY application.properties src/main/resources/application.properties
RUN mvn clean package
WORKDIR /opt/target
EXPOSE 8080
CMD ["java","-jar","student-registration-backend-0.0.1-SNAPSHOT.jar"]
4. Build backend image
docker build -t backend-app .

Run backend container
docker run -d -p 8080:8080 backend-app

##  C. Frontend Deployment (Node.js + Apache)
bash
1. Update .env
cd EasyCRUD/frontend
vim .env
Set:
VITE_API_BASE_URL=http://<EC2_PUBLIC_IP>:8080

2. Frontend Dockerfile
Create frontend/Dockerfile:

FROM node:24-alpine
COPY . /opt/
WORKDIR /opt
RUN npm install && npm run build

RUN apk update && apk add apache2
RUN rm -rf /var/www/localhost/htdocs/*
RUN cp -rf dist/* /var/www/localhost/htdocs/
EXPOSE 80
CMD ["httpd", "-D", "FOREGROUND"]

3. Build frontend image
bash
Copy code
docker build -t frontend-app .
4. Run frontend container
docker<img width="1486" height="420" alt="Screenshot 2025-11-05 203849" src="https://github.com/user-attachments/assets/ff4cc41f-c2a6-482e-bd3d-65b325188665" /> run -d -p 80:80 frontend-app
<img width="1694" height="328" alt="Screenshot 2025-11-05 203026" src="https://github.com/user-attachments/assets/4c72d945-cfbd-498e-837e-5b69534a15ca" />

###  Final Step
bash
Open your browser and visit:
http://<YOUR_EC2_PUBLIC_IP>
You should now see your website live! 
<img width="1905" height="1011" alt="Screenshot 2025-11-05 202951" src="https://github.com/user-attachments/assets/0c5709a0-835a-4fed-9b0a-8fb22ee4198d" />
<img width="1486" height="420" alt="Screenshot 2025-11-05 203849" src="https://github.com/user-attachments/assets/ff4cc41f-c2a6-482e-bd3d-65b325188665" />
