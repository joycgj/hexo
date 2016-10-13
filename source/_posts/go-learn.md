---
title: go learn
date: 2016-05-30 23:39:18
tags: [golang]
categories: [golang]
---


```
package main

import (
	"fmt"
	"io/ioutil"
)

func main() {
	b, e := ioutil.ReadFile("/Users/chengaojie/awktest/test")
	if e != nil {
		fmt.Println("read file error")
		return
	}
	fmt.Println(string(b))
}

a 1
b 2
a 10
b 25
```


