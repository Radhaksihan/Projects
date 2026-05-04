# Nginx Docker Task

## Step 1: Run nginx container
docker run -d --name mynginx -p 8080:80 nginx

## Step 2: Verify container
docker ps

Output:
Container was running successfully.

## Step 3: Access application
Opened:
http://localhost:8080

Verified nginx default page.

## Step 4: Login into container
docker exec -it mynginx bash

Checked:
- pwd
- ls
- nginx config files

## Step 5: Stop container
docker stop mynginx

## Step 6: Restart container
docker start mynginx

## Step 7: Remove container
docker rm -f mynginx
