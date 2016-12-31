# springREST
It record the debug tips when writing spring rest codes


# Spring REST

## Issues and solutions

### #1 issue
When URL include "?", it can't be requested mapped well, e.g., when having below codes in controller

`@RequestMapping(value = "/articles/?offset={offset}&limit={limit}", method = RequestMethod.GET, headers = "Accept=Application/Json")`

The request will not match /articles?offset..., instead it will just match "articles".

### #1 Solution
Use @RequestParam to handle it
Just like this:

 `@RequestMapping(value = "/articles", method = RequestMethod.GET, headers = "Accept=Application/Json", params = "offset")
    public Object getArticlesPagingList(@RequestParam int offset, @RequestParam int limit, HttpServletResponse rep)`
    
### #2 issue

We want to have two request. 
One is to get the articles list with /articles URL;
Another is to get the articles with pagination with /articles?offset={offset}&limit={limit}.
If we write them with the following way:

`@RequestMapping(value = "/articles", method = RequestMethod.GET, headers = "Accept=Application/Json")
    public Object getArticlesList(HttpServletResponse rep)`
    
`@RequestMapping(value = "/articles", method = RequestMethod.GET, headers = "Accept=Application/Json")
    public Object getArticlesPagingList(@RequestParam int offset, @RequestParam int limit, HttpServletResponse rep)`

There will be error like below:    
`java.lang.IllegalStateException: Ambiguous mapping found. Cannot map 'articleController' bean method 
public java.lang.Object com.mae.mobileBackend.controller.ArticleController.getArticlePagingList(int,int,javax.servlet.http.HttpServletResponse)
to {[/articles],methods=[GET],params=[],headers=[],consumes=[],produces=[application/json],custom=[]}: There is already 'articleController' bean method`

### #2 solution
The reason is that there are two same requests mapping for /articles, we should use different parameter to indicate it.

Chaning codes to below, it works:

` @RequestMapping(value = "/articles", method = RequestMethod.GET, headers = "Accept=Application/Json")`
`@RequestMapping(value = "/articles", method = RequestMethod.GET, headers = "Accept=Application/Json", params = "offset")
    public Object getScopePagingList(@RequestParam int offset, @RequestParam int limit, HttpServletResponse rep)`


