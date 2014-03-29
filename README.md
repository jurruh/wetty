wetty = Web + tty
-----------------

Terminal over http. Wetty is an alternative to ajaxterm/anyterm but much better than them because wetty uses ChromeOS' javascript terminal emulator (hterm) which is a full fledged implementation of terminal emulation. Also it uses websockets instead of Ajax and hence better response time.

![Wetty](/terminal.jpg?raw=true)

Install
-------

  `git clone https://github.com/krishnasrinivas/wetty`
  
  `cd wetty`

  `npm install`

Run on http:
-----------
  `node app.js -p 3000`

If you run it as root it will launch /bin/login (where you can specify the username), else it will launch ssh to localhost with the node process' user as the ssh login username.

Run on https:
------------
Always use SSL! If you don't have ssl certificates from CA you can create a self signed certificate using this command:

  `openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 30000 -nodes`

And then run:

  `node app.js --sslkey key.pem --sslcert cert.pem -p 3000`


Run wetty behind nginx:
----------------------

Put the following config in nginx's conf:

    location /wetty {
	    proxy_pass http://127.0.0.1:3000/wetty;
	    proxy_http_version 1.1;
	    proxy_set_header Upgrade $http_upgrade;
	    proxy_set_header Connection "upgrade";
	    proxy_read_timeout 43200000;

	    proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	    proxy_set_header Host $http_host;
	    proxy_set_header X-NginX-Proxy true;
    }

In the browser you have to use: 'http://yourserver.com/wetty'. Note that if your nginx is configured for https you should run wetty without ssl.
