# Unit 4 - Server Configuration and Network Services (Theory Questions with Examples)

---

## 1. Define DNS Lookup Process in Detail (with Example)
DNS (Domain Name System) lookup is the process of translating a human-readable domain name (like `www.example.com`) into a machine-understandable IP address (like `192.0.2.1`).

**Example:** When you type `www.google.com`, your system goes through the DNS lookup process to get Google’s IP address so it can connect to the correct server.

**Detailed Steps:**
1. **User Request**: User types `www.google.com` in browser.
2. **Browser Cache**: Checks if IP is stored.
3. **OS Resolver Cache**: If not, OS resolver checks its cache.
4. **Recursive DNS Resolver**: If still not found, OS queries ISP’s recursive DNS resolver.
5. **Root Server Lookup**: Resolver asks root server where `.com` TLD is managed.
6. **TLD Server Lookup**: Points to the server handling `google.com`.
7. **Authoritative Server**: Returns IP like `142.250.182.68`.
8. **Response**: Browser connects to the returned IP to load the site.

---

## 2. Explain the Core Working of httpd Server (with Example)
An `httpd` server serves content over HTTP/S. Apache is a popular implementation.

**Example:** When someone visits `http://yourdomain.com`, Apache listens, processes the request, and serves the appropriate web page.

**Working:**
1. **Listening** on port 80 or 443.
2. **Parses** GET/POST requests.
3. **Serves** static content from `/var/www/html/`.
4. **Handles dynamic content** (e.g., PHP) via modules.
5. **Responds** with headers and data.
6. **Logs** every transaction.
7. **Modular Architecture** for added features (SSL, redirects).

---

## 3. Core Functionality of DNS Server (with Example)
DNS servers convert domain names into IP addresses.

**Example:** A DNS server converts `www.wikipedia.org` to its IP like `208.80.154.224`.

**Functions:**
- Name resolution
- Caching
- Forwarding queries
- Hosting records (A, MX, TXT, etc.)
- DNSSEC for security

---

## 4. Difference Between Apache and FTPD Server (with Example)
| Feature        | Apache Server                         | FTPD Server                            |
|----------------|--------------------------------------|----------------------------------------|
| Purpose        | Serve websites                       | Transfer files                         |
| Protocol       | HTTP/HTTPS                           | FTP                                    |
| Example        | Visit `http://site.com/index.html`  | Upload files to server via FileZilla   |

---

## 5. What is DHCP? Explain (with Example)
DHCP dynamically assigns IP addresses to devices.

**Example:** When a laptop connects to a Wi-Fi network, the DHCP server assigns it an IP like `192.168.0.102`.

**Steps:**
1. Device broadcasts for IP
2. DHCP server offers address
3. Device requests lease
4. Server confirms with acknowledgment

---

## 6. (Repeated) Difference Between Apache and FTPD Server
*Refer to Answer 4.*

---

## 7. Steps to Configure DHCP Server (with Example)
**Example Configuration:**
1. **Install DHCP:**
```bash
sudo apt install isc-dhcp-server
```
2. **Edit dhcpd.conf:**
```conf
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.50 192.168.10.150;
  option routers 192.168.10.1;
  option domain-name-servers 8.8.8.8;
}
```
3. **Assign interface** in `/etc/default/isc-dhcp-server`.
4. **Restart service:**
```bash
sudo systemctl restart isc-dhcp-server
```

---

## 8. Configuration Steps for Apache Server (with Example)
1. **Install:**
```bash
sudo apt install apache2
```
2. **Place HTML file:** `/var/www/html/index.html`
3. **Edit Virtual Host:**
```conf
<VirtualHost *:80>
  ServerName example.com
  DocumentRoot /var/www/html/
</VirtualHost>
```
4. **Enable mod_rewrite:**
```bash
sudo a2enmod rewrite
```
5. **Restart Apache:**
```bash
sudo systemctl restart apache2
```
6. **Visit Site:** `http://localhost`

---

## 9. What is MUA and MTA? Explain (with Example)
**MUA** (e.g., Thunderbird): Sends and receives emails.
**MTA** (e.g., Postfix): Transfers email between servers.

**Example:**
- Alice composes email in Outlook (MUA)
- Outlook sends email to Gmail’s server using SMTP (MTA)
- Gmail’s MTA delivers to Bob’s mail server
- Bob opens email using his MUA (e.g., mobile app)

---

## 10. What is Apache Web Server? Explain (with Example)
Apache is open-source web server software.

**Example:** Hosting a portfolio site by placing `index.html` in Apache’s root folder.

**Features:**
- Modules (mod_ssl, mod_rewrite)
- Virtual hosts
- Logging
- Works on Linux and Windows

---

## 11. Opening Mail Server for External Mail (with Example)
**Example:** Hosting your own mail server to send/receive using `mail.yourdomain.com`.

**Steps:**
1. Install & configure Postfix.
2. Open ports 25, 587, 993.
3. Set DNS records: MX, SPF, DKIM, DMARC.
4. Use SSL (Let's Encrypt).
5. Test with telnet or mail tester tools.

---

## 12. (Repeated) Core Working of httpd Server
*Refer to Answer 2.*

---

## 13. (Repeated) Steps to Configure DHCP Server
*Refer to Answer 7.*

---

## 14. (Repeated) Define DNS Lookup Process
*Refer to Answer 1.*

---

## 15. (Repeated) Apache Configuration Steps
*Refer to Answer 8.*

---



