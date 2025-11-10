# Easycrud-docker-rds
my-git--docker--rds
###Documentation###
##1.Purpose##: Deploy  (frontend, backend, database) inside Docker containers, with minimal infrastructure friction.
#Tech stack#
#Frontend#:Node.js + npm
#Backend# : Java + Maven
#Database#- AWS EC2 (t2.medium)
## Architecture Diagram# :AWS EC2 (t2.large)
bash
               ┌─── ────────────────────┐
               │        Frontend        │
               │   (Node.js + npm)      │
               │  Container : Port 80   │
               └─────────────┬──────────┘
                             │
                             ▼
               ┌────────────────────────┐
               │        Backend         │
               │ (Java + Maven + JAR)   │
               │  Container : Port 8080 │
               └─────────────┬──────────┘
                             │
                             |
                             ▼
               ┌──────────    ──────────┐
               │        Database        │
               │    (MySQL/MariaDB)     │
               │  Container : Port 3306 │
               └────────────────────────┘
         ##  Tech Stack  
         
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
