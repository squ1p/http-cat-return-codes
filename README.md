# About
This is a simple implementation of using some of the [http.cat](https://http.cat) HTTP return codes in HAproxy. It simply displays the corresponding picture on a black background.

Note that it uses hyperlinks to http.cat, so it could be replaced by any other link. If you're a dog person, for example, you could go for [HTTP Status Dogs !](https://httpstatusdogs.com/)

# Using
Simply copy the folder containing the pages into your haproxy config folder:
```bash
cp -r http-cat-errors /etc/haproxy/
```

Add the following section to your haproxy config:
```
http-errors kitties
	errorfile 400 /etc/haproxy/http-cat-errors/400.http
	errorfile 401 /etc/haproxy/http-cat-errors/401.http
	errorfile 403 /etc/haproxy/http-cat-errors/403.http
	errorfile 404 /etc/haproxy/http-cat-errors/404.http
	errorfile 408 /etc/haproxy/http-cat-errors/408.http
	errorfile 500 /etc/haproxy/http-cat-errors/500.http
	errorfile 502 /etc/haproxy/http-cat-errors/502.http
	errorfile 503 /etc/haproxy/http-cat-errors/503.http
	errorfile 504 /etc/haproxy/http-cat-errors/504.http
```

...and finally, add these lines to your frontend so that HAProxy will intercept the errors coming from its backend servers and replace them with its own:
```
	errorfiles kitties
	http-response return status 400 default-errorfiles if { status 400 }
	http-response return status 401 default-errorfiles if { status 401 }
	http-response return status 403 default-errorfiles if { status 403 }
	http-response return status 404 default-errorfiles if { status 404 }
	http-response return status 408 default-errorfiles if { status 408 }
	http-response return status 500 default-errorfiles if { status 500 }
	http-response return status 502 default-errorfiles if { status 502 }
	http-response return status 503 default-errorfiles if { status 503 }
	http-response return status 504 default-errorfiles if { status 504 }
```

Check everything and reload HAProxy:
```
haproxy -c -V -f /etc/haproxy/haproxy.cfg
systemctl reload haproxy
```

And voilà ! 
