layout: ../../layouts/MarkdownPostLayout.astro
title: 'Wails Guide Application Development'
pubDate: "2023-11-11"
description: 'wails 框架应用开发准则。'
author: 'Ongraiso'
image:
    url: 'https://docs.astro.build/assets/full-logo-light.png'
    alt: 'The full Astro logo.'
tags: ["Wails", "blogging", "learning in public"]

# Application Development

There are no hard and fast rules for developing applications with Wails, but there are some basic guidelines.

这里有开发应用时用到的基本准则。

## Application Setup

The pattern used by the default templates are that `main.go` is used for configuring and running the application, whilst `app.go` is used for defining the application logic.

默认模板用的模式为：main.go用于配置和运行应用；app.go用于定义应用程序逻辑部分。

The `app.go` file will define a struct that has 2 methods which act as hooks into the main application:

app.go会定义一个结构体，这个结构体有两个方法，这两个方法作为主应用的hook

[of] `app.go`文件将定义一个结构体，该结构体有 2 个方法作为主应用程序的回调：

app.go

```go
type App struct {
    ctx context.Context
}

func NewApp() *App {
    return &App{}
}

func (a *App) startup(ctx context.Context) {
    a.ctx = ctx
}

func (a *App) shutdown(ctx context.Context) {
}
```

