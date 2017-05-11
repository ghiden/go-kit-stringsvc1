# Go kit: stringsvc1 with http method check

[Go kit](https://gokit.io)  
[stringsvc](https://gokit.io/examples/stringsvc.html)  

When I tested the stringsvc example with different method like GET, it just returned 500.
So here it is, the same example with method check.

```
go build -o server
./server
```

```
$ curl -v localhost:8080/uppercase
*   Trying ::1...
* Connected to localhost (::1) port 8080 (#0)
> GET /uppercase HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.43.0
> Accept: */*
>
< HTTP/1.1 405 Method Not Allowed
< Date: Thu, 11 May 2017 00:35:27 GMT
< Content-Length: 0
< Content-Type: text/plain; charset=utf-8
<
* Connection #0 to host localhost left intact
```

```go
func methodControl(method string, h http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		if r.Method == method {
			h.ServeHTTP(w, r)
		} else {
			http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
		}
	})
}
```

```go
http.Handle("/uppercase", methodControl("POST", uppercaseHandler))
```
