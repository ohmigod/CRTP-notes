---
description: RESTful API Vulnerabilities
---

# REST

> A RESTful API is an architectural style for an application program interface (API) that uses HTTP requests to access and use data. That data can be used to GET, PUT, POST and DELETE data types, which refers to the reading, updating, creating and deleting of operations concerning resources.

### Checklist

* [ ] Change content-type from json to XML. Then try [XXE](../vulnerabilities/xxe.md).
* [ ] Find the [API definition](rest.md#undefined).
* [ ] Try accessing /version.txt, /api/version.txt, /api/v1/version.txt...
* [ ] Find GUIs like swagger-ui or others.&#x20;
* [ ] Add the \[] instead of a value (may trigger stack traces): /api/v1/books/\[]&#x20;
* [ ] Read the docs if it is an open source API. Might contain endpoints to perform OS execution, file read...&#x20;

### Finding REST API docs

If available, it's useful to get a copy of API docs. Specifically in Swagger/OpenAPI or WADL format. Some of the common doc paths are (not necessarily available by default):

| Framework                      | Path                                                                |
| ------------------------------ | ------------------------------------------------------------------- |
| Generic                        | /, /doc, /v1/api/doc, etc.                                          |
| Jersey                         | /application.wadl\[detailed=true], /\<resource>/application.wadl    |
| RESTEasy                       | /application.xml                                                    |
| CXF                            | /\<WAR>/\<servlet>/services, /\<WAR>/\<servlet>/\<resource>/?\_wadl |
| Spring w/ Springfox            | /v2/api-docs                                                        |
| ASP.NET Web API w/ Swashbuckle | /swagger/docs/v1                                                    |

### API docs wordlist

```
application.xml
doc
api/doc
v2/api-docs
swagger/docs/v1
swagger-ui.html
swagger/swagger-ui.html
api/swagger-ui.html
v1.0/swagger-ui.html
v1/swagger-ui.html
api/v1/swagger-ui.html
swagger/index.html
swagger/index.html?url=/api-docs/openapi.yaml
application.wadl
application.wadl?detail=false
application.wadl?detail=true
```
