<h1>Nginx Load balancer for a dockerized Node.js</h1>

<pre>
$ ~/nginx.conf

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