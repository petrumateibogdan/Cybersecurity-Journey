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
