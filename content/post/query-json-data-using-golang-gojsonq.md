---
title: Query JSON data using Golang (gojsonq package)
tags: ["go", "golang", "gojsonq", "query json", "golang query from json"]
date: "2018-05-29"
---
Most often developer needs to consume JSON data from other service and query over them. Querying JSON data is little time consuming. For the last few days I was working on a package for Golang to query JSON data easily. The idea and inspiration comes form PHP-JSONQ by Nahid Bin Azhar.
Lets take a sample JSON data to start with:

{{< gist thedevsaddam ee7f25adb9f4278ce0ef37b8d28bed98 >}}


Now we are ready to query the data, lets see some examples
### Example 1
Query: select * from vendor.items where price > 1200 or id null
Using gojsonq we can do the query like:

{{< gist thedevsaddam 06b19c3375b943619a3eb8c7555f6504 >}}

### Example 2
Query: select name, price from vendor.items where price > 1200 or id null
Using gojsonq we can do the query like:

{{< gist thedevsaddam 19de6e4aa48924a7f492e6e7749fc450 >}}

### Example 3
Query: select sum(price) from vendor.items where price > 1200 or id null
Using gojsonq we can do the query like:

{{< gist thedevsaddam 4f807403b5a88b8b50521e9dedf67352 >}}

Note you can use other aggregation function like Min/Max/Avg/Count`

###Example 4
Query: select price from vendor.items where price > 1200
Using gojsonq we can do the query like:

{{< gist thedevsaddam 4d7eeba49f3b77cba1da78d3bc13fb49>}}

Note: You'll get a plain array of price

### Example 5
Query: select * from vendor.items order by price
Using gojsonq we can do the query like:

{{< gist thedevsaddam ca0b5adb81772e808b25dbfffa26cd47>}}

### Example 6
Query: select description from .
Using gojsonq we can do the query like:

{{< gist thedevsaddam 6819b4ae4e7d6c52cde58a7ae3e52d60>}}

There are some other useful methods available in the package. The package is still under heavy development! If you have any suggestion or bug report don't forget to open an issue
If you are not using Go, you can also use the same implementation of the package using PHP, Javascript, Python and Kotlin. Links are listed below:
php-jsonq
js-jsonq
py-jsonq
kotlin-jsonq

If you like the idea don't forget to give a STAR :)
GithubLink for gojsonq
Thank you very much for reading the article
