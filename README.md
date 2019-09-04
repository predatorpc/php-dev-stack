# php-dev-stack
Development stack for casual purposes of PHP programmers

# Which packages are included?
This bitch-pack includes the following stuff:
+ Nginx 1.17@80 port over Ubuntu
+ PHP-FPM@9000 port over Ubuntu + SupervisorD enabled on FPM and RabbitMQ Queue app
+ MongoDB (no shell) over Ubuntu
+ Redis + redis-client over Apline
+ RabbitMQ over Alpine
+ Clickhouse + client over Ubuntu
+ MySQL over Ubuntu
+ PHP MyAdmin over Ubuntu
+ Portainer docker manager for newbies

All containers have a general network to communicate without deps/links,
also you can access hosts(containers) via service name.

Default settings are placed in `./config` directory
Default aaplication directory is `./app`

# Installation

> Use the following to clone this to your side & get it up
`$ git clone https://github.com/predatorpc/php-dev-stack.git`

`$ cd php-dev-stack`

`$ docker-compose up -d`

Profit!
