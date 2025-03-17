# HTTP


## What is an HTTP Redirect?

An **HTTP redirect** is a response from a web server that tells the client (e.g., a browser) that the requested resource has moved to a new location. The client is then expected to follow this new location.

## Types of HTTP Redirects

There are multiple types of redirects, but the most relevant for a **permanent redirect** is:

### 301 Moved Permanently

- Indicates that the resource has been **permanently moved** to a new location.
- Clients (browsers, search engines) should **cache** the new location and use it in future requests.

### 302 Found (Temporary Redirect)

- Indicates a **temporary** redirection.
- Clients should **not cache** the redirect.
- Often used when a resource is temporarily moved but might return later.

## How an HTTP Redirect Works

1. **Client makes a request:**

```http
    GET /old-path HTTP/1.1
    Host: example.com
```

2. **Server responds with a redirect:**

```http
    HTTP/1.1 301 Moved Permanently
    Location: https://example.com/new-path
```

3. **Client follows the `Location` header** and requests the new resource:

```http
    GET /new-path HTTP/1.1
    Host: example.com
```

#  Implementation in HTTPServer

### Step 1: Add a Redirect Map

- Define a mapping of old paths to new paths (e.g., using a JavaScript `Map` or an object).
- Example:
    
    ```javascript
    const redirectMap = {
      "/old-page": "/new-page",
      "/outdated-link": "/updated-link"
    };
    ```
    

### Step 2: Modify `handleRequest` to Check for Redirects

- Extract the request path from `req.path`.
- Check if it exists in the `redirectMap`.
- If found, generate a **301 Redirect** response.

### Step 3: Modify the `Response` Class to Send Redirects

- Add a method (e.g., `sendRedirect`) in `Response` that:
    
    1. Sets status code to `301`.
    2. Adds a `Location` header pointing to the new URL.
    3. Sends the response through the socket.

Example:
```javascript
    sendRedirect(url) {
      this.setStatus(301, Response.STATUS_CODES[301]);
      this.setHeader("Location", url);
      this.send();
    }
```


### Step 4: Use the New Redirect Method in `handleRequest`

- If a redirect path is found, call the `sendRedirect` method:
    
```javascript
    if (redirectMap.hasOwnProperty(req.path)) {
      res.sendRedirect(redirectMap[req.path]);
      return;
    }
```


# Additional Considerations

**Logging:** Log redirects for debugging purposes.  
**Edge Cases:** Handle cases where paths may have trailing slashes (`/old-path/` vs `/old-path`).  
**Testing:** Test different redirect scenarios to ensure proper behavior.

