1. Using Domain manager, create a domain/subdomain e.g demo.abc.com and point the A record to your `VPS`

2. Using VPS, setup the following configuration for Nginx proxy that will direct the trafic to a specific port e.g 5000:
```
server {
	server_name  demo.abc.com;
	location / {
		proxy_pass http://127.0.0.1:5000;
	}
}
```

3. On your machine, connect to the VPS using a SSL tunnel by `mapping` your `localhost port` e.g 3000 against `VPS mapped port` e.g 5000.
```
ssh -R 5000:localhost:3000 user@demo.abc.com
```

4. Your website is now live at http://demo.abc.com. To close the demo, simply disconnect from the VPS server
