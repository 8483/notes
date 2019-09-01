The key principles of REST involve separating your API into logical resources. These resources are manipulated using HTTP requests where each method has a specific meaning.

The most used ones are (CRUD):
- **POST** - Create
- **GET** - Read
- **PUT** - Update
- **DELETE** - Delete

Use 2 base URLs per resource. In your URLs - **nouns are good; verbs are bad**.

The endpoints should **always** be plural.

Resource | POST | GET | PUT | DELETE
--- | --- | --- | --- | ---
/dogs | create new dog | list **all** dogs | update **all** dogs | delete **all** dogs
/dogs/123 | **ERROR** | show **one** dog | update **one** dog | delete **one** dog

### Example

Method | Resource | Result
--- | --- | --- 
GET | /tickets | Retrieves a list of tickets
GET | /tickets/12 | Retrieves a specific ticket
POST | /tickets | Creates a new ticket
PUT | /tickets/12 | Updates ticket #12
PATCH | /tickets/12 | Partially updates ticket #12
DELETE | /tickets/12 | Deletes ticket #12

### Nested resources

Method | Resource | Result
--- | --- | --- 
GET | /tickets/12/messages | Retrieves list of messages for ticket #12
GET | /tickets/12/messages/5 | Retrieves message #5 for ticket #12
POST | /tickets/12/messages | Creates a new message in ticket #12
PUT | /tickets/12/messages/5 | Updates message #5 for ticket #12
PATCH | /tickets/12/messages/5 | Partially updates message #5 for ticket #12
DELETE | /tickets/12/messages/5 | Deletes message #5 for ticket #12

### Actions that don't fit into CRUD

Restructure the action to appear like a field of a resource. This works if the action doesn't take parameters. For example an activate action could be mapped to a boolean activated field and updated via a PATCH to the resource.  

Treat it like a sub-resource with RESTful principles. For example, GitHub's API lets you star a gist with PUT /gists/:id/star and unstar with DELETE /gists/:id/star.

Sometimes you really have no way to map the action to a sensible RESTful structure. For example, a multi-resource search doesn't really make sense to be applied to a specific resource's endpoint. In this case, /search would make the most sense even though it isn't a resource. This is OK - just do what's right from the perspective of the API consumer and make sure it's documented clearly to avoid confusion.

# Naming Convention

Use consistent resource naming conventions and URI formatting for minimum ambiguily and maximum readability and maintainability. You may implement below design hints to achieve consistency:

### **Use forward slash (/) to indicate a hierarchical relationships**

The forward slash (/) character is used in the path portion of the URI to indicate a hierarchical relationship between resources. e.g.

```
http://api.example.com/device-management
http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices/{id}
http://api.example.com/device-management/managed-devices/{id}/scripts
http://api.example.com/device-management/managed-devices/{id}/scripts/{id}
```

### **Do not use trailing forward slash (/) in URIs**

As the last character within a URI’s path, a forward slash (/) adds no semantic value and may cause confusion. It’s better to drop them completely.

```
http://api.example.com/device-management/managed-devices/
http://api.example.com/device-management/managed-devices 	/*This is much better version*/
```
### **Use hyphens (-) to improve the readability of URIs**

To make your URIs easy for people to scan and interpret, use the hyphen (-) character to improve the readability of names in long path segments.

```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory-management/managedEntities/{id}/installScriptLocation  //Less readable
```

### **Do not use underscores ( _ )**

It’s possible to use an underscore in place of a hyphen to be used as separator – But depending on the application’s font, it’s possible that the underscore (_) character can either get partially obscured or completely hidden in some browsers or screens.

To avoid this confusion, use hyphens (-) instead of underscores ( _ ).

```
http://api.example.com/inventory-management/managed-entities/{id}/install-script-location  //More readable
http://api.example.com/inventory_management/managed_entities/{id}/install_script_location  //More error prone
```

### **Use lowercase letters in URIs**

When convenient, lowercase letters should be consistently preferred in URI paths.

RFC 3986 defines URIs as case-sensitive except for the scheme and host components. e.g.
```
http://api.example.org/my-folder/my-doc  //1
HTTP://API.EXAMPLE.ORG/my-folder/my-doc  //2
http://api.example.org/My-Folder/my-doc  //3
```
In above examples, 1 and 2 are same but 3 is not as it uses My-Folder in capital letters.

### **Never use CRUD function names in URIs**

