# Deployment

- Signup on AWS 
- Launch instance
- chmod 400 <secret>.pem
- ssh -i "localhost-secret.pem" ubuntu@ec2-54-163-129-146.compute-1.amazonaws.com
- Install Node version 23.6.1
- Git clone
- Frontend    
    - npm install  -> dependencies install
    - npm run build
    - sudo apt update
    - sudo apt install nginx
    - sudo systemctl start nginx
    - sudo systemctl enable nginx
    - Copy code from dist(build files) to /var/www/html/
    - sudo scp -r dist/* /var/www/html/
    - Enable port :80 of your instance
- Backend
    - updated DB password
    - allowed ec2 instance public IP on mongodb server
    - npm install pm2 -g
    - pm2 start npm --name "localhost-backend" -- start
    - pm2 logs
    - pm2 list, pm2 flush <name> , pm2 stop <name>, pm2 delete <name>
    - config nginx - /etc/nginx/sites-available/default
    - restart nginx - sudo systemctl restart nginx
    - Modify the BASEURL in frontend project to "/api"


# Ngxinx config:

    Frontend = http://43.204.96.49/
    Backend = http://43.204.96.49:7777/

    Domain name = localhosts.club => 43.204.96.49

    Frontend = localhosts.club
    Backend = localhosts.club:7777 => localhosts.club/api

    nginx config : 

    server_name 43.204.96.49;

    location /api/ {
        proxy_pass http://localhost:7777/;  # Pass the request to the Node.js app
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }