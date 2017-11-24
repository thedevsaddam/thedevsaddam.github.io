---
title: Let’s make a simple todo app with Go
#cover: "/images/coffee.jpg"
tags: ["go", "golang", "todo", "beginner", "vuejs"]
date: "2017-11-24"
---


![todo application](/media/posts/lets-make-a-simple-todo-app-with-Go/todo-app.png)

Generally Go is perfect for building micro services, but it doesn’t mean that trivial MVC app are not easy to build. Go has built-in support for parsing html template, though we are not going to focus on that.

First of all let’s write our go code for serving a html page and save it in a directory.

In our case let’s call the directory todoapp and create a home.tpl file inside it. Paste the html in home.tpl, it doing some http call to our backend application.

{{< gist thedevsaddam 92458365a430d2b6728d83395f14b3da >}}

I am not going to describe about the template and vue.js written in the template file in this article.

Let’s start building the backend, create a `main.go` file inside the same directory where the `home.tpl` reside.

_We are using chi-router for routing, mgo for mongodb to persist our data and renderer to render response_. You can install those packages using go get see the command below:

```bash
$ go get github.com/go-chi/chi
$ go get gopkg.in/mgo.v2
$ go get github.com/thedevsaddam/renderer
```

Now start to write our code, open main.go in your favorite editor.

Let initialize the essential constants, variables and create some types to use

{{< gist thedevsaddam 8f81cba7e2935fa2eac7f0a71a09566f >}}

Let’s describe the above example, between line 4–9 we declared a constant block to define some constants like host name, database name, port etc.

Between line 11–15 we declared two struct `todo` and `todoModel`. They both have the same number of fields, fields name also remain same. But in `todoModel` the ID field is a type of bson.ObjectId. We used bson tag so that mgo package can use the tags. And in our todo we used json tag so that json decoder can use these tags to serialize the Go type to typical json.

If you look the example, we missed the first two lines, where we declared two variables, first one is going to hold data type of pointer to renderer.Render and second one pointer to mgo.Database. See the code between line 27–33, wehere we initialized those two variables.

Lets begin to see our part two of the code

{{< gist thedevsaddam a66b4f877eeff413ddb09186d663a189 >}}

If you take a look to the above example, between line 1–4 we wrote a function `homeHandler` which is going to render our home page using the render package.

Between line 6–37 we wrote our `main` function which is our entry point. Inside main function at line 7 we created a channel type of os.Signal and at line 8 we register the channel to os.Signal package with an argument os.Interrupt which basically listen to os signals, if use signal cancel (ctrl+c) then notify the channel about the signal. We are using this signal to shutdown our server gracefully. At line 10 we are declaring and initializing a variable as chi router. At line 11 we are using a middleware to called Logger which comes with chi router as a sub-package, it basically log our incoming requests to stdout. At line 16 we declared a http.Server server variable and initialized it using struct literal. Between line 24 -29 we started a goroutine and fired up our server. At line 31 we are releasing the signal from the blocked channel and line 33 we are declaring a context with time out of 5 sec and passing it to the server shout down method. At line 35 we are using defer statement as soon as the main function ends the deferred cancel is called and release the context resources. At lien 14 we mounted a route group called todo and between line 39–48 we registered a route group which contains 4 routes.

Lets write the third part of the code

{{< gist thedevsaddam 666559abe98aeb67b27387eab8974497 >}}

In the above example line between 1–36 we wrote our `createTodo` function which is responsible for creating a new todo. At line 2 we we declared a variable of type todo and between line 4–7 we decode the incoming request json data to our `t` variable, if it fails we return a json reply to user using renderer.JSON() method. Betwen line 10–15 we checked if the title is empty then we send a json reposne to user. At line 18 we declared `tm` and initialized using struct literal with the incoming data form t variable and other information. Between line 24–30 we inserted the data to mondo db. Finally at 76 we sending a `json` reply to user that the todo created with the `todo_id`

If you see the `updateTodo` and `deleteTodo` functions we are doing similar type of work. When we are fetching todo list form mongo db we are using `fetchTodos` function. Between line 94–102 we are transforming the fetched `todoModel` data to `todo` type and sending the list to consumer as json. Thats it!

As you see I have written the article in a random order to tell you the story, it may be hard for a beginner to write the full source code and run it properly. Don’t worry, [here is the full source code](https://github.com/thedevsaddam/todoapp). Clone the source code to your `$GOPATH/src` directory, use `go get .` command to run the install the dependencies.

If you have any problem to understand any part of the article, or have any feedback or found anything that misleading please let me know using the comment section below :)

_If you like the article don’t forget to let me know, hit the appreciate button and share the article with others._

> Note: I’ll try to update the article based on your feedback, and if you have any question about the template part I’ll write about it soon. Thank you
If you like my writing please follow me [Saddam H](https://github.com/thedevsaddam).