- The startup method is called as soon as Wails allocates the resources it needs and is a good place for creating resources, setting up event listeners and anything else the application needs at startup. It is given a `context.Context` which is usually saved in a struct field. This context is needed for calling the [runtime](https://wails.io/docs/reference/runtime/intro). If this method returns an error, the application will terminate. In dev mode, the error will be output to the console.

- The shutdown method will be called by Wails right at the end of the shutdown process. This is a good place to deallocate memory and perform any shutdown tasks.

- [on] ~~startup方法一般是在wails分配了它需要的资源后立刻调用~~，它是创建资源，设置事件监听，~~任何应用启动需要的好地方~~~。它提供了一个context，一般会存到一个结构体字段中。调用runtime的时候会用到他。

- [of] 一旦 Wails 分配了它需要的资源，就会调用 startup 方法，它是创建资源、设置事件侦听器以及应用程序在启动时需要的任何其他东西的好地方。 它提供了一个 `context.Context`， 通常保存在一个结构体字段中。 调用 [运行时](https://wails.io/zh-Hans/docs/reference/runtime/intro) 需要此`context.Context`。 如果此方法返回错误，则应用程序将终止。 在开发模式下，错误会输出到控制台。

- [on] ~~wails会在进程结束时会调用shutdoewn函数。~~他是~~取消分配~~内存以及~~平台~~关闭任务的好地方。

- [of] Shutdown 方法将在关闭过程结束时由 Wails 调用。 这是释放内存和执行关闭任务的好地方。

  > as soon as 是一旦 就。翻译习惯问题。
  >
  > 看不懂 为啥这样翻译：Shutdown 方法将在关闭过程结束时由 Wails 调用

The `main.go` file generally consists of a single call to `wails.Run()`, which accepts the application configuration. The pattern used by the templates is that before the call to `wails.Run()`, an instance of the struct we defined in `app.go` is created and saved in a variable called `app`. This configuration is where we add our callbacks:

[on] main.go通常会调用包含了一次wails.Run调用，wails.Run接受了应用配置。模板用的模式为：在调用wails.Run之前，我们在app.go中定义的结构体一个实例已经被创建，并且存到一个叫app的变量中。这个配置被我们添加到回调中。

[of] `main.go` 文件通常由对`wails.Run()`的单个调用组成，它接受应用程序配置。 模板使用的模式是，在调用 `wails.Run()` 之前， 我们创建并保存一个在 `app.go` 中定义的结构体的实例在名为 `app` 的变量中。 这个配置是我们添加回调的地方：

main.go

```go
func main() {

    app := NewApp()

    err := wails.Run(&options.App{
        Title:             "My App",
        Width:             800,
        Height:            600,
        OnStartup:  app.startup,
        OnShutdown: app.shutdown,
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

More information on application lifecycle hooks can be found [here](https://wails.io/docs/howdoesitwork#application-lifecycle-callbacks).

更多应用生命周期钩子可以在这里[here](https://wails.io/docs/howdoesitwork#application-lifecycle-callbacks)找到

可以在 [此处](https://wails.io/zh-Hans/docs/howdoesitwork#应用程序生命周期回调) 找到有关应用程序生命周期回调的更多信息。

> 翻译习惯
>
> 被动句 ACB
>
> 翻译规则：
>
> A be done by B。 （be done ->C ；C也就是动作）    A C B
>
> if len(A) < len(B):  
>
> ​	改 B C A（强调动作C或施动者B )
>
> else：
>
> ​	 改主动 A C B
>
> 例子：
>
> The shutdown method will be called by Wails right at the end of the shutdown process.
>
> showdon会在关闭进程时候 被调用
>
> A B C。
>
> More information on application lifecycle hooks can be found here
>
> 可以在这里 找到  A。。。
>
> B C A。
>
> 总结 翻译为中文，要看中文语句的习惯。 哪个重要（哪个字数长？），如果A（被动物）重要 那边就翻译主动（翻译为中文A放前面），如果B（动作或者施动者）重要，那么翻译被动（B放前面）
>
> https://zhuanlan.zhihu.com/p/157834944 被动句的翻译技巧与方法
>
> 为了强调被动动作或突出施动者时，可以将英语被动句译为汉语被动句。

## Binding Methods

It is likely that you will want to call Go methods from the frontend. This is normally done by adding public methods to the already defined struct in `app.go`:

A C B  

∵ (B>A)

∴ B C A

[on] 有可能你想从前端调用go方法。可通过在app.go添加公共方法到已经定义的结构体中完成这件事。

[of] 您可能希望从前端调用 Go 方法。 这通常是通过向 `app.go` 中已经定义的结构体中添加公共方法来实现的：

app.go

```go
type App struct {
    ctx context.Context
}

func NewApp() *App {
    return &App{}
}

func (a *App) startup(ctx context.Context) {
    a.ctx = ctx
}

func (a *App) shutdown(ctx context.Context) {
}

func (a *App) Greet(name string) string {
    return fmt.Sprintf("Hello %s!", name)
}
```

In the main application configuration, the `Bind` key is where we can tell Wails what we want to bind:

[on]  在主应用配置中，bind 键 是我们告诉wail要要绑定什么的位置

[of] 在主应用程序中，`Bind` 字段是我们告诉 Wails 想要绑定什么：

main.go

```go
func main() {

    app := NewApp()

    err := wails.Run(&options.App{
        Title:             "My App",
        Width:             800,
        Height:            600,
        OnStartup:  app.startup,
        OnShutdown: app.shutdown,
        Bind: []interface{}{
            app,
        },
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

This will bind all public methods in our `App` struct (it will never bind the startup and shutdown methods).

```go
    Bind: []interface{}{
        app,
    },
```

这将绑定 `App` 结构中的所有公共方法(它永远不会绑定 startup 和 shutdown 方法)。

### Dealing with context when binding multiple structs

If you want to bind methods for multiple structs but want each struct to keep a reference to the context so that you can use the runtime functions, a good pattern is to pass the context from the `OnStartup` method to your struct instances :

[on] 如果你要为多个结构体绑定方法，但是希望每个结构体都能保持一个对context的引用，使得你可以使用runtime函数，一种好方法是从OnStartup方法将context传递给你的struct实例。

[of] 如果您想为多个结构绑定方法，但希望每个结构都保留对 context 的引用，以便您可以使用运行时函数，一个好的方式是将上下文从 `OnStartup` 方法传递给您的结构实例：

```go
func main() {

    app := NewApp()
    otherStruct := NewOtherStruct()

    err := wails.Run(&options.App{
        Title:             "My App",
        Width:             800,
        Height:            600,
        OnStartup:  func(ctx context.Context){
            app.SetContext(ctx)
            otherStruct.SetContext(ctx)
        },
        OnShutdown: app.shutdown,
        Bind: []interface{}{
            app,
            otherStruct
        },
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

More information on Binding can be found [here](https://wails.io/docs/howdoesitwork#method-binding).

## Application Menu

Wails supports adding a menu to your application. This is done by passing a [Menu](https://wails.io/docs/reference/menus#menu) struct to application config. It's common to use a method that returns a Menu, and even more common for that to be a method on the `App` struct used for the lifecycle hooks.

wails支持添加一个菜单到你的应用。这可通过传递一个菜单结构体到应用配置中完成。通常会用一个返回菜单的方法~~，甚至更普遍的是app struct的一个方法成为生命重启钩子~~。

Wails 支持向您的应用程序添加菜单。 这是通过将 [菜单结构体](https://wails.io/zh-Hans/docs/reference/menus#菜单结构体) 传递给应用程序配置来完成的。 常见做法是使用一个返回菜单的方法，更常见的是用作生命周期回调的 `App` 结构体上的方法。

main.go

```go
func main() {

    app := NewApp()

    err := wails.Run(&options.App{
        Title:             "My App",
        Width:             800,
        Height:            600,
        OnStartup:  app.startup,
        OnShutdown: app.shutdown,
        Menu:       app.menu(),
        Bind: []interface{}{
            app,
        },
    })
    if err != nil {
        log.Fatal(err)
    }
}
```



## Assets

The great thing about the way Wails v2 handles assets is that it doesn't! The only thing you need to give Wails is an `embed.FS`. How you get to that is entirely up to you. You can use vanilla html/css/js files like the vanilla template. You could have some complicated build system, it doesn't matter.

[on] 关于wails v2处理assets的好消息是它不需要处理。你只需要给wails一个embed.FS。如何达到这点完全取决于你。你可以用像vanilla模板一样的 vanilla html/CSS/JS。可能你有更复杂的构建系统，这不重要。

[of] Wails v2 处理资源的方式的伟大之处在于它不需要处理！ 您唯一需要给 Wails 的是一个 `embed.FS`。 你如何做到这一点完全取决于你。 您可以像 vanilla 模板一样使用 vanilla html/css/js 文件。 您可能有一些复杂的构建系统，但这并不影响。

When `wails build` is run, it will check the `wails.json` project file at the project root. There are 2 keys in the project file that are read:

- "frontend:install"
- "frontend:build"

[of] 运行 `wails build` 时，它会检查项目根目录的 `wails.json` 文件。 文件中有 2 个字段会被读取：

The first, if given, will be executed in the `frontend` directory to install the node modules. The second, if given, will be executed in the `frontend` directory to build the frontend project.

[on] 如果给了第一个，fronted目录下将会执行node modules安装。如果给了第二个，fronted 目录将会执行前端项目构建。

[of] 第一个，如果有给定，将在 `frontend` 目录中执行以安装 node 模块。 第二个，如果有给定，将在 `frontend` 目录中执行以构建前端项目。

If these 2 keys aren't given, then Wails does absolutely nothing with the frontend. It is only expecting that `embed.FS`.

[of] 如果没有给出这两个字段，那么 Wails 不会对前端做任何操作。 它仅仅被用作 `embed.FS`。

### AssetsHandler

A Wails v2 app can optionally define a `http.Handler` in the `options.App`, which allows hooking into the AssetServer to create files on the fly or process POST/PUT requests. GET requests are always first handled by the `assets` FS. If the FS doesn't find the requested file the request will be forwarded to the `http.Handler` for serving. Any requests other than GET will be directly processed by the `AssetsHandler` if specified. It's also possible to only use the `AssetsHandler` by specifiy `nil` as the `Assets` option.

[on] ~~Wails v2 app可以选择在options.App中定义一个http.Handler，它允许触发assetsever在网络或者处理POST/PUT请求时创建文件~~。GET请求首先被assets FS处理。如果FS找不到请求文件，请求会被传到http.Handler服务。~~如果指定，除了GET的任何请求会直接被AssetHandler处理~~。~~也可以只用做作为Assets选项被指定为nil的AssetsHandler使用。~~



[of] Wails v2 应用程序可以选择在 `options.App` 中定义一个 `http.Handler`，它允许连接到 AssetServer 以动态创建文件或处理 POST/PUT 请求。 GET 请求总是首先由 `assets` FS 处理。 如果 FS 没有找到请求的文件，请求将被转发到 `http.Handler` 服务。 如果指定，除了 GET 以外的任何请求都将由 `AssetsHandler` 直接处理。 也可以仅通过将 `nil` 指定为 `Assets` 选项来使用 `AssetsHandler`。

## Built in Dev Server

Running `wails dev` will start the built in dev server which will start a file watcher in your project directory. By default, if any file changes, wails checks if it was an application file (default: `.go`, configurable with `-e` flag). If it was, then it will rebuild your application and relaunch it. If the changed file was in the assets, it will issue a reload after a short amount of time.

运行 `wails dev` 将启动内置的开发服务器，它将在您的项目目录中启动一个文件监听器。 默认情况下，如果有任何文件更改，wails 会检查它是否是应用程序文件（默认：.go，可使用 `-e` 标志配置）。 如果是，那么它将重新构建您的应用程序并重新启动它。 如果更改的文件在 assetdir 目录中，它会在很短的时间后重新加载。

The dev server uses a technique called "debouncing" which means it doesn't reload straight away, as there may be multiple files changed in a short amount of time. When a trigger occurs, it waits for a set amount of time before issuing a reload. If another trigger happens, it resets to the wait time again. By default this value is `100ms`. If this value doesn't work for your project, it can be configured using the `-debounce` flag. If used, this value will be saved to your project config and become the default.

开发服务器使用一种称为“防抖”的技术，这意味着它不会立即重新加载，因为可能会在短时间内更改多个文件。 当触发发生时，它会在发出重新加载之前等待一定的时间。 如果发生另一个触发，它会再次重置为等待时间。 默认情况下，此值为 100ms。 如果此值不适用于您的项目，则可以使用 `-debounce` 标志进行配置。 如果使用，此值将保存到您的项目配置中并成为默认值。

## External Dev Server

Some frameworks come with their own live-reloading server, however they will not be able to take advantage of the Wails Go bindings. In this scenario, it is best to run a watcher script that rebuilds the project into the build directory, which Wails will be watching. For an example, see the default svelte template that uses [rollup](https://rollupjs.org/guide/en/).

一些框架带有自己的实时重新加载服务器，但是它们将无法利用 Wails Go 绑定。 在这种情况下，最好运行一个监听脚本，将项目重新构建到构建目录中，Wails 将监视该目录。 有关示例，请参阅使用 [rollup](https://rollupjs.org/guide/en/) 的默认 svelte 模板。 对于 [create-react-app](https://create-react-app.dev/)，可以使用 [此脚本](https://gist.github.com/int128/e0cdec598c5b3db728ff35758abdbafd) 来实现类似的结果。

### Create React App[暂时跳过]

The process for a Create-React-App project is slightly more complicated. In order to support live frontend reloading the following configuration needs to be added to your `wails.json`:

```json
  "frontend:dev:watcher": "yarn start",
  "frontend:dev:serverUrl": "http://localhost:3000",
```

创建react app项目的步骤稍稍复杂一点。为了支持动态前端重加载，需要将下面这段代码加到wails.json

The `frontend:dev:watcher` command will start the Create-React-App development server (hosted on port `3000` typically). The `frontend:dev:serverUrl` command then instructs Wails to serve assets from the development server when loading the frontend rather than from the build folder. In addition to the above, the `index.html` needs to be updated with the following:

[of] frontend:dev:watcher 命令将启动 Create-React-App 开发服务器（通常托管在 3000 端口）。然后，frontend:dev:serverUrl 命令会指示 Wails 在加载前端时从开发服务器而不是从构建文件夹提供资产。除上述内容外，还需要用以下内容更新 index.html：

```html
    <head>
        <meta name="wails-options" content="noautoinject" />
        <script src="/wails/ipc.js"></script>
        <script src="/wails/runtime.js"></script>
    </head>
```



This is required as the watcher command that rebuilds the frontend prevents Wails from injecting the required scripts. This circumvents that issue by ensuring the scripts are always injected. With this configuration, `wails dev` can be run which will appropriately build the frontend and backend with hot-reloading enabled. Additionally, when accessing the application from a browser the React developer tools can now be used on a non-minified version of the application for straightforward debugging. Finally, for faster builds, `wails dev -s` can be run to skip the default building of the frontend by Wails as this is an unnecessary step.

[of] 之所以需要这样做，是因为重建前台的观察者命令会阻止 Wails 注入所需的脚本。这样可以确保脚本始终被注入，从而规避这一问题。有了这项配置，就可以运行 wails dev，它将在启用热加载的情况下适当地构建前端和后端。此外，当从浏览器访问应用时，React 开发者工具现在可用于非最小化版本的应用，以便直接调试。最后，为了加快构建速度，可以运行 wails dev -s，跳过 Wails 默认的前端构建，因为这是一个不必要的步骤。

## Go Module

The default Wails templates generate a `go.mod` file that contains the module name "changeme". You should change this to something more appropriate after project generation.

[of] 默认的 Wails 模板会生成一个包含模块名称“changeme”的 `go.mod` 文件。 您应该在项目生成后将其更改为更合适的内容。













b  b
