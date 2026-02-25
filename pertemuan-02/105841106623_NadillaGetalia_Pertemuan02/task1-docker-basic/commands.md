PS C:\Users\ASUS> docker pull nginx:alpine
alpine: Pulling from library/nginx
5e7756927bef: Pull complete
589002ba0eae: Pull complete
399d0898a94e: Pull complete
6d397a54a185: Pull complete
bca5d04786e1: Pull complete
955a8478f9ac: Pull complete
3e2c181db1b0: Pull complete
6b7b6c7061b7: Pull complete
Digest: sha256:1d13701a5f9f3fb01aaa88cef2344d65b6b5bf6b7d9fa4cf0dca557a8d7702ba
Status: Downloaded newer image for nginx:alpine
docker.io/library/nginx:alpine
PS C:\Users\ASUS> docker run -d --name web-praktikum -p 8080:80 nginx:alpine
627d1c24a3f2eef7514dfbdbd48173f0c416ed90d36a9a94ac4f81e5d5ebc87f
PS C:\Users\ASUS> docker logs web-praktikum
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/02/25 03:05:28 [notice] 1#1: using the "epoll" event method
2026/02/25 03:05:28 [notice] 1#1: nginx/1.29.5
2026/02/25 03:05:28 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/02/25 03:05:28 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2026/02/25 03:05:28 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/02/25 03:05:28 [notice] 1#1: start worker processes
2026/02/25 03:05:28 [notice] 1#1: start worker process 30
2026/02/25 03:05:28 [notice] 1#1: start worker process 31
2026/02/25 03:05:28 [notice] 1#1: start worker process 32
2026/02/25 03:05:28 [notice] 1#1: start worker process 33
2026/02/25 03:05:28 [notice] 1#1: start worker process 34
2026/02/25 03:05:28 [notice] 1#1: start worker process 35
2026/02/25 03:05:28 [notice] 1#1: start worker process 36
2026/02/25 03:05:28 [notice] 1#1: start worker process 37
2026/02/25 03:05:28 [notice] 1#1: start worker process 38
2026/02/25 03:05:28 [notice] 1#1: start worker process 39
2026/02/25 03:05:28 [notice] 1#1: start worker process 40
2026/02/25 03:05:28 [notice] 1#1: start worker process 41
2026/02/25 03:05:28 [notice] 1#1: start worker process 42
2026/02/25 03:05:28 [notice] 1#1: start worker process 43
2026/02/25 03:05:28 [notice] 1#1: start worker process 44
2026/02/25 03:05:28 [notice] 1#1: start worker process 45
172.17.0.1 - - [25/Feb/2026:03:05:38 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36" "-"
172.17.0.1 - - [25/Feb/2026:03:05:38 +0000] "GET /favicon.ico HTTP/1.1" 404 555 "http://localhost:8080/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36" "-"
2026/02/25 03:05:38 [error] 30#30: *1 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost:8080", referrer: "http://localhost:8080/"
PS C:\Users\ASUS> docker exec -it web-praktikum sh
/ # ls -la /usr/share/nginx/html
total 16
drwxr-xr-x    2 root     root          4096 Feb  4 23:53 .
drwxr-xr-x    3 root     root          4096 Feb  4 23:53 ..
-rw-r--r--    1 root     root           497 Feb  4 20:18 50x.html
-rw-r--r--    1 root     root           615 Feb  4 20:18 index.html
/ # cat /etc/nginx/nginx.conf

user  nginx;
worker_processes  auto;

error_log  /var/log/nginx/error.log notice;
pid        /run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
/ # exit
PS C:\Users\ASUS> docker stop web-praktikum
web-praktikum
PS C:\Users\ASUS> docker rm web-praktikum
web-praktikum