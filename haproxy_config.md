frontend http_front
bind \*:80

 <!-- Listen for incoming HTTP requests on port 80 on all interfaces -->

    default_backend aws_backends
    <!-- Forward all requests to the 'aws_backends' backend group -->

backend aws_backends

balance roundrobin

 <!-- Distribute requests evenly across the servers below -->

server nginx1 172.31.29.80:80 check

 <!-- First backend server with health checks enabled -->

server nginx2 172.31.26.80:80 check

 <!-- Second backend server with health checks enabled -->
