# Module 6: How The Web Works

## 1 - DNS in Detail
DNS (Domain Name System)

**Objective:**
I explored how the Domain Name System (DNS) operates as the "phonebook of the internet." I learned how domain names are structured, the different types of DNS records, and the exact step-by-step journey a computer takes to translate a human-readable website name into an IP address.

###  What is DNS?
**DNS (Domain Name System)** provides a simple way to communicate with devices on the internet without having to memorize complex numbers. Every computer on the internet has a unique **IP (Internet Protocol)** address (e.g., `104.26.10.229`). Because it's inconvenient to remember these numbers, DNS translates a friendly name like `tryhackme.com` into the correct IP address in the background.

###  Domain Name Hierarchy
I learned that a domain name is broken down into specific parts, read from right to left:

*   **TLD (Top-Level Domain):** The most right-hand part of the domain (e.g., `.com`). There are two main types:
    *   **gTLD (Generic Top-Level Domain):** Historically used to tell the user the domain's purpose (e.g., `.com` for commercial, `.org` for organization, `.edu` for education). Today, there are hundreds, like `.biz` or `.online`.
    *   **ccTLD (Country Code Top-Level Domain):** Used for geographical purposes (e.g., `.ca` for Canada, `.co.uk` for the UK).
*   **Second-Level Domain:** This sits just to the left of the TLD. In `tryhackme.com`, `tryhackme` is the second-level domain. It is limited to 63 characters and can only use letters (a-z), numbers (0-9), and hyphens. 
*   **Subdomain:** This sits on the left-hand side of the second-level domain, separated by a period (e.g., `admin.tryhackme.com`). You can chain multiple subdomains together (like `jupiter.servers.tryhackme.com`), provided the total length of the name does not exceed 253 characters.

###  Common DNS Record Types
DNS isn't just for routing websites; it handles multiple types of records for different services. I broke down the most common ones:

| Record Type | Abbreviation Meaning | Description | Example |
| :--- | :--- | :--- | :--- |
| **A** | Address | Resolves a domain name to an **IPv4 (Internet Protocol version 4)** address. | `104.26.10.229` |
| **AAAA** | Address (IPv6) | Resolves a domain name to an **IPv6 (Internet Protocol version 6)** address. | `2606:4700:20::681a:be5` |
| **CNAME** | Canonical Name | Resolves a domain name to *another* domain name rather than an IP. A second lookup is then performed. | `store.tryhackme.com` resolving to `shops.shopify.com` |
| **MX** | Mail Exchange | Resolves to the servers that handle email for the domain. They include a priority flag to determine backup server order if the main one fails. | `alt1.aspmx.l.google.com` |
| **TXT** | Text | Free text fields used to store text-based data. Commonly used for domain ownership verification or email spoofing prevention. | `_acme-challenge.example.com TXT "token_value_here"` |

###  The DNS Request Journey
When I type a website into my browser, my computer goes through a specific sequence to find the IP address:

1.  **Local Cache:** My computer first checks its own local cache to see if I've visited the site recently. 
2.  **Recursive DNS Server:** If not found locally, the request goes to a recursive server, usually provided by my **ISP (Internet Service Provider)**. If this server has it cached, it returns the IP and the journey ends here.
3.  **Root DNS Server:** If the recursive server doesn't know the IP, it queries the root servers (the DNS backbone of the internet). The root server looks at the TLD (like `.com`) and redirects the request to the appropriate TLD server.
4.  **TLD Server:** The Top-Level Domain server holds the records for where to find the specific authoritative server for that domain, and passes that info along.
5.  **Authoritative DNS Server (Nameserver):** This server actually stores the final DNS records for the specific domain name (e.g., `kip.ns.cloudflare.com`). 

**The Return Trip & Caching:** 
The final DNS record is sent back to the Recursive DNS Server, which saves a local copy for future requests, and then delivers it back to my computer. All DNS records come with a **TTL (Time To Live)** value—a number in seconds dictating exactly how long my computer should save (cache) this answer before it needs to perform this entire lookup process again.

## 2 - HTTP(Hypertext Transfer Protocol) in Detail

**Objective:**
I explored the  protocols of the web: HTTP and HTTPS. I learned how URLs are structured, how my browser communicates with web servers through requests and responses, and the specific roles of HTTP methods, status codes, headers, and cookies.

