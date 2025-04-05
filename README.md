# Unit 4 - Server Configuration and Network Services (Theory Questions)

---

## 1. Define DNS Lookup Process in Detail
DNS (Domain Name System) lookup is the process of translating a human-readable domain name (like `www.example.com`) into a machine-understandable IP address (like `192.0.2.1`). This process is essential for browsing the internet, as users typically remember domain names rather than IP addresses.

**Detailed Steps:**
1. **User Request**: The user types a URL into the web browser.
2. **Browser Cache**: The browser first checks its local cache to see if it has the IP address stored from a previous lookup.
3. **OS Resolver Cache**: If not found, the browser asks the operating system’s DNS resolver.
4. **Recursive DNS Resolver**: If the OS doesn’t have it cached, it sends a query to a recursive DNS resolver (usually provided by the ISP).
5. **Root Server Lookup**: The resolver sends a request to a root server. Root servers provide information about top-level domain (TLD) servers (like `.com`, `.org`).
6. **TLD Server Lookup**: The root server responds with the address of a TLD server. The resolver then queries this server.
7. **Authoritative Server Lookup**: The TLD server replies with the address of the authoritative DNS server that holds the actual IP address of the domain.
8. **IP Address Retrieval**: The authoritative server responds with the IP address of the domain.
9. **Return to Client**: The resolver sends this IP back to the client, and the browser can now send a request to the web server using this IP.

---

## 2. Explain the Core Working of httpd Server
An `httpd` server (short for HTTP Daemon) is a software program that serves web content over the HTTP/HTTPS protocols. Apache HTTP Server is a widely used `httpd`.

**Core Working Principles:**
1. **Listening for Requests**: The httpd server listens on specified ports (usually 80 for HTTP and 443 for HTTPS) for incoming web requests from clients.
2. **Request Parsing**: When a request comes in (e.g., a GET or POST request), it parses the request and determines what is being asked (e.g., access a webpage, submit a form).
3. **Content Handling**: If the requested file (e.g., HTML, image, script) exists, the server retrieves it from its document root directory.
4. **Dynamic Content Handling**: If the request involves dynamic content (e.g., PHP or Python scripts), the server passes the request to an appropriate processor using modules like mod_php.
5. **Response Generation**: The server generates a response header and attaches the content body (HTML page, image, etc.).
6. **Logging**: The server logs the details of the request in its access and error logs for monitoring and troubleshooting.
7. **Modules**: httpd supports dynamic loading of modules to add features like SSL (mod_ssl), URL rewriting (mod_rewrite), compression, and more.

---

## 3. Core Functionality of DNS Server
A DNS server is responsible for resolving domain names to IP addresses. This makes internet browsing user-friendly, as users can remember names instead of numbers.

**Core Functions:**
1. **Name Resolution**: Converts a domain name (e.g., www.example.com) into its corresponding IP address.
2. **Zone Management**: DNS servers manage different zones, which are parts of the DNS namespace. Each zone contains resource records for domain names.
3. **Caching**: Stores previous query results temporarily to speed up future lookups and reduce DNS traffic.
4. **Forwarding Queries**: Can forward requests it can't resolve to another DNS server.
5. **Record Support**: Maintains various record types:
   - A (IPv4 address)
   - AAAA (IPv6 address)
   - MX (Mail exchange)
   - CNAME (Canonical name/alias)
   - TXT (Text records, used for SPF/DKIM)
6. **Security Extensions (DNSSEC)**: Provides data integrity and authenticity to prevent spoofing.

---

## 4. Difference Between Apache and FTPD Server

| Feature         | Apache Server                           | FTPD Server                                   |
|----------------|------------------------------------------|-----------------------------------------------|
| Purpose         | Serve web pages and content             | Transfer files over the network               |
| Protocol        | HTTP/HTTPS                              | FTP                                           |
| Ports           | 80, 443                                 | 21, (20 for data)                             |
| Authentication  | Optional, based on configuration        | Usually required (username/password)         |
| Use Case        | Websites, APIs, web applications        | File uploading and downloading                |
| Clients         | Web browsers (Chrome, Firefox)          | FTP clients (FileZilla, WinSCP, command line) |
| Encryption      | Via HTTPS using SSL/TLS                 | Requires FTPS/SFTP for secure transfers       |

---

## 5. What is DHCP? Explain
DHCP (Dynamic Host Configuration Protocol) is a network management protocol used to dynamically assign IP addresses to devices in a network.