URIs should not be used to indicate that a CRUD function is performed. URIs should be used to uniquely identify resources and not any action upon them. HTTP request methods should be used to indicate which CRUD function is performed.
```
HTTP GET http://api.example.com/device-management/managed-devices  //Get all devices
HTTP POST http://api.example.com/device-management/managed-devices  //Create new Device

HTTP GET http://api.example.com/device-management/managed-devices/{id}  //Get device for given Id
HTTP PUT http://api.example.com/device-management/managed-devices/{id}  //Update device for given Id
HTTP DELETE http://api.example.com/device-management/managed-devices/{id}  //Delete device for given Id
```

### **Use query component to filter URI collection**

Many times, you will come across requirements where you will need a collection of resources sorted, filtered or limited based on some certain resource attribute. For this, do not create new APIs – rather enable sorting, filtering and pagination capabilities in resource collection API and pass the input parameters as **query parameters**. e.g.

```http://api.example.com/device-management/managed-devices
http://api.example.com/device-management/managed-devices?region=USA
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ
http://api.example.com/device-management/managed-devices?region=USA&brand=XYZ&sort=installation-date
```

# SSL everywhere - all the time

Always use SSL. No exceptions. Today, your web APIs can get accessed from anywhere there is internet (like libraries, coffee shops, airports among others). Not all of these are secure. Many don't encrypt communications at all, allowing for easy eavesdropping or impersonation if authentication credentials are hijacked.  

Another advantage of always using SSL is that guaranteed encrypted communications simplifies authentication efforts - you can get away with simple access tokens instead of having to sign each API request.  

One thing to watch out for is non-SSL access to API URLs. Do not redirect these to their SSL counterparts. Throw a hard error instead! The last thing you want is for poorly configured clients to send requests to an unencrypted endpoint, just to be silently redirected to the actual encrypted endpoint.

# Result filtering, sorting & searching

It's best to keep the base resource URLs as lean as possible. Complex result filters, sorting requirements and advanced searching (when restricted to a single type of resource) can all be easily implemented as query parameters on top of the base URL. Let's look at these in more detail:  

### Filtering

Use a unique query parameter for each field that implements filtering. For example, when requesting a list of tickets from the /tickets endpoint, you may want to limit these to only those in the open state. This could be accomplished with a request like GET /tickets?state=open. Here, state is a query parameter that implements a filter.  

### Sorting

Similar to filtering, a generic parameter sort can be used to describe sorting rules. Accommodate complex sorting requirements by letting the sort parameter take in list of comma separated fields, each with a possible unary negative to imply descending sort order. Let's look at some examples:  

`GET /tickets?sort=-priority` - Retrieves a list of tickets in descending order of priority  

`GET /tickets?sort=-priority,created_at` - Retrieves a list of tickets in descending order of priority. Within a specific priority, older tickets are ordered first  

### Searching

Sometimes basic filters aren't enough and you need the power of full text search. Perhaps you're already using ElasticSearch or another Lucene based search technology. When full text search is used as a mechanism of retrieving resource instances for a specific type of resource, it can be exposed on the API as a query parameter on the resource's endpoint. Let's say q. Search queries should be passed straight to the search engine and API output should be in the same format as a normal list result.

Combining these together, we can build queries like:  

`GET /tickets?sort=-updated_at` - Retrieve recently updated tickets.  
`GET /tickets?state=closed&sort=-updated_at` - Retrieve recently closed tickets.  
`GET /tickets?q=return&state=open&sort=-priority,created_at` - Retrieve the highest priority open tickets mentioning the word 'return'

# JSON only responses

It's time to leave XML behind in APIs. It's verbose, it's hard to parse, it's hard to read, its data model isn't compatible with how most programming languages model data and its extendibility advantages are irrelevant when your output representation's primary needs are serialization from an internal representation.

# snake_case vs camelCase for field names

If you're using JSON (JavaScript Object Notation) as your primary representation format, the "right" thing to do is to follow JavaScript naming conventions - and that means camelCase for field names!  

If you then go the route of building client libraries in various languages, it's best to use idiomatic naming conventions in them - camelCase for C# & Java, snake_case for python & ruby.  

Food for thought: I've always felt that snake_case is easier to read than JavaScript's convention of camelCase. I just didn't have any evidence to back up my gut feelings, until now. Based on an eye tracking study on camelCase and snake_case (PDF) from 2010, **snake_case is 20% easier to read than camelCase**! That impact on readability would affect API explorability and examples in documentation.  

