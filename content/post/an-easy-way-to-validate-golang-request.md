---
title: An easy way to validate Go request
#cover: "/images/coffee.jpg"
tags: ["go", "golang", "api", "request", "request validation"]
date: "2017-11-22"
---

When building REST API or web applications using Go, an essential part is to validate the incoming request data.
I have worked in some small and medium project in Golang, most of them are micro-services, providing restful api. I have used different way to validate incoming data. Lets start with particular one that I use frequently.
For validating application/json or text/plain request, first I build a struct type of that particular request and add a validate method on the struct. Lets see an example:


{{< gist thedevsaddam 2d5ec1d84a3c7292f3a9119db98af2b8 >}}

This is my first choice when accepting json payload, but for form-data, query string or x-www-form-urlencoded , its little bit different.
For complex validation generally I use: https://github.com/asaskevich/govalidator , but some times it took more time for validating the request other than writing the business logic. When a validation rules change you have to rewrite the message also. To avoid this, I have build a simple validator package for golang request validation.
Lets see how it works:


{{< gist thedevsaddam 2c0c0070a7b8593f0eb91ffa687ba688 >}}

This is little bit cleaner and simple, when you need to modify the rules you can do it easily. What if you need a custom validation rule, for example you are asking for a rule where you can check unique username. You can add your custom rule easily. Lets see an example of adding a custom rules:


{{< gist thedevsaddam a26cf974a263a6ae0adcaa7ea5d822e7 >}}

Thats simple to add a custom rule. I am still experimenting on request validation process, using struct tags or string for rules is not enough. Hope one day there will be a much more better solution for validating incoming request. Please share your opinion on the validation process, how you like it.
If you like the post don’t forget to hit the love button :D and of course hit the star button on this repo: https://github.com/thedevsaddam/govalidator
Create an issue, do some discussion to make the validation way better. Until then stay happy with coding!
