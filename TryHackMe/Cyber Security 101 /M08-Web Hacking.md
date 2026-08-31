# Module 8 : Web Hacking

# 1. Web Application Basics


I explored the core foundational concepts of web applications, including client-server architecture, URL structures, HTTP request/response mechanics, message bodies, and essential security headers.

---

##  Web Application Architecture: Front End vs. Back End
A web application functions through the interaction between visible client-side components and underlying server-side infrastructure.

### Front End (Client-Side)
The front end runs inside the **web browser** (the tool used to interact with web apps):
* **HTML (Hypertext Markup Language):** The foundational structure and instructions that dictate what the browser displays.
* **CSS (Cascading Style Sheets):** Defines styling, visual layout, colors, and typography.
* **JavaScript (JS):** Handles dynamic behavior, client-side logic, and complex decision-making in the browser.

### Back End (Server-Side)
The back end consists of non-visual services that process logic and maintain state:
* **Web Server:** Responsible for hosting and delivering content to clients.
* **Database:** Stores, modifies, and retrieves persistent data (such as user preferences and account data).
* **Web Application Firewall (WAF):** An optional protective layer that inspects and filters incoming traffic to block malicious attacks before reaching the web server.

---

##  Anatomy of a URL (Uniform Resource Locator)
A URL guides the web browser to the exact resource requested on the internet:

