glfont for gles2


# Example:

```go
package main

import (
	// "fmt"
	"log"
	"runtime"
	"fmt"
	"bytes"
	"github.com/gobuffalo/packr"

	"github.com/go-gl/gl/v2.1/gl"
	"github.com/go-gl/glfw/v3.1/glfw"
	// "github.com/nullboundary/glfont"
	"github.com/liamg/aminal/glfont"

)

const windowWidth = 500
const windowHeight = 400

func init() {
	runtime.LockOSThread()
}

func loadFont(name string,scale float32, windowWidth int, windowHeight int) (*glfont.Font, error) {
	box := packr.NewBox("/Users/evil/dev/go/aminal/gui/packed-fonts/")
	fontBytes, err := box.Find(name)
	if err != nil {
		return nil, fmt.Errorf("packaged font '%s' could not be read: %s", name, err)
	}

	font, err := glfont.LoadFont(bytes.NewReader(fontBytes), scale, windowWidth, windowHeight)
	if err != nil {
		return nil, fmt.Errorf("font '%s' failed to load: %v", name, err)
	}

	return font, nil
}

func main() {

	if err := glfw.Init(); err != nil {
		log.Fatalln("failed to initialize glfw:", err)
	}
	defer glfw.Terminate()
	window, _ := glfw.CreateWindow(int(windowWidth), int(windowHeight), "glfontExample", nil, nil)

	window.MakeContextCurrent()
	glfw.SwapInterval(1)
	
	if err := gl.Init(); err != nil { 
		panic(err)
	}

	//load font (fontfile, font scale, window width, window height
	font, err := loadFont("Hack Regular Nerd Font Complete.ttf", 18, windowWidth, windowHeight)
	if err != nil {
		log.Panicf("LoadFont: %v", err)
	}

	gl.Enable(gl.DEPTH_TEST)
	gl.DepthFunc(gl.LESS)
	gl.ClearColor(0.0, 0.0, 0.0, 0.0)

	for !window.ShouldClose() {
		gl.Clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT)

     //set color and draw text
		font.SetColor(1.0, 1.0, 1.0, 1.0) //r,g,b,a font color
		font.Print(4, 100, "hello,world") //x,y,scale,string,printf args

		window.SwapBuffers()
		glfw.PollEvents()

	}
}
```

#### Contributors
* [evilbinary](https://github.com/evilbinary)
* [kivutar](https://github.com/kivutar)
