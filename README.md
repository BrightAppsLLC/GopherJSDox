# GopherJS Dox
- `go get github.com/gopherjs/gopherjs`
- `gopherjs build` / `gopherjs build -m`
- `gopherjs serve`
- Recomendations
    - Use `-m` when building
    - Enable `gzip` compression on web server
    - Use `int` instead of (u)int8/16/32/64
    - Use `float64` instead of `float32`

# JS Analogy - alert()
- JS: `alert("message")`
- Gopher: `js.Global.Call("alert", "message")`
- DOM Binding: `dom.GetWindow().Alert("message")`

# JS Analogy - getElementById()
- JS: `element = document.getElementById("container")`
- Gopher: `element := js.Global.Get("document").Call("getElementById", "container")`
- DOM Binding: `element := dom.GetWindow().Document().GetElementByID("container")`

# JS Analogy - querySelector()
- JS: `element = document.querySelector("#thumbnails .active")`
- Gopher: `element := js.Global.Get("document").Call("querySelector", "#thumbnails .active")`
- DOM Binding: `element := dom.GetWindow().Document().QuerySelector("#thumbnails .active")`

# Aliasing
- `var JS = js.Global`
- `var D = dom.GetWindow().Document()`

# JS Analogy - Change CSS
- JS:
```javascript
element = document.getElementById("container");
element.style.display = "none";
```
- Gopher:
```go
var JS = js.Global
element := JS.Get("document").Call("getElementById", "container")
element.Get("style").Set("display") = "none"
```
- DOM Binding:
```go
var D = dom.GetWindow().Document()
element := D.GetElementByID("container")
element.Style().SetProperty("display", "none")
```

# XHR
- `honnef.co/go/js/xhr`
- `xhr.Send("POST", "/lowercase-text", textBytes)`
- `textBytes, err := json.Marshal(textToLowercase.Value)`

# Exchange Binary Data - Client <-> Server
- `encoding/gob`
- Client: 
```go
var carsDataBuffer bytes.Buffer
enc := gob.NewEncoder(&carsDataBuffer)
enc.Encode(cars)

xhrResponse, err := xhr.Send("POST", "/cars-data", carsDataBuffer.Bytes())
```
- Server: 
```go
var cars []model.Car
var carsDataBuffer bytes.Buffer

dec := gob.NewDecoder(&carsDataBuffer)
body, err := ioutil.ReadAll(r.Body)
carsDataBuffer = *bytes.NewBuffer(body)
err = dec.Decode(&cars)
```

# Websocket
- Server
    - `github.com/gorilla/websocket`
    - `c, err := upgrader.Upgrade(w, r, nil)` - converts the connection to a WebSocket
    - `mt, message, err := c.ReadMessage()`
    - `err = c.WriteMessage(mt, message)`
- Client
    - `github.com/gopherjs/websocket`
    - `AddEventListener("beforeunload"`
    - `c, err := websocket.Dial(u.String())`
    - `c.Write([]byte(t.String()))`
    - `n, err = c.Read(message)`

# Kick - automate the manual building of Go to JS (GopherJS)
- `kick --appPath=. --gopherjsAppPath=./client --mainSourceFile=main.go`

# SPA
- `IsoKit` -> `https://github.com/IsomorphicGo/isokit`
- `env := common.Env{}`
- 
```
r := isokit.NewRouter()
r.Handle("/feed", handlers.FeedHandler(&env))
r.Handle("/friends", handlers.FriendsHandler(&env))
r.Handle("/myprofile", handlers.MyProfileHandler(&env))
r.Listen()
```
