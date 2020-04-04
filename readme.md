# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) AppServer
[![GoDoc](https://godoc.org/github.com/codemodify/systemkit-logging?status.svg)](https://godoc.org/github.com/codemodify/systemkit-appsever)
[![0-License](https://img.shields.io/badge/license-0--license-brightgreen)](https://github.com/codemodify/TheFreeLicense)
[![Go Report Card](https://goreportcard.com/badge/github.com/codemodify/systemkit-appsever)](https://goreportcard.com/report/github.com/codemodify/systemkit-appsever)
[![Test Status](https://github.com/danawoodman/systemservice/workflows/Test/badge.svg)](https://github.com/danawoodman/systemservice/actions)
![code size](https://img.shields.io/github/languages/code-size/codemodify/SystemKit?style=flat-square)

### The Missing Application Server in Go
#### Supported: Linux, Raspberry Pi, FreeBSD, Mac OS, Windows, Solaris

# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) Install
```go
go get github.com/codemodify/systemkit-appserver
```
# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) API

&nbsp;																| &nbsp;
---     															| ---
NewHTTPServer(`handlers`)                                           | ---
NewMixedServer(`servers`)                                           | ---
NewJSONServer(`handlers`)                                           | ---
NewSSHTunnelServer(`sshServerConfig`, `server`)                     | ---
NewWebSocketsServer(`handlers`)                                     | ---
Run(`ipPort`, `enableCORS`)                                         | ---
PrepareRoutes(`router`)                                             | ---
RunOnExistingListenerAndRouter(`listener`, `router`, `enableCORS`)  | ---
Write(`data`) (`int`, err`or)                                       | ---
runSSH(`connection` )                                               | ---
setupCommunication(`ws`, han`dler)                                  | ---
SendToAllPeers(`data`)                                              | ---
addPeer(`peer`)                                                     | ---
removePeer(`peer`)                                                  | ---


- If http://tomcat.apache.org provides Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies
- `AppServer` provides an alternative way to write in Go
    - API Services
    - Middleware
    - Web Apps Frameworks - analogy for NodeJS + Express

- For sample servers see `samples/sample.go` and `samples/sample.html`

# ![](https://fonts.gstatic.com/s/i/materialicons/bookmarks/v4/24px.svg) Usage
```go
package main

import (
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
	"sync"
	"time"

	logging "github.com/codemodify/systemkit-logging"

	jm "github.com/codemodify/systemkit-appserver"
)

func main() {

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