**Detailed Explanation:**
- When a device connects to the network, it sends a broadcast message to discover DHCP servers.
- The DHCP server replies with an IP address and other configuration settings (subnet mask, default gateway, DNS server).
- The device accepts the offer and uses the IP for the lease time specified by the server.
- After lease expiration, the device must renew the IP or request a new one.

**Benefits:**
- Reduces manual configuration effort.
- Prevents IP conflicts.
- Supports mobile and dynamic networks.

---

## 6. (Repeated) Difference Between Apache and FTPD Server
*Refer to Answer 4 for complete comparison.*

---

## 7. Steps to Configure DHCP Server
**Step-by-Step Guide:**

1. **Install DHCP Server Package:**
```bash
sudo apt install isc-dhcp-server
```

2. **Edit Configuration File:**
Open `/etc/dhcp/dhcpd.conf` and add:
```conf
subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.100 192.168.1.200;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    default-lease-time 600;
    max-lease-time 7200;
}
```

3. **Assign Network Interface:**
Edit `/etc/default/isc-dhcp-server` and specify the interface (e.g., `eth0`).

4. **Start DHCP Service:**
```bash
sudo systemctl restart isc-dhcp-server
```

5. **Enable on Boot:**
```bash
sudo systemctl enable isc-dhcp-server
```

6. **Verify Operation:**
Check log files or use a client to test IP assignment.

---

## 8. Configuration Steps for Apache Server
1. **Install Apache**:
```bash
sudo apt install apache2
```

2. **Start Apache**:
```bash
sudo systemctl start apache2
```

3. **Document Root**:
Place your website files (HTML, CSS, JS) in `/var/www/html/`.

4. **Virtual Host Configuration**:
Edit `/etc/apache2/sites-available/000-default.conf` to set ServerName, DocumentRoot, etc.

5. **Enable Modules**:
```bash
sudo a2enmod rewrite
```

6. **Restart Apache**:
```bash
sudo systemctl restart apache2
```

7. **Configure Firewall**:
```bash
sudo ufw allow 'Apache Full'
```

8. **Testing**:
Open a browser and go to `http://your-server-ip`.

---

## 9. What is MUA and MTA? Explain
- **MUA (Mail User Agent)**: An application used by users to compose, send, and read emails. Examples: Outlook, Thunderbird.
- **MTA (Mail Transfer Agent)**: Software that transfers emails between mail servers. Examples: Postfix, Sendmail, Exim.

**Workflow:**
1. User sends email from MUA.
2. MUA hands over the message to the configured MTA.
3. MTA checks recipient domain and sends the email over SMTP.
4. Recipient’s MTA receives the email.
5. Recipient reads email using their MUA via IMAP or POP3.

---

## 10. What is Apache Web Server? Explain
Apache is a widely used open-source web server software that allows websites and web applications to be hosted and served to users.

**Features and Benefits:**
- **Modularity**: Support for modules like SSL, URL rewriting, compression.
- **Cross-Platform**: Runs on Linux, Windows, macOS.
- **Virtual Hosting**: Host multiple domains on the same server.
- **Security Features**: Supports SSL/TLS, access control.
- **Logging & Monitoring**: Detailed logs help in debugging and analytics.
- **Customization**: Configurable through `.conf` files.

---

## 11. Opening Mail Server for External Mail
To send/receive external emails, your mail server must be publicly accessible and correctly configured.

**Steps:**
1. **Configure MTA (e.g., Postfix)** with `myhostname`, `myorigin`, `relayhost`.
2. **Enable SMTP Authentication** to prevent abuse.
3. **Open Required Ports**:
   - Port 25: SMTP
   - Port 587: Submission
   - Port 993: Secure IMAP
4. **Configure DNS Records**:
   - MX: Mail Exchange Record
   - SPF: Prevents spoofing
   - DKIM: Verifies sender
   - DMARC: Email validation policy
5. **Secure Your Server**:
   - Use SSL/TLS
   - Enable firewalls with proper port rules
6. **Testing**:
   - Use telnet or online SMTP test tools

---

## 12. (Repeated) Core Working of httpd Server
*Refer to Answer 2 for detailed explanation.*

---

## 13. (Repeated) Steps to Configure DHCP Server
*Refer to Answer 7 for detailed explanation.*

---

## 14. (Repeated) Define DNS Lookup Process
*Refer to Answer 1 for detailed explanation.*

---

## 15. (Repeated) Apache Configuration Steps
*Refer to Answer 8 for detailed explanation.*

---




