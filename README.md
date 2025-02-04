
[中文](README.zh-CN.md)   | English    

### GoVCL

Cross-platform Golang GUI library, The core binding is [liblcl](https://github.com/ying32/liblcl), a common cross-platform GUI library created by [Lazarus](https://www.lazarus-ide.org/).    

**GoVCL is a native GUI library, not based on HTML, let alone DirectUI library, everything is practical.**       

**Full name: `Go Language Visual Component Library`**    

*govcl minimum requirement is go1.9.2.*    

[Screenshots](https://z-kit.cc/en/screenshot.html) | 
[WIKI(Chinese)](https://gitee.com/ying32/govcl/wikis/pages) | 
[What's-new(Chinese)](https://z-kit.cc/en/changelog.html) 

----

### Ⅰ. Support Platform    
**Windows** | **Linux** | **macOS**  

> If you want to support linux arm and linux 32bit, you need to compile the corresponding liblcl binary.   

### Ⅱ. Pre-compiled GUI library binary download ([source code](https://github.com/ying32/liblcl))     
[![liblcl](https://img.shields.io/github/downloads/ying32/govcl/latest/liblcl-2.2.3.zip.svg)](https://github.com/ying32/govcl/releases/download/v2.2.3/liblcl-2.2.3.zip)  

### Ⅲ. UI Designer(Two options)  

* 1、 Easy UI designer (single-page design, suitable for those who do not want to install Lazarus, and the project is not too complicated)      

[![GoVCLDesigner.win](https://img.shields.io/github/downloads/ying32/govcl/latest/GoVCLDesigner-win-1.2.0.zip.svg)](https://github.com/ying32/govcl/releases/download/v2.2.3/GoVCLDesigner-win-1.2.0.zip)  

**Note: This UI designer is no longer updated, but it does not affect use.**  

* 2. res2go IDE plugin source code（[source code](https://github.com/ying32/res2go-ide-plugin)）  

**How to use: [Installation method](https://gitee.com/ying32/govcl/wikis/pages?sort_id=2645001&doc_id=102420)**   

> Note: Designed in Lazarus, code written in Golang.  
 
### Ⅳ. usage: 

#### Step 1: Get the govcl code  

> go get -u github.com/ying32/govcl    

*Note: You can also use go module mode, configure in go.mod, such as: `github.com/ying32/govcl v2.2.3+incompatible`.*  

#### Step 2: Write the code

* Method 1(Use Lazarus to design the GUI. recommend): 

```golang
package main


import (
   // Do not reference this package if you use custom syso files
   _ "github.com/ying32/govcl/pkgs/winappres"
   "github.com/ying32/govcl/vcl"
)

type TMainForm struct {
    *vcl.TForm
    Btn1     *vcl.TButton
}

type TAboutForm struct {
    *vcl.TForm
    Btn1    *vcl.TButton
}

var (
    mainForm *TMainForm
    aboutForm *TAboutForm
)

func main() {
    vcl.Application.Initialize()
    vcl.Application.SetMainFormOnTaskBar(true)
    vcl.Application.CreateForm(&mainForm)
    vcl.Application.CreateForm(&aboutForm)
    vcl.Application.Run()
}

// -- TMainForm

func (f *TMainForm) OnFormCreate(sender vcl.IObject) {
    
}

func (f *TMainForm) OnBtn1Click(sender vcl.IObject) {
    aboutForm.Show()
}

// -- TAboutForm

func (f *TAboutForm) OnFormCreate(sender vcl.IObject) {
 
}

func (f *TAboutForm) OnBtn1Click(sender vcl.IObject) {
    vcl.ShowMessage("Hello!")
}
```
**Method 1 needs to be used in conjunction with the res2go tool.**  


* Method 2(Pure code, imitating the way of FreePascal class):  

```golang
package main


import (
   // Do not reference this package if you use custom syso files
   _ "github.com/ying32/govcl/pkgs/winappres"
   "github.com/ying32/govcl/vcl"
)

type TMainForm struct {
    *vcl.TForm
    Btn1     *vcl.TButton
}

type TAboutForm struct {
    *vcl.TForm
    Btn1    *vcl.TButton
}

var (
    mainForm *TMainForm
    aboutForm *TAboutForm
)

func main() {
    vcl.RunApp(&mainForm, &aboutForm)
}

// -- TMainForm

func (f *TMainForm) OnFormCreate(sender vcl.IObject) {
    f.SetCaption("MainForm")
    f.Btn1 = vcl.NewButton(f)
    f.Btn1.SetParent(f)
    f.Btn1.SetBounds(10, 10, 88, 28)
    f.Btn1.SetCaption("Button1")
    f.Btn1.SetOnClick(f.OnBtn1Click)  
}

func (f *TMainForm) OnBtn1Click(sender vcl.IObject) {
    aboutForm.Show()
}


// -- TAboutForm

func (f *TAboutForm) OnFormCreate(sender vcl.IObject) {
    f.SetCaption("About")
    f.Btn1 = vcl.NewButton(f)
    //f.Btn1.SetName("Btn1")
    f.Btn1.SetParent(f)
    f.Btn1.SetBounds(10, 10, 88, 28)
    f.Btn1.SetCaption("Button1")
    f.Btn1.SetOnClick(f.OnBtn1Click)  
}

func (f *TAboutForm) OnBtn1Click(sender vcl.IObject) {
    vcl.ShowMessage("Hello!")
}
``` 

#### Step 3: Copy the corresponding binary   

* Windows: Depending on whether the compiled binary is 32 or 64 bits, copy the corresponding `liblcl.dll` to the current executable file directory or system environment path.  
  * Go environment variable: `GOARCH = amd64 386` `GOOS = windows` `CGO_ENABLED=0`    

* Linux: Copy `liblcl.so` under the current executable file directory (you can also copy `liblcl.so` to `/usr/lib/` (32bit liblcl) or `/usr/lib/x86_64-linux-gnu/` (64bit liblcl) directory , Used as a public library).  
  * Go environment variable: `GOARCH = amd64` `GOOS = linux` `CGO_ENABLED=1`  

* MacOS: Copy `liblcl.dylib` to the current executable file directory (note under MacOS: you need to create info.plist file yourself), or refer to: [App packaging on MacOS](https://gitee.com/ying32/govcl/wikis/pages?sort_id=410056&doc_id=102420)  
  * Go environment variable: `GOARCH = amd64` `GOOS = darwin` `CGO_ENABLED=1`  

Note: The "current executable file directory" here refers to the location of the executable file generated by your currently compiled project.

---   

**Special Note: All UI components are non-threaded/non-coroutine safe. When used in goroutine, use [vcl.ThreadSync](https://gitee.com/ying32/govcl/wikis/pages?sort_id=976890&doc_id=102420) to synchronize updates to the UI.**  

**Special Note 2: If you use go>=1.15 to compile Windows executable files, you must use the `-buildmode=exe` compilation option, otherwise there will be errors.**  

---

### Ⅴ. FAQ

Q: Why is there no English WIKI?   
A: My English is bad. You can try using Google Translate [Chinese WIKI](https://gitee.com/ying32/govcl/wikis/pages).    
 
---  

### Ⅵ. API document

* [Lazarus LCL component WIKI](http://wiki.freepascal.org/LCL_Components)  
* [Windows API document](https://msdn.microsoft.com/zh-cn/library/ms123401.aspx)

----

![jetbrains](https://z-kit.cc/assets/images/jetbrains.png)  
[Thanks jetbrains](https://www.jetbrains.com/?from=govcl)  