### What are HTTP and HTTPS?
*   **HTTP (HyperText Transfer Protocol):** Developed by Tim Berners-Lee between 1989-1991, this is the core set of rules used for communicating with web servers to transmit webpage data (HTML, images, videos, etc.).
*   **HTTPS (HyperText Transfer Protocol Secure):** This is the secure, encrypted version of HTTP. It prevents attackers from intercepting or viewing my data and guarantees that I am communicating with the legitimate web server rather than an impersonator.

### Anatomy of a URL
A **URL (Uniform Resource Locator)** is essentially an instruction manual telling the browser how to access a specific resource. I broke down its main components:

*   **Scheme:** Instructs what protocol to use (e.g., `http://`, `https://`, `ftp://`).
*   **User:** Used for basic authentication (e.g., passing a username and password directly in the URL).
*   **Host:** The domain name or IP address of the server I want to access.
*   **Port:** The port to connect to (usually `80` for HTTP and `443` for HTTPS).
*   **Path:** The specific file or location on the server I am trying to reach.
*   **Query String:** Extra information sent to the path, usually starting with a `?` (e.g., `/blog?id=1`).
*   **Fragment:** A reference to a specific location on the requested page, helping to jump straight to a certain section.

### Making Requests and Responses
To get a rich web experience, my browser and the server exchange detailed blocks of text. 

**The HTTP Request:**
When I ask a server for a webpage, my browser sends a request that looks like this:
> `GET / HTTP/1.1` (The Method, Path, and Protocol Version)
> `Host: tryhackme.com` (The specific website I want)
> `User-Agent: Mozilla/5.0 Firefox/87.0` (Telling the server I am using Firefox)
> `Referer: https://tryhackme.com/` (The page that linked me here)
*Note: Requests always end with a blank line to tell the server the request is finished.*

**The HTTP Response:**
The server replies with a response that looks like this:
> `HTTP/1.1 200 OK` (Protocol version and the Status Code confirming success)
> `Server: nginx/1.15.8` (The server software)
> `Content-Type: text/html` (Telling my browser to expect HTML data)
> `Content-Length: 98` (The exact size of the response to ensure no data is missing)
*Note: After a blank line, the actual requested data (like the HTML code) is delivered.*

### HTTP Methods
Methods are how I show my intended action when making a request to the server:
*   **GET:** Used for retrieving/getting information from a web server.
*   **POST:** Used for submitting data to the server (like filling out a form or creating a record).
*   **PUT:** Used for submitting data to update existing information.
*   **DELETE:** Used for deleting information or records from the server.

### HTTP Status Codes
The first line of an HTTP response contains a status code. I learned how they are grouped into five ranges:

| Range | Meaning | Description |
| :--- | :--- | :--- |
| **100-199** | Information | The request was accepted; the client should continue sending the rest. |
| **200-299** | Success | The request was successfully completed. |
| **300-399** | Redirection | The request is redirected to another resource or website. |
| **400-499** | Client Error | Something was wrong with my request (e.g., missing parameters, bad syntax). |
| **500-599** | Server Error | The server encountered a major problem and couldn't handle the request. |

**Most Common Status Codes:**
*   **200 OK:** Everything was successful.
*   **201 Created:** A resource (like a new user profile) was successfully created.
*   **301 Moved Permanently:** The resource has permanently moved to a new location.
*   **302 Found:** A temporary redirect to a new location.
*   **400 Bad Request:** My browser sent a malformed or missing request.
*   **401 Not Authorised:** I need to log in to view this resource.
*   **403 Forbidden:** I do not have permission to view this, even if logged in.
*   **404 Page Not Found:** The resource simply does not exist.
*   **405 Method Not Allowed:** Using the wrong method (e.g., trying to `GET` a page that requires a `POST`).
*   **500 Internal Service Error:** The server crashed or encountered an unhandled error.
*   **503 Service Unavailable:** The server is overloaded or down for maintenance.

### HTTP Headers
Headers are extra bits of data sent between my browser and the server. 

**Common Request Headers (From Me to the Server):**
*   **Host:** Specifies which website I want if the server hosts multiple domains.
*   **User-Agent:** Identifies my browser so the server can format the site correctly.
*   **Content-Length:** Tells the server how much data I am sending in my request.
*   **Accept-Encoding:** Tells the server what compression methods my browser supports.
*   **Cookie:** Sends stored data back to the server to remind it who I am.

