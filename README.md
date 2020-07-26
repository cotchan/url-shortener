### url-shortener

#### API service to generate shortened URL

+ URL: http://3.34.166.160

+ Description


---

### Contents

1. API    
    1-1. UserLevel API    
    1-2. SystemLevel API    

2. Overall structure design    

3. Diagram    
    3-1. Class Diagram    
    3-2. Sequence Diagram         
    
---

### 1. API

### 1-1. User Level API

#### REST API
```
POST /encode -> A short Url is generated for the received Url.
GET /decode/:shortUrl -> A UrlDTO Information is provided for the received Url.
GET /:shortUrl -> If you enter the short Url, you are redirected to the original Url page.
```

---

<code> POST /encode </code>: A short Url is generated for the received Url.

Request Body
```
{
"url": String
}
```
Success case (Response Body)
```
statusCode: 200
{
"shortUrl": String
}
```

Fail case (Response Body)

```
statusCode: 200
{
}
```

<code>GET /decode/:shortUrl</code>: A UrlDTO Information is provided for the received Url.

Request Body
```
None
```

Success case (Response Body)

```
statusCode: 200
{
"uuid": Long,
"shortUrl": String,
"originalUrl": String
}
```

Fail case (Response Body)

```
statusCode: 404
{
}

```

<code>GET /:shortUrl</code>: If you enter the short Url, you are redirected to the original Url page.   

Request Body   

```
None
```
Success case (Response Body)
```
statusCode: 302 (Found)
{
}
```
Fail case (Response Body)
```
statusCode: 404
{
}
```

---

### 1-2. System Level API    

* Contents
    1. DTO
    2. UrlStorageService.java
    3. UrlEncodeService.java 
    4. UrlDecodeService.java 
    5. UrlEncodeController.java 
    6. UrlDecodeController.java 
    7. UrlRouterController.java

#### 1. DTO

```
/**
 * Url DTO
 */
class Url { 
    private long uuid;
    private String shortUrl;
    private String originalUrl;
}
```

#### 2. UrlStorageService.java  
UrlRepository class

```
Url getByOriginalUrl(String originalUrl);
Url getByShortUrl(String shortUrl);
CompletableFuture<Boolean> saveUrl(Url url);

```
    
#### 3. UrlEncodeService.java 
Class to make shortUrl from OriginalUrl

```
/**
 * Method to make shortUrl from OriginalUrl
 * @param originalUrl
 * @return shortUrl
 */
public CompletableFuture<String> encodeUrl(final String requestedUrl)

```

#### 4. UrlDecodeService.java 
Class to get UrlDTO information from shortUrl

```
/**
 * Method to get UrlDTO information from shortUrl
 * @param shortUrl
 * @return Url
 */
public Url decodeUrl(final String requestedUrl);
```

#### 5. UrlEncodeController.java
Class that handles "A short Url is generated for the received Url" request

```
/**
 * Method that handles "A short Url is generated for the received Url" request
 * @param requestBody
 * @return shortUrl
 */
public CompletableFuture<String> encodeUrl(@RequestBody String requestBody)
```

#### 6. UrlDecodeController.java 
Class that handles "A UrlDTO Information is provided for the received Url." request

```
/**
 * Method that handles "A UrlDTO Information is provided for the received Url." request
 * @param shortUrl
 * @return HttpResponse
 */
public HttpResponse decodeUrl(@Param String url)
```

#### 7. UrlRouterController.java
Class that handles "If you enter the short Url, you are redirected to the original Url page." request

```
/**
 * Method to redirect to originalUrl page
 * @param shortUrl
 * @return HttpResponse
 */
public HttpResponse routeUsingShortUrl(@Param String url)
```


