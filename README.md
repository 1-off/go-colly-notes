# go-colly-notes


## basic colly scrap 
- The OnHTML event can be used to take action when a specific HTML element is found. OnHTML is a powerful tool. It can search for CSS selectors (i.e. div.my_fancy_class or #someElementId)
- The OnRequest event is raised when an HTTP request is sent to a URL. This event is used to track which URL is being visited.
- OnResponse can be used to examine the response. 
- Attr() method and Text property that colly.HTMLElement has, we can also use it to traverse child elements. The ChildAttr(), ChildText(), and ForEach() methods
  - ChildText() to get the text of all the paragraphs in a section
```go
    c.OnHTML("#myCoolSection", func(e *colly.HTMLElement) {
    fmt.Println(e.ChildText("p"))})
```
Example:
```go
package main

import (
   "github.com/gocolly/colly"
)

func main(url string) {
unc search(item string) {
	c := colly.NewCollector(
  // Allow visiting the same page multiple times
    colly.AllowURLRevisit(),
    // Allow crawling to be done in parallel / async
    colly.Async(true),

)

	// Find and visit all links
	c.OnHTML("a[href]", func(e *colly.HTMLElement) {
  	link := e.Attr("href")
    fmt.Println(link)
		err := e.Request.AbsoluteURL(link))
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

