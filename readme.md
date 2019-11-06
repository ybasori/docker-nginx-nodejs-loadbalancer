<h1>Nginx Load balancer for a dockerized Node.js</h1>

<h2>Requirement</h2>
<ul>
    <li>Docker</li>
    <li>Docker Compose</li>
</ul>

<pre>
$ ~# git clone https://github.com/ybasori/docker-nginx-nodejs-loadbalancer
$ ~# cd docker-nginx-nodejs-loadbalancer 
</pre>

<h2>Configure Nginx</h2>
<pre>
$ ~/docker-nginx-nodejs-loadbalancer/nginx.conf

upstream loadbalance {
    least_conn;
    server 192.168.1.58:5001;
    server 192.168.1.58:5002;
}

server {
    location / {
        proxy_pass http://loadbalance;
    }
}

"192.168.1.58" change to your local ip address

</pre>

<h2>Configure Nodejs</h2>
<pre>
$ ~/docker-nginx-nodejs-loadbalancer# cd nodejs
$ ~/docker-nginx-nodejs-loadbalancer/nodejs# npm install
</pre>

<h2>Start</h2>
<pre>
$ ~/docker-nginx-nodejs-loadbalancer# docker-compose up
</pre>
<p>Run http://localhost:5000 or http://ip_address:5000 on your favorite browser</p>

<h2>If you want to have one nodejs in your docker-compose.yaml but still wants to run more than one<h2>

<pre>
$ ~/docker-nginx-nodejs-loadbalancer/docker-compose.yaml

version: "3.3"

services:

  nginx:
    image: nginx:latest
    ports:
      - "5000:80"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf

  nodejs:
    image: node:latest
    ports:
      - '5001:5000'
    environment: 
      NODE_ENV: production
    working_dir: /home/app
    restart: always
    volumes:
      - ./nodejs:/home/app
    command: bash -c "npm install && node index.js"
</pre>

<p>Run 3 nodejs service</p>
<pre>

$ ~/docker-nginx-nodejs-loadbalancer# docker-compose up --scale nodejs=3

</pre>