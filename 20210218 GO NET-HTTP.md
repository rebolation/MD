---
date: 2021.02.18
title: GO NET/HTTP
tag: go, net, http
---

# GO NET/HTTP

### GO NET/HTTP

[package http](https://golang.org/pkg/net/http/#pkg-index) 를 살펴보자.

### 요청

Go의 http 패키지에는 python의 requests나 nodejs의 request 모듈과 같은 기능을 하는 부분이 있다.

간단한 요청은 http.Get/Post 함수를 사용할 수 있다. 내부적으로 디폴트 클라이언트가 사용된다.

```go
func Get(url string) (resp *Response, err error)
func Head(url string) (resp *Response, err error)
func Post(url, contentType string, body io.Reader) (resp *Response, err error)
func PostForm(url string, data url.Values) (resp *Response, err error)
...
```

보다 세밀한 요청은 Client.Do/Get/Post 함수를 사용할 수 있다. Client 구조체의 CheckRedirect 필드를 설정해 리디렉션을 거부하거나, Request 구조체의 Header 필드를 설정해 HTTP 요청 헤더를 추가할 수 있다.

```go
type Client
    func (c *Client) CloseIdleConnections()
    func (c *Client) Do(req *Request) (*Response, error)
    func (c *Client) Get(url string) (resp *Response, err error)
    func (c *Client) Head(url string) (resp *Response, err error)
    func (c *Client) Post(url, contentType string, body io.Reader) (...)
    func (c *Client) PostForm(url string, data url.Values) (resp *Response, err error)
type Request
    func NewRequest(method, url string, body io.Reader) (*Request, error)
    ...
    func (r *Request) AddCookie(c *Cookie)
    func (r *Request) BasicAuth() (username, password string, ok bool)
    func (r *Request) Clone(ctx context.Context) *Request
    func (r *Request) Context() context.Context
    func (r *Request) Cookie(name string) (*Cookie, error)
    func (r *Request) Cookies() []*Cookie
    func (r *Request) FormFile(key string) (multipart.File, *multipart.FileHeader, error)
    func (r *Request) FormValue(key string) string
    func (r *Request) MultipartReader() (*multipart.Reader, error)
    ...
```

### 응답 - ListenAndServe와 Handler

브라우저 등으로부터 오는 요청에 대한 응답도 가능하다. 즉, 웹 서버 구현이 가능하다.

http.ListenAndServe 함수에서 시작해보자. Server 인스턴스를 만들고 Server.ListenAndServe 를 실행한다.

```go
// ListenAndServe listens on the TCP network address addr and then calls
// Serve with handler to handle requests on incoming connections.
// Accepted connections are configured to enable TCP keep-alives.
//
// The handler is typically nil, in which case the DefaultServeMux is used.
//
// ListenAndServe always returns a non-nil error.
func ListenAndServe(addr string, handler Handler) error {
    server := &Server{Addr: addr, Handler: handler}
    return server.ListenAndServe()
}
```

Server.ListenAndServe는 net.Listen을 실행하고 Server.Serve를 실행한다.

```go
// ListenAndServe listens on the TCP network address srv.Addr and then
// calls Serve to handle requests on incoming connections.
// Accepted connections are configured to enable TCP keep-alives.
//
// If srv.Addr is blank, ":http" is used.
//
// ListenAndServe always returns a non-nil error. After Shutdown or Close,
// the returned error is ErrServerClosed.
func (srv *Server) ListenAndServe() error {
    ...
    ln, err := net.Listen("tcp", addr)
    ...
    return srv.Serve(ln)
}
```

Server.serve는 루프에서 Listener.Accept, Server.newConn를 실행하고 고루틴 conn.serve를 실행한다.

```go
// Serve accepts incoming connections on the Listener l, creating a
// new service goroutine for each. The service goroutines read requests and
// then call srv.Handler to reply to them.
//
// HTTP/2 support is only enabled if the Listener returns *tls.Conn
// connections and they were configured with "h2" in the TLS
// Config.NextProtos.
//
// Serve always returns a non-nil error and closes l.
// After Shutdown or Close, the returned error is ErrServerClosed.
func (srv *Server) Serve(l net.Listener) error {
    ...
    l = &onceCloseListener{Listener: l}
    ...
    for {
        rw, err := l.Accept()
        ...
        c := srv.newConn(rw)
        ...
        go c.serve(connCtx)
    }
}
```

conn.serve는 tls.Conn.Handshake, conn.readRequest 등을 실행하고, serverHandler.ServeHTTP를 호출한다.

```go
// Serve a new connection.
func (c *conn) serve(ctx context.Context) {
    ...
        if err := tlsConn.Handshake(); err != nil {
    ...
    for {
        w, err := c.readRequest(ctx)
        ...
        // HTTP cannot have multiple simultaneous active requests.[*]
        // Until the server replies to this request, it can't read another,
        // so we might as well run the handler in this goroutine.
        // [*] Not strictly true: HTTP pipelining. We could let them all process
        // in parallel even if their responses need to be serialized.
        // But we're not going to implement HTTP pipelining because it
        // was never deployed in the wild and the answer is HTTP/2.
        serverHandler{c.server}.ServeHTTP(w, w.req)
        ...
        w.finishRequest()
        ...
    }
}
```

serverHandler.ServeHTTP는 Handler.ServeHTTP를 호출하는데, Handler는 인터페이스이다.

```go
func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request) {
    handler := sh.srv.Handler
    ...
    handler.ServeHTTP(rw, req)
}
```

FileServer가 반환하는 fileHandler가 Handler 인터페이스를 구현한다. fileHandler.ServeHTTP는 serveFile을 호출하고 serveFile은 serveContent를 호출하는데 여기에서 ResponseWriter를 사용하여 응답을 보낸다.

```go
func FileServer(root FileSystem) Handler {
    return &fileHandler{root}
}
func (f *fileHandler) ServeHTTP(w ResponseWriter, r *Request) {
    ...
    serveFile(w, r, f.root, path.Clean(upath), true)
}
func serveFile(w ResponseWriter, r *Request, fs FileSystem, name string, redirect bool) {
    const indexPage = "/index.html"
    ...
    f, err := fs.Open(name)
    ...
    serveContent(w, r, d.Name(), d.ModTime(), sizeFunc, f)
}
func serveContent(w ResponseWriter, r *Request, ..., content io.ReadSeeker) {
    ...
    code := StatusOK    
    ...
    ctypes, haveType := w.Header()["Content-Type"]
        ...
        w.Header().Set("Content-Type", ctype)
    ...
    var sendContent io.Reader = content
    ...
        case len(ranges) > 1:
            ...
            pr, pw := io.Pipe()
            mw := multipart.NewWriter(pw)
            ...
            sendContent = pr
            ...
            go func() {
                ...
                    part, err := mw.CreatePart(ra.mimeHeader(ctype, size))
                    ...
                    if _, err := io.CopyN(part, content, ra.length); err != nil {                            ...
                ...
            }()
        }
    ...
    w.WriteHeader(code)
    ...
    if r.Method != "HEAD" {
        io.CopyN(w, sendContent, sendSize)
    }
}
```

결국, http.ListenAndServe에 Handler.ServeHTTP 구현체를 전달하면 요청이 들어올 때 ServeHTTP를 호출하여 응답을 보내는 구조로 보인다. Handler를 전달하기만 하면 아래와 같이 간단한 서버를 만들 수도 있다.

```go
http.ListenAndServe(":8080", http.FileServer(http.Dir("./public")))
```

### 응답 - Handle과 HandleFunc

http.Handle/HandleFunc를 사용하여 url 패턴에 따라 여러가지 핸들러를 등록할 수 있다. 핸들을 위해 ResponseWriter와 Request를 사용한다.

```go
// Handle registers the handler for the given pattern in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func Handle(pattern string, handler Handler) { DefaultServeMux.Handle(pattern, handler) }

// HandleFunc registers the handler function for the given pattern
// in the DefaultServeMux.
// The documentation for ServeMux explains how patterns are matched.
func HandleFunc(pattern string, handler func(ResponseWriter, *Request)) {
    DefaultServeMux.HandleFunc(pattern, handler)
}
// A ResponseWriter interface is used by an HTTP handler to construct an HTTP response.
// A ResponseWriter may not be used after the Handler.ServeHTTP method has returned.
type ResponseWriter interface {    
    Header() Header // returns the header map that will be sent by WriteHeader.    
    Write([]byte) (int, error) // writes the data to the conn as part of an HTTP reply.
    WriteHeader(statusCode int) // sends an HTTP response header with the status code.
}

// A Request represents an HTTP request received by a server or to be sent by a client.
type Request struct {
    Method string
    URL *url.URL
    Proto      string // "HTTP/1.0"
    ProtoMajor int    // 1
    ProtoMinor int    // 0
    Header Header
    Body io.ReadCloser
    GetBody func() (io.ReadCloser, error)
    ContentLength int64
    TransferEncoding []string
    Close bool
    Host string
    Form url.Values
    PostForm url.Values
    MultipartForm *multipart.Form
    Trailer Header
    RemoteAddr string
    RequestURI string
    TLS *tls.ConnectionState
    Cancel <-chan struct{}
    Response *Response
    ctx context.Context
}
http.Handle("/", http.FileServer(http.Dir("./public")))
http.HandleFunc("/hello", func(rw http.ResponseWriter, r *http.Request) {
    fmt.Fprint(rw, "Hello World")
})
http.ListenAndServe(":3000", nil)
```

이 밖에도 에러처리를 위한 http.Error, 리디렉션을 위한 http.Redirect 등이 있다.

### 응답 - URL Query, Body

Request로부터 요청 URL 파라미터와 Body 데이터를 가져올 수 있다. JSON을 받으려면 사용자 정의 구조체와 json.Decoder.Decode를 사용한다. JSON으로 응답할 때는 구조체를 JSON으로 변환하는 json.Marshal과 http.Header.Add를 사용한다.

```go
type Msg struct {
    Test string
}

http.HandleFunc("/", func(rw http.ResponseWriter, r *http.Request) {
    url := r.URL.Query().Get("test") // REQUEST URL QUERY

    var m Msg
    json.NewDecoder(r.Body).Decode(&m) // REQUEST BODY (JSON)

    m.Test = url + " " + m.Test
    data, _ := json.Marshal(m)
    rw.Header().Add("content-type", "application/json")
    fmt.Fprint(rw, string(data)) // RESPONSE (JSON)
})
curl "http://localhost:3000/?test=hello" -d {\"test\":\"world\"}
{"Test":"hello world"}
```

### 참고

- [package http](https://golang.org/pkg/net/http/#pkg-index)
- [Golang으로 웹서버 만들기](https://velog.io/@soosungp33/Golang으로-웹서버-만들기)
- [김정환블로그 - Go net/http 패키지](https://jeonghwan-kim.github.io/dev/2019/02/07/go-net-http.html)

