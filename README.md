CI/CD Pipeline Flow
Developer → Git Push → CI Pipeline Runs → Build → Test → Deploy → Production

Step-by-Step CI/CD Process
Step 1: Create a Project
Example: Node.js App
mkdir ci-cd-demo
cd ci-cd-demo
npm init -y
npm install express
Create index.js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
 res.send('CI/CD Pipeline Working!');
});

app.listen(3000, () => console.log('Server running on port 3000'));

Step 2: Add Test Script
Install testing tool:
npm install jest --save-dev
Update package.json
"scripts": {
 "test": "jest"
}
Create test file app.test.js
test('Sample test', () => {
 expect(1 + 1).toBe(2);
});

Step 3: Push Code to GitHub
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/your-repo.git
git push -u origin main

Step 4: Create CI/CD Pipeline (GitHub Actions)
Create folder:
.github/workflows/
Create file:
ci-cd.yml

Step 5: Write CI/CD YAML File
name: CI/CD Pipeline

on:
 push:
   branches:
     - main

jobs:
 build-test-deploy:
   runs-on: ubuntu-latest

   steps:
     - name: Checkout Code
       uses: actions/checkout@v3

     - name: Setup Node.js
       uses: actions/setup-node@v3
       with:
         node-version: '18'

     - name: Install Dependencies
       run: npm install

     - name: Run Tests
       run: npm test

     - name: Build Step
       run: echo "Build completed"

     - name: Deploy Step
       run: echo "Deploying application..."

Step-by-Step Breakdown
🔹 1. Trigger
on:
 push:
   branches:
     - main
👉 Runs pipeline when code is pushed to main

🔹 2. Checkout Code
uses: actions/checkout@v3
👉 Pulls your repo into the runner

🔹 3. Setup Environment
uses: actions/setup-node@v3
👉 Installs Node.js

🔹 4. Install Dependencies
run: npm install

🔹 5. Run Tests
run: npm test

🔹 6. Build
run: echo "Build completed"

🔹 7. Deploy
run: echo "Deploying..."
👉 Replace this with real deployment (AWS, Docker, etc.)

Step 6: Real Deployment Example (Docker)
Create Dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "index.js"]

Update CI/CD for Docker
- name: Build Docker Image
 run: docker build -t my-app .

- name: Run Container
 run: docker run -d -p 3000:3000 my-app

Step 7: Deployment to AWS EC2 (Example)
- name: Deploy to EC2
 run: |
   ssh user@your-ec2-ip "
     cd /app &&
     git pull &&
     npm install &&
     pm2 restart all
   "

 Final Pipeline Flow
1. Developer pushes code
2. GitHub Actions triggered
3. Code checkout
4. Dependencies installed
5. Tests executed
6. Build created
7. Docker image built (optional)
8. Deployment to server/cloud

