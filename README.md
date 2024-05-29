# ok-API

Lightweight and minimalistic HTTP monitor. 

* ☎️ Built-in plugins to setup free Email/Slack/Telegram notifications
* 🪧 Agents installed using **simple code snippets**. Options: Docker/Compose/Bash+curl+crontab.
* 🏙 Allows to monitor as many hosts as needed
* ⏰️ Configurable monitoring interval
* 🌐 HTTP / HTTPS Monitor, status code check, webpage keywords existence, basic auth, notifications
* 🔒 HTTPS SSL check with expiration reminder (default is 14 days prior to expire, configurable)

# Preview

Available notification plugins:

![image](https://user-images.githubusercontent.com/1838656/196889594-5a405ca0-0608-4a39-8a42-b62d74055f99.png)

HTTP(s) Monitor setup:

<img src="https://user-images.githubusercontent.com/1838656/196887413-14055152-4114-42e8-841f-cd5ec7c33e50.png" width="500"/>



# Installation

First you need to run ok-API Web Server. Web-server itself, when will be started, will give you clear guide how to add agents via own Web UI. 

You can use any host with public IP. You are responsible for setting up HTTPS.

For HTTPS you can use anything, most simple options:
- Nginx or Traefik with connected external certificate or free Let's Encrypt certificate
- Free Cloudflare CDN which terminates SSL and gives additional layer of security.


Now you should proxy https://subdomain.yourdomain.com to serve requests from 127.0.0.1:8007.

Example Nginx Proxy:

```
server {
  listen 443;
  server_name subdomain.yourdomain.com;
  ssl_certificate     wildcard.crt;
  ssl_certificate_key   wildcard.key;

  charset utf-8;
  client_max_body_size 75M;

  gzip on;
  gzip_disable "msie6";
  gzip_vary on;
  gzip_proxied any;
  gzip_comp_level 8;
  gzip_buffers 16 8k;
  gzip_http_version 1.1;
  gzip_min_length 256;
  gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/vnd.ms-fontobject application/x-font-ttf font/opentype image/svg+xml image/x-icon;

  location / {
      proxy_read_timeout 220s;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_redirect off;
      proxy_pass http://127.0.0.1:8007;
  }
}
```


* After that Use https://subdomain.yourdomain.com to view as admin and add new agents


# Plugin development

Every plugin is a standalone .js in ESM format (with exports/imports, CommonJS with require is not supported). File should have a defined structure. Use `server/src/plugins/slack.js` for a Hello-World example. It is well documented

Currently plugin file should be placed into `server/src/plugins` directory.

If you want to bundle some dedicated node modules then you need to bundle them using webpack or other bundler which supports ESM outputs. Please see `server/src/plugins/gmail.src/gmail.src.js` - it bundels node-mailer into plugin.


# Development


Default local credentials:
- admin:123456


First, start back:

```
cd server
npm ci
npm start
```

Then start front:

```
cd frontend 
npm ci
npm run dev
```

