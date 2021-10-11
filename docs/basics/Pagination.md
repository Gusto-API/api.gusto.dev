# Pagination

To enchance API performance, several collection endpoints support pagination. To use this feature add a `page` and, optionally, a `per` parameter to the URL's query string. For example, the following url...

```
https://api.gusto.com/v1/companies/abc123/employees?page=2&per=5
```

...will return the second page of five employees of the company identified by the UUID `abc123`. If the `per` parameter is not provided, the API will return 25 records per page by default, unless otherwise specified.

Information on whether a given endpoint supports pagination parameters can be found on its respective page in the documentation.

## Paginated responses

If pagination parameters are provided in the request, some metadata about the paginated collection will be returned in the response's headers.

```
X-Page: 3
X-Total-Count: 542
X-Total-Pages: 22
X-Per-Page: 25
```

Here's a summary of the meaning of each of these headers:

Header | Description | Example
---------|----------|---------
 `X-Page` | The current page being returned | `3`
 `X-Total-Count` | The total number of records in the full collection | `542`
 `X-Total-Pages` | Total number of pages for this collection | `22`
 `X-Per-Page` | Numer of records currently being returned per page | `25`
