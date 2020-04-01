# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) AppServer
[![GoDoc](https://godoc.org/github.com/codemodify/systemkit-logging?status.svg)](https://godoc.org/github.com/codemodify/systemkit-appsever)
[![0-License](https://img.shields.io/badge/license-0--license-brightgreen)](https://github.com/codemodify/TheFreeLicense)
[![Go Report Card](https://goreportcard.com/badge/github.com/codemodify/systemkit-appsever)](https://goreportcard.com/report/github.com/codemodify/systemkit-appsever)
[![Test Status](https://github.com/danawoodman/systemservice/workflows/Test/badge.svg)](https://github.com/danawoodman/systemservice/actions)
![code size](https://img.shields.io/github/languages/code-size/codemodify/SystemKit?style=flat-square)

#### Robust AppServer for Go.


# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) Install
```go
go get github.com/codemodify/systemkit-events
```

# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) API

&nbsp;										| &nbsp;
---             							| ---
---											| ---
---											| ---
---											| ---
---											| ---


# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) Usage
```go
func main() {
	logging.Init(logging.NewConsoleLogger())

	const port = 9999

	fmt.Println(fmt.Sprintf("curl localhost:%d/SayHello", port))
	fmt.Println(fmt.Sprintf("curl localhost:%d/EchoBack -X POST -d 'should-send-back-the-same'", port))
	fmt.Println(fmt.Sprintf("curl localhost:%d/MyRestEndopint -H \"Content-Type: application/json\" -X POST -d '{\"Field1\":\"val1\", \"Field2\": 0, \"Field3\": 1.0, \"Field4\": true}'", port))
	fmt.Println("ALSO: open the 'sample.html' to see data streaming")

	jm.NewMixedServer([]jm.IServer{
		jm.NewHTTPServer([]jm.HTTPHandler{
			jm.HTTPHandler{
				Route:   "/SayHello",
				Verb:    "GET",
				Handler: sayHelloRequestHandler,
			},
			jm.HTTPHandler{
				Route:   "/EchoBack",
				Verb:    "POST",
				Handler: echoBackRequestHandler,
			},
		}),
		jm.NewJSONServer([]jm.JSONHandler{
			jm.JSONHandler{
				Route:    "/MyRestEndopint",
				Template: &IncomingJson{},
				Handler:  jsonRequestHandler,
			},
		}),
		jm.NewWebSocketsServer([]jm.WebSocketsHandler{
			jm.WebSocketsHandler{
				Route:   "/StreamTelemetry",
				Handler: streamTelemetryRequestHandler,
			},
		}),
	}).Run(fmt.Sprintf(":%d", port), true)
}
```

>### `The Missing Application Server in Go`

- If http://tomcat.apache.org provides Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies
- `AppServer` provides an alternative way to write in Go
    - API Services
    - Middleware
    - Web Apps Frameworks - analogy for NodeJS + Express

- For sample servers see `samples/sample.go` and `samples/sample.html`