$$\text{Scheme} \, \text{://} \, [\text{User}@] \, \text{Host/Domain} \, [:\text{Port}] \, \text{/Path} \, [?\text{Query String}] \, [\#\text{Fragment}]$$

* **Scheme:** The protocol used (e.g., `http` or `https`; HTTPS provides encrypted communication).
* **User:** Optional authentication credentials (rarely used due to cleartext exposure risks).
* **Host/Domain:** The unique domain name identifying the website.
  * *Security Note:* Attackers frequently register misspelled variations of popular domains (**typosquatting**) for phishing campaigns.
* **Port:** The communication port on the web server (ranges from 1–65,535; default is 80 for HTTP, 443 for HTTPS).
* **Path:** The specific directory/file roadmap pointing to the requested endpoint or resource.
* **Query String:** Starts with `?` and passes additional data to the server using `key=value` pairs (must be sanitized to prevent injection attacks).
* **Fragment:** Starts with `#` and directs the browser to a specific section/anchor on the page.

---

##  HTTP Message Structure
HTTP communication consists of **HTTP Requests** (sent by the client) and **HTTP Responses** (returned by the server). Both share a standardized four-part format:

1. **Start Line:** The Request Line (for requests) or Status Line (for responses).
2. **Headers:** Key-value pairs providing instructions and metadata.
3. **Empty Line:** A mandatory blank line separating headers from the message body.
4. **Body:** The payload data (form data in requests; HTML/JSON in responses).

---

##  HTTP Requests & Methods

### Request Line Format
`METHOD /path HTTP/version` (e.g., `GET /index.html HTTP/1.1`)

### HTTP Request Methods & Security Considerations
* **`GET`:** Fetches data without modifying state. *Security:* Never transmit sensitive tokens or passwords in GET parameters, as they are exposed in plaintext URLs and logs.
* **`POST`:** Submits data to create or update resources. *Security:* Requires strict input validation against SQLi and XSS.
* **`PUT`:** Replaces/updates a target resource completely. *Security:* Must require strict authorization checks.
* **`DELETE`:** Removes a target resource. *Security:* Requires proper access control.
* **`PATCH`:** Applies partial modifications to a resource.
* **`HEAD`:** Operates like `GET`, but requests only headers without the response body.
* **`OPTIONS`:** Describes available communication options and supported HTTP methods for the target resource.
* **`TRACE`:** Performs a loop-back test for debugging (often disabled on production servers for security).
* **`CONNECT`:** Establishes a tunnel (commonly used for HTTPS connections).

### HTTP Protocol Versions
* **HTTP/0.9 (1991):** Primitive version supporting only `GET`.
* **HTTP/1.0 (1996):** Introduced headers, caching, and multi-content type support.
* **HTTP/1.1 (1997):** The most widely adopted version; introduced persistent connections and chunked transfer encoding.
* **HTTP/2 (2015):** Added multiplexing, header compression, and prioritization.
* **HTTP/3 (2022):** Utilizes the QUIC protocol for faster, more secure connections.

---

##  Request Headers & Body Formats

### Common Request Headers
* `Host:` Specifies the domain name of the destination web server.
* `User-Agent:` Shares client browser and operating system details.
* `Referer:` Indicates the previous URL from which the request originated.
* `Cookie:` Passes stored key-value session data back to the server.
* `Content-Type:` Describes the MIME type/format of the request body data.

### Request Body Formats
* **`application/x-www-form-urlencoded`:** Default form format where key-value pairs are joined with `&` (e.g., `name=Aleksandra&age=27`) and special characters are percent-encoded.
* **`multipart/form-data`:** Uses boundary strings to separate multi-part blocks; required for uploading binary files and images.
* **`application/json`:** Formats data inside curly braces as JSON key-value pairs (`{"key": "value"}`).
* **`application/xml`:** Formats data inside nested XML opening and closing tags (`<user><name>Aleksandra</name></user>`).

---

##  HTTP Responses & Status Codes

### Status Line Format
`HTTP/version StatusCode ReasonPhrase` (e.g., `HTTP/1.1 200 OK`)

### Status Code Categories
* **100–199 (Informational):** Server received initial request and is waiting for completion (e.g., `100 Continue`).
* **200–299 (Successful):** Request processed successfully (e.g., `200 OK`).
* **300–399 (Redirection):** Resource has moved; client must take further action (e.g., `301 Moved Permanently`).
* **400–499 (Client Error):** The client submitted an invalid request (e.g., `404 Not Found`).
* **500–599 (Server Error):** The server failed to fulfill a valid request due to an internal issue (e.g., `500 Internal Server Error`).

---

##  Response Headers & Cookie Security

### Standard Response Headers
* `Date:` Timestamp when the response was generated.
* `Content-Type:` MIME type and character set of the response body (e.g., `text/html; charset=utf-8`).
* `Server:` Discloses server software and version (e.g., `Server: nginx`; recommended to obscure or remove to prevent banner grabbing).
* `Cache-Control:` Dictates client-side caching policies (e.g., `max-age=600` or `no-cache`).
* `Location:` Specifies the redirect destination URL in `3xx` responses (must be validated to prevent open redirects).

### `Set-Cookie` Security Flags
* **`Secure`:** Ensures the browser only transmits the cookie over encrypted **HTTPS** connections.
* **`HttpOnly`:** Prevents client-side JavaScript from accessing the cookie, mitigating session hijacking via **Cross-Site Scripting (XSS)**.

---

##  HTTP Security Headers

```http
Content-Security-Policy: default-src 'self'; script-src 'self' [https://cdn.tryhackme.com](https://cdn.tryhackme.com); style-src 'self'
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin

Content-Security-Policy (CSP)
Mitigates Cross-Site Scripting (XSS) and injection attacks by establishing an allowlist of trusted content origins:

default-src 'self': Restricts all fallbacks to the current origin only.

script-src: Restricts the exact domains from which JavaScript files can execute.

style-src: Restricts origins for CSS stylesheets.

Strict-Transport-Security (HSTS)
Enforces that browsers only interact with the website over HTTPS:

max-age: Time duration (in seconds) the browser must enforce HTTPS.

includeSubDomains: Applies the HTTPS-only policy to all subdomains under the root domain.

preload: Authorizes the site to be hardcoded into browser HSTS preload lists.

X-Content-Type-Options
nosniff: Prevents browsers from MIME-sniffing a response away from the declared Content-Type, protecting against malicious content interpretation.

Referrer-Policy
Controls how much referral information is sent in the Referer header during navigation:

no-referrer: Completely disables sending referrer data.

same-origin: Sends referrer data only when navigating within the exact same origin.

strict-origin: Sends only the origin domain, and only when the protocol remains HTTPS-to-HTTPS.

strict-origin-when-cross-origin: Sends full URL paths on same-origin requests, but restricts cross-origin HTTPS requests to origin-only data.

```
# 2. JavaScript Essentials

# JavaScript for Cybersecurity: The Basics

I learned that JavaScript (JS) is a scripting language used to add interactive features to websites built with HTML and CSS. From a cybersecurity perspective, it is crucial to understand how legitimate JS functionalities can be exploited for malicious purposes.

Here are my notes on JS fundamentals, how it is integrated into web applications, and how to analyze it during an engagement.

---

##  Core JS Concepts
I learned the essential building blocks that make up JavaScript programs. Since JS is an interpreted language, the code is executed directly in the browser without prior compilation, which makes it incredibly easy to interact with using the Google Chrome Console.

* **Variables:** These act as containers to store data values. 
* **Variable Declarations:** They can be declared using `var` (function-scoped), `let` (block-scoped), or `const` (block-scoped).
* **Data Types:** JS handles data types like strings (text), numbers, booleans (true/false), null, undefined, and objects.
* **Functions:** Functions represent blocks of code designed to perform specific tasks, preventing the need to write repetitive code.
* **Loops:** Loops (such as `for`, `while`, and `do...while`) allow code blocks to run multiple times as long as a certain condition remains true.

---

##  Integrating JS into HTML
I learned that JS is typically not used to render content directly, but rather works alongside HTML and CSS. There are two main ways developers integrate JS:

* **Internal JS:** The JS code is embedded directly within the HTML document inside `<script>` tags. These tags can be placed in the `<head>` or `<body>` sections.
* **External JS:** The JS code is stored in a separate file with a `.js` extension. It is linked to the HTML document using the `src` attribute within a `<script>` tag (e.g., `<script src="script.js"></script>`). This keeps HTML documents clean and easier to maintain.
* **Verification:** During a penetration test, I can determine if a website uses internal or external JS simply by viewing the page's source code.

---

##  Abusing Dialogue Functions
JS includes built-in functions designed for user interaction. However, I learned that if these are not implemented securely, attackers can exploit them for attacks like Cross-Site Scripting (XSS).

* **`alert`:** Displays a message box with an "OK" button.
* **`prompt`:** Displays a box asking the user for input and returns that value.
* **`confirm`:** Displays a message with "OK" and "Cancel" buttons, returning a boolean value (`true` or `false`).
* **The Exploit:** A bad actor can craft malicious JS (such as putting an `alert` inside a loop that runs 500 times) to severely disrupt a user's browsing experience.

---

##  Control Flow and Bypassing Login Forms
Control flow statements (like `if-else` and `switch`) determine the order in which code executes based on specific conditions.

* **The Vulnerability:** Developers sometimes use JS to handle authentication directly on the client side. 
* **The Bypass:** Relying entirely on JS for form validation or authentication is extremely insecure. Because the code runs in the user's browser, an attacker can easily view the source code to find hardcoded passwords, manipulate the JS, or bypass the logic entirely.

---

##  Minification and Obfuscation
When reviewing source code, I learned that developers often alter the readability of their JS files for production.

* **Minification:** This is the process of compressing JS files by removing spaces, line breaks, and comments to improve loading times. 
* **Obfuscation:** This technique makes the code deliberately difficult for humans to understand by renaming variables to meaningless characters and inserting dummy code. 
* **Execution:** Even though obfuscated code looks like complete gibberish, the browser can still execute it perfectly.
* **Deobfuscation:** I can use online deobfuscation tools to translate the gibberish back into a human-readable format, making it easier to analyze the script's true function.

---

##  JS Security Best Practices
Based on these vulnerabilities, I learned several best practices for securing JS code:

* **Do not rely solely on client-side validation:** Validation must always be performed on the server side as well, since users can manipulate client-side JS.
* **Do not add untrusted libraries:** Attackers frequently upload malicious libraries with names that closely resemble legitimate ones.
* **Never hardcode secrets:** Sensitive data like API keys, access tokens, or passwords should never be hardcoded in JS, as users can easily view the source code to extract them.
* **Minify and obfuscate production code:** While it will not stop a determined attacker from reverse-engineering the script, it does increase the effort required.

# 3. SQL FUNDAMENTALS

# SQL Fundamentals: Database Basics and Queries

I learned that databases are ubiquitous in cybersecurity, used in web applications, SOCs, SIEMs, and malware analysis. Understanding them is crucial for identifying SQL vulnerabilities offensively and for navigating data defensively. Here is my cheat sheet and summary of SQL and database fundamentals.

---

##  Database Concepts
* I learned that databases are organized collections of structured information or data that can be easily accessed and manipulated.
* I discovered that relational databases store structured data in rows and columns within tables.
* I learned that non-relational databases store data in a non-tabular format, which is useful when data formats vary greatly.
* I found out that a primary key uniquely identifies each record stored in a table.
* I learned that a foreign key is a column that links two tables together by referencing a column in another table.

---

##  Basic Database & Table Statements
I learned how to manage databases and tables using basic SQL statements.

* **Create a database**: I can create a new database using `CREATE DATABASE database_name;`.
* **Select a database**: I must tell MySQL which database to interact with using `USE database_name;`.
* **Show databases/tables**: I can list databases using `SHOW DATABASES;` and list tables using `SHOW TABLES;`.
* **Describe a table**: I learned that `DESCRIBE table_name;` (or `DESC`) shows the columns and data types within a table.
* **Create a table**: I can build a table by defining columns and data types.
  ```sql
  CREATE TABLE book_inventory (
      book_id INT AUTO_INCREMENT PRIMARY KEY,
      book_name VARCHAR(255) NOT NULL,
      publication_date DATE
  );

Alter a table: I can modify an existing table using ALTER TABLE table_name ADD column_name data_type;.

Drop: I learned that DROP DATABASE database_name; and DROP TABLE table_name; will delete them entirely.

 CRUD Operations
I learned that CRUD stands for Create, Read, Update, and Delete, which are the fundamental data operations.

Create (INSERT): I learned to add new records using the INSERT INTO statement.

INSERT INTO books (id, name, published_date, description) VALUES (1, "Book Name", "2024-01-01", "A description");**

Read (SELECT): I can retrieve information from a table using the SELECT statement.

SELECT name, description FROM books;

Update (UPDATE): I found out how to modify an existing record using the UPDATE statement combined with SET and WHERE.

UPDATE books SET description = "New description" WHERE id = 1;


Delete (DELETE): I can remove records from a table using the DELETE FROM statement.

SQL
DELETE FROM books WHERE id = 1;

I learned that clauses help define how data should be retrieved, sorted, or grouped.

I learned that the DISTINCT clause avoids duplicate records, returning only unique values.

I can aggregate data from multiple records using the GROUP BY clause.

I learned to sort records in ascending (ASC) or descending (DESC) order using ORDER BY.

I discovered that HAVING filters groups or results after an aggregation is performed.


Operators
I learned how to use operators to filter data effectively.

I learned to use LIKE for filtering specific patterns within a column (e.g., WHERE description LIKE "%guide%").

I can combine conditions using AND (all conditions must be true) and OR (at least one condition must be true).

I learned that NOT reverses a boolean operator to exclude a condition.

I found out how to test if a value exists within a range using BETWEEN.

I learned standard comparison operators like = (Equal), != (Not Equal), < (Less Than), > (Greater Than), <= (Less Than or Equal), and >= (Greater Than or Equal).

SQL Functions
I learned that functions streamline queries and manipulate data.

String Functions
I learned that CONCAT() combines two or more strings together.

I discovered that GROUP_CONCAT() concatenates data from multiple rows into one field.

I can extract a portion of a string using SUBSTRING().

I learned that LENGTH() returns the number of characters in a string.

Aggregate Functions
I learned that COUNT() returns the number of records within an expression.

I can calculate the total sum of a column using SUM().

I found out that MAX() retrieves the highest value, and MIN() retrieves the lowest value in a column.

Here are some practical queries I wrote during my lab exercises to extract specific data from the database:

Finding the tool with the longest name:

SELECT name, LENGTH(name) AS name_length FROM hacking_tools ORDER BY name_length DESC LIMIT 1;

Calculating the total sum of all tools:

SELECT SUM(amount) FROM hacking_tools;
Grouping tool names where the amount does not end in 0:

ELECT GROUP_CONCAT(name SEPARATOR ' & ') AS tools FROM hacking_tools WHERE amount % 10 != 0;

# 4. Burp Suite: The Basics

# Burp Suite: Basics & Proxy Cheat Sheet

I recently started exploring **Burp Suite**, the industry-standard framework for web application penetration testing. This tool is essential for intercepting, manipulating, and analyzing HTTP/HTTPS traffic between a browser and a web server. 

Here are my notes on setting it up, navigating the interface, configuring the proxy, and bypassing client-side filters.

---

## 1. What is Burp Suite?
Burp Suite is a Java-based framework that captures and allows manipulation of web traffic. I am using the **Community Edition** (free for non-commercial use). 

*Note: The Professional Edition includes automated scanning, unrestricted brute-forcing, and the Burp Collaborator, while the Enterprise Edition is used for continuous infrastructure scanning.*

### Core Components I Learned About:
* **Proxy:** The heart of Burp. Intercepts and allows modification of requests/responses.
* **Repeater:** Captures a request and lets me manually modify and resend it multiple times (great for SQLi or testing endpoints).
* **Intruder:** Sprays endpoints with payloads for brute-forcing or fuzzing (rate-limited in the Community Edition).
* **Decoder:** Quickly decodes or encodes data (e.g., URL encoding, Base64).
* **Comparer:** Compares two pieces of data byte-by-byte or word-by-word.
* **Sequencer:** Analyzes the randomness of session tokens.
* **Extender (BApp Store):** Allows adding third-party modules (Java, Python, Ruby) to extend functionality (like `Logger++`).

---

## 2. Navigating the Interface
The Burp dashboard can be overwhelming, but the navigation is logical.

* **Top Menu Bar:** Used to switch between the main modules (Proxy, Repeater, Intruder, etc.).
* **Second Menu Bar:** Contains sub-tabs specific to the selected module (e.g., Proxy -> Intercept).
* **Settings:** Found via the top right gear icon. 
  * *Global Settings:* Affects the entire installation.
  * *Project Settings:* Specific to the current session (lost on closing in the Community Edition).
* **Detaching Tabs:** `Window -> Detach` allows me to pull a tab into its own window for a multi-monitor setup.

**Keyboard Shortcuts I Use Constantly:**
* `Ctrl + Shift + P` : Proxy tab
* `Ctrl + Shift + R` : Repeater tab
* `Ctrl + Shift + I` : Intruder tab
* `Ctrl + Shift + D` : Dashboard
* `Ctrl + U` : URL encode selected text (crucial when injecting payloads).

---

## 3. Configuring the Browser Proxy
To get my browser traffic to flow through Burp Suite, I need to configure a proxy.

### Option 1: Using FoxyProxy (Standard Browser)
1. Install the **FoxyProxy** extension in Firefox.
2. Add a new configuration:
   * **Title:** Burp
   * **Proxy IP:** `127.0.0.1`
   * **Port:** `8080`
3. Activate the configuration in FoxyProxy.
4. Ensure **Intercept is on** in the Burp Proxy tab. The browser will hang when navigating until I forward or drop the request in Burp.

### Option 2: Using Burp's Built-in Browser
Burp includes a pre-configured Chromium browser.
* In the Proxy tab, I can just click **Open Browser**. Traffic automatically routes through Burp without any plugin configuration.
* *Linux Root Note:* If running on Linux as root (like the AttackBox), the built-in browser might fail to launch due to sandbox restrictions. I can fix this by going to `Settings -> Tools -> Burp's browser` and checking `Allow Burp's browser to run without a sandbox`.

### Fixing HTTPS/TLS Certificate Errors
When routing HTTPS traffic through Burp, my browser will show a security warning because it doesn't trust Burp's certificate.
1. With the Burp Proxy running, visit `http://burp/cert` in my browser to download `cacert.der`.
2. Open Firefox Settings -> Search "Certificates" -> **View Certificates**.
3. Click **Import**, select `cacert.der`.
4. Check **"Trust this CA to identify websites"** and hit OK.

---

##  Scoping & Filtering Traffic
If I don't set a scope, Burp will capture background traffic from every open tab, which creates a massive mess.

 **Add to Scope:** Go to the `Target -> Site map` tab, right-click the target domain, and select **Add To Scope**.
 **Stop Logging Out-of-Scope Traffic:** Click "Yes" when Burp prompts to stop logging out-of-scope items.
 **Filter Interceptions:** Go to the `Proxy -> Proxy settings` tab. Under "Intercept Client Requests", check **"And URL Is in target scope"**. 
*This ensures Burp only pauses and intercepts traffic for the exact app I am testing.*

---

## Practical Exploitation: Bypassing Client-Side Filters
During my lab practice, I used the Burp Proxy to bypass a client-side filter and execute a Reflected Cross-Site Scripting (XSS) attack.

**The Scenario:**
A support form asked for an email address. If I typed `<script>alert("XSS")</script>`, the browser stopped me, stating it wasn't a valid email format. This was a **client-side filter** running in my browser, which is easily bypassed.

**The Bypass Methodology:**
 I turned **Intercept On** in Burp Suite.
 In the web browser, I typed a legitimate email (`test@example.com`) to satisfy the client-side filter and clicked Submit.
 The request was caught by Burp Suite *before* reaching the server.
 Inside Burp, I deleted `test@example.com` and replaced it with my payload: `<script>alert("Succ3ssful XSS")</script>`.
 I highlighted the payload and pressed `Ctrl + U` to URL encode it.
 I clicked **Forward**.
 The server accepted the payload, and an alert box popped up on the webpage, proving the XSS vulnerability!