**Common Response Headers (From the Server to Me):**
*   **Set-Cookie:** Gives my browser data to store and send back on future requests.
*   **Cache-Control:** Dictates how long my browser should save the response locally.
*   **Content-Type:** Tells my browser what kind of file it is receiving (HTML, PDF, Image, etc.).
*   **Content-Encoding:** Tells my browser how the data was compressed for transit.

### Cookies
Because HTTP is "stateless" (it forgets who I am after every single request), **Cookies** act as a memory bank. 
*   When I log in, the server sends a `Set-Cookie` header containing a unique, secret token.
*   My browser saves this token. On every subsequent request, my browser automatically sends a `Cookie` header back to the server.
*   This is how the server remembers my authentication, my personal settings, or my shopping cart without making me log in on every single page click. I can inspect these at any time using my browser's Developer Tools under the "Network" tab.

## 3 - How Websites Work


**Objective:**
I explored the fundamental components of how websites are built, focusing on the differences between the front end and back end. I learned how HTML and JavaScript function together, and I was introduced to my first client-side vulnerabilities: Sensitive Data Exposure and HTML Injection.

### The Two Major Components of a Website
*   **Front End (Client-Side):** How my browser (like Chrome or Safari) renders the website so I can see and interact with it.
*   **Back End (Server-Side):** A dedicated computer (server) somewhere else in the world that processes my requests and returns the necessary data.

### The Building Blocks of the Web
Websites are primarily created using three core languages:
*   **HTML (HyperText Markup Language):** Builds the website and defines its core structure.
*   **CSS (Cascading Style Sheets):** Makes the website visually appealing by adding styling options (colors, fonts, layouts).
*   **JavaScript (JS):** Implements complex features and interactivity (animations, button clicks, dynamic updates).

### HTML Structure and Attributes
HTML uses elements (also known as tags) to tell the browser how to display content. I broke down the standard structure of an HTML document:
*   `<!DOCTYPE html>`: Tells the browser to use HTML5 to interpret the page for standardization.
*   `<html>`: The root element; all other elements sit inside this tag.
*   `<head>`: Contains background information about the page, such as the page title.
*   `<body>`: The actual visible content of the page. Only content inside the body is shown in the browser.
*   `<h1>` and `<p>`: Elements used for large headings and paragraphs, respectively.

**Attributes:** 
Tags can contain extra data called attributes, which serve unique purposes:
*   **class:** Used to style an element (e.g., `<p class="bold-text">`). Multiple elements can share the same class.
*   **id:** A unique identifier for a specific element, used for styling or targeting with JavaScript (e.g., `<p id="example">`). Unlike classes, an ID must be unique to one element.
*   **src (source):** Specifies the location of a resource, commonly used for displaying images (e.g., `<img src="img/cat.jpg">`).

### JavaScript Interactivity
Without JavaScript, a page is entirely static. JS allows the page to dynamically update in real-time.
*   It can be loaded directly inside `<script>` tags or included remotely using the `src` (source) attribute.
*   JS can target specific HTML elements to change them dynamically: `document.getElementById("demo").innerHTML = "Hack the Planet";`
*   HTML elements can use **events** to trigger JS, such as `onclick` or `onhover` (e.g., `<button onclick='...'>Click Me!</button>`).

### Vulnerability 1: Sensitive Data Exposure
This occurs when a developer fails to protect or remove sensitive clear-text information from the front-end code. 
*   By simply right-clicking and selecting "View Page Source," I can inspect a website's raw HTML and JS.
*   Sometimes, developers accidentally leave HTML comments containing temporary login credentials, hidden links to private admin panels, or other sensitive data. Reviewing the page source code is one of the first steps in assessing web application security.

### Vulnerability 2: HTML Injection
This vulnerability occurs when a website takes user input but fails to **sanitize** it (filtering out malicious text or code before processing it).
*   If a website implicitly trusts user input and displays it directly on the page, I can type raw HTML into a text box (like injecting a malicious hyperlink).
*   The browser will execute my input as actual code instead of plain text, allowing me to alter the page's appearance, hijack functionality, or trick users into clicking bad links. 
*   **The Golden Rule of Web Security:** Never trust user input. Developers must sanitize everything a user enters.

## 4 - Putting it all together




