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
  - And we can use ForEach() to iterate over an elements children that match a specific selector
  ```go
   c.OnHTML("#myCoolSection", func(e *colly.HTMLElement) {
    e.ForEach("p", func(_ int, elem *colly.HTMLElement) {
        if strings.Contains(elem.Text, "golang") {
            fmt.Println(elem.Text)
        }    
    })})
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

## htmlquery 
htmlquery is an XPath query package for HTML, lets you extract data or evaluate from HTML documents by an XPath expression.
```go
package main

import (
    "fmt"

    "github.com/PuerkitoBio/goquery"
)

func main() {
    doc, err := goquery.NewDocument(url)
    if err != nil {
        panic(err)
    }
    s := doc.Find(`html > head > meta[name="viewport"]`)
    if s.Length() == 0 {
        fmt.Println("could not find viewpoint")
        return
    }
    fmt.Println(s.Eq(0).AttrOr("content", ""))
```
- https://github.com/antchfx/htmlquery

