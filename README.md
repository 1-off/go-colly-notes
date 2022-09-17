# go-colly-notes


## basic colly scrap 
- The OnHTML event can be used to take action when a specific HTML element is found.
- The OnRequest event is raised when an HTTP request is sent to a URL. This event is used to track which URL is being visited.
- OnResponse can be used to examine the response. 
```go
package main

import (
   "github.com/gocolly/colly"
)

func main(url string) {
unc search(item string) {
	c := colly.NewCollector()

	// Find and visit all links
	c.OnHTML("a", func(e *colly.HTMLElement) {
  	
    fmt.Println(e.Attr("href"))
		err := e.Request.Visit(e.Attr("href"))
		if err != nil {
			return
		}
	})

	c.OnRequest(func(r *colly.Request) {
		fmt.Println("Visiting", r.URL)
	})
  
  c.OnResponse(func(r *colly.Response) {
   fmt.Println(r.StatusCode)
})

	err := c.Visit(url)
	if err != nil {
		return
	}
}
```

## Target specific tags 
- the ```ChildAttr``` function that takes two parameters: the CSS selector and the name of the attribute
```go
type Book struct {
	Title string
	Price string
}

book := Book{}
	book.Title = e.ChildAttr(".image_container img", "alt")
	book.Price = e.ChildText(".price_color")
	fmt.Println(book.Title, book.Price)
})
```
![](https://github.com/1-off/go-colly-notes/blob/main/img/1.png)

## References
- https://www.scrapingbee.com/blog/web-scraping-go/
- https://go-colly.org/