Many popular JSON APIs use snake_case. I suspect this is due to serialization libraries following naming conventions of the underlying language they are using. Perhaps we need to have JSON serialization libraries handle naming convention transformations.

# Pretty print by default & ensure gzip is supported
An API that provides white-space compressed output isn't very fun to look at from a browser. Although some sort of query parameter (like ?pretty=true) could be provided to enable pretty printing, an API that pretty prints by default is much more approachable. The cost of the extra data transfer is negligible, especially when you compare to the cost of not implementing gzip.  

Consider some use cases: What if an API consumer is debugging and has their code print out data it received from the API - It will be readable by default. Or if the consumer grabbed the URL their code was generating and hit it directly from the browser - it will be readable by default. These are small things. Small things that make an API pleasant to use!  

### But what about all the extra data transfer?

Let's look at this with a real world example. I've pulled some data from GitHub's API, which uses pretty print by default. I'll also be doing some gzip comparisons:  
```bash
$ curl https://api.github.com/users/veesahni > with-whitespace.txt
$ ruby -r json -e 'puts JSON JSON.parse(STDIN.read)' < with-whitespace.txt > without-whitespace.txt
$ gzip -c with-whitespace.txt > with-whitespace.txt.gz
$ gzip -c without-whitespace.txt > without-whitespace.txt.gz
```
The output files have the following sizes:
```bash
without-whitespace.txt - 1252 bytes  
with-whitespace.txt - 1369 bytes  
without-whitespace.txt.gz - 496 bytes  
with-whitespace.txt.gz - 509 bytes  
```

In this example, the whitespace increased the output size by 8.5% when gzip is not in play and 2.6% when gzip is in play. On the other hand, the act of gzipping in itself provided over 60% in bandwidth savings. Since the cost of pretty printing is relatively small, it's best to pretty print by default and ensure gzip compression is supported!

To further hammer in this point, Twitter found that there was an 80% savings (in some cases) when enabling gzip compression on their Streaming API. Stack Exchange went as far as to never return a response that's not compressed!

# Authentication

A RESTful API should be stateless. This means that request authentication should not depend on cookies or sessions. Instead, each request should come with some sort authentication credentials.

By always using SSL, the authentication credentials can be simplified to a randomly generated access token that is delivered in the user name field of HTTP Basic Auth. The great thing about this is that it's completely browser explorable - the browser will just popup a prompt asking for credentials if it receives a 401 Unauthorized status code from the server.  

However, this token-over-basic-auth method of authentication is only acceptable in cases where it's practical to have the user copy a token from an administration interface to the API consumer environment. In cases where this isn't possible, OAuth 2 should be used to provide secure token transfer to a third party. OAuth 2 uses Bearer tokens & also depends on SSL for its underlying transport encryption.  

An API that needs to support JSONP will need a third method of authentication, as JSONP requests cannot send HTTP Basic Auth credentials or Bearer tokens. In this case, a special query parameter access_token can be used. Note: there is an inherent security issue in using a query parameter for the token as most web servers store query parameters in server logs.  

For what it's worth, all three methods above are just ways to transport the token across the API boundary. The actual underlying token itself could be identical.

# HTTP status codes

HTTP defines a bunch of meaningful status codes that can be returned from your API. These can be leveraged to help the API consumers route their responses accordingly. I've curated a short list of the ones that you definitely should be using:

Code | Message | Meaning
--- | --- | ---
200 | OK | Response to a successful GET, PUT, PATCH or DELETE. Can also be used for a POST that doesn't result in a creation.
201 | Created | Response to a POST that results in a creation. Should be combined with a Location header pointing to the location of the new resource
204 | No Content | Response to a successful request that won't be returning a body (like a DELETE request)
304 | Not Modified | Used when HTTP caching headers are in play
400 | Bad Request | The request is malformed, such as if the body does not parse
401 | Unauthorized | When no or invalid authentication details are provided. Also useful to trigger an auth popup if the API is used from a browser
403 | Forbidden | When authentication succeeded but authenticated user doesn't have access to the resource
404 | Not Found | When a non-existent resource is requested
405 | Method Not Allowed | When an HTTP method is being requested that isn't allowed for the authenticated user
410 | Gone | Indicates that the resource at this end point is no longer available. Useful as a blanket response for old API versions
415 | Unsupported Media Type | If incorrect content type was provided as part of the request
422 | Unprocessable Entity | Used for validation errors
429 |Too Many Requests | When a request is rejected due to rate limiting