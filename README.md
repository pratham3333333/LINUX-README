# Unit 2 - Linux System Administration (RHEL 6)

---

## 1. What is Network Manager? Explain in detail
**Theory:**
Network Manager is a system service in Linux that manages and automates network connections. It provides a consistent interface for both wired and wireless networks and supports features such as dynamic configuration, VPN integration, and mobile broadband.

**Features:**
- Automatic detection of network devices
- Easy management of connections via GUI, CLI (`nmcli`), and TUI (`nmtui`)
- Supports various networking technologies like Ethernet, Wi-Fi, PPPoE, etc.
- Handles DNS and routing changes dynamically

**Example:**
```bash
nmcli device
nmcli connection add type ethernet ifname eth0 con-name office static ip4 192.168.1.100/24 gw4 192.168.1.1
```
This sets a static IP address using `nmcli`.

---

## 2. How would you elaborate the nice command? Explain process niceness
**Theory:**
The `nice` command in Linux controls the priority of a process. Niceness is a value that affects a process's scheduling priority. A lower nice value means higher priority, and vice versa. Only root can use negative (higher priority) values.

**Nice Value Range:**
- From -20 (highest priority) to +19 (lowest priority)

**Example - Start a new process with lower priority:**
```bash
nice -n 10 gzip bigfile.tar
```

**Example - Change priority of running process:**
```bash
renice -n -5 -p 1234  # PID 1234 priority increased
```

---

## 3. What is nsswitch? Explain in detail
**Theory:**
`nsswitch.conf` is a configuration file that defines how a Linux system should resolve different types of databases like user accounts, hostnames, and passwords. It tells the system where to look for information—files, DNS, LDAP, etc.

**File Location:**
`/etc/nsswitch.conf`

**Example Entry:**
```
hosts: files dns
passwd: files ldap
```
This means: first check local files (like `/etc/hosts`), then query DNS or LDAP as necessary.

---

## 4. How to change ownership of the users with what key sequences?
**Theory:**
File and directory ownership can be changed using the `chown` (change owner) command. Ownership includes the user and optionally the group.

**Syntax:**
```bash
chown USER[:GROUP] filename
```

**Example:**
```bash
chown alice:staff report.txt
```
This changes the owner to `alice` and the group to `staff`.

---

## 5. What is Job Scheduling in RHEL 6? Explain Crontab
**Theory:**
Job scheduling allows repetitive tasks to be run automatically using `cron`. The `crontab` file defines what commands are executed and when.

**Command to edit crontab:**
```bash
crontab -e
```

**Crontab Time Format:**
```
* * * * * command_to_execute
│ │ │ │ │
│ │ │ │ └── Day of the week (0 - 7) (Sunday=0 or 7)
│ │ │ └──── Month (1 - 12)
│ │ └────── Day of month (1 - 31)
│ └──────── Hour (0 - 23)
└────────── Minute (0 - 59)
```

**Example - Backup every day at 2 AM:**
```bash
0 2 * * * /usr/bin/backup.sh
```

---

## 6. Explain setting the default permission with umask in RHEL 6
**Theory:**
The `umask` value determines the default permissions for newly created files and directories. It works by subtracting the `umask` value from the base permissions (666 for files, 777 for directories).

**Command to check umask:**
```bash
umask
```

**Example:**
If `umask` is 022:
- New file permission = 666 - 022 = 644
- New directory permission = 777 - 022 = 755

**Set temporary umask:**
```bash
umask 027
```

---

## 7. What are the key sequence for user administration in RHEL
**Key Commands for User Administration:**
- Create User: `useradd john`
- Set Password: `passwd john`
- Modify User: `usermod -s /bin/bash john`
- Lock Account: `passwd -l john`
- Delete User: `userdel -r john`
- View User Info: `id john`

**Example:**
```bash
useradd alice
passwd alice
usermod -aG wheel alice
```
Adds `alice` and makes her a sudoer.

---

## 8. List and explain most useful attributes for security of files in RHEL 6
**Theory:**
Linux provides various commands and attributes to secure files:
- `chmod`: Set file permissions
- `chown`: Change file owner
- `umask`: Set default creation permissions
- `lsattr`: List file attributes
- `chattr`: Modify file attributes

**Important Attributes with `chattr`:**
- `+i`: Immutable, cannot be changed or deleted
- `+a`: Append only

**Example:**
```bash
chattr +i confidential.txt
lsattr confidential.txt
```

---

## 9. List and explain the parameters for user administration commands
**Common Parameters:**
- `useradd -m username`: Create user with home dir
- `useradd -s /bin/bash`: Specify default shell
- `usermod -L username`: Lock user account
- `usermod -aG group username`: Add user to group
- `passwd -e username`: Force password change on next login

**Example:**
```bash
useradd -m -s /bin/bash -G developers raj
```
Creates a user `raj` with `/bin/bash` shell and adds to `developers` group.

---

## 10. How would you elaborate creating groups in RHEL 6
**Theory:**
Groups help manage file permissions collectively. Users can belong to multiple groups. The `groupadd` and `usermod` commands are used to manage groups.

**Commands:**
```bash
groupadd design
usermod -aG design bob
```
**Example:**
To check groups for user `bob`:
```bash
groups bob
```

---

## 11. How would you discuss managing permission in detail
**Theory:**
Linux uses three types of permissions for each file/directory:
- **Read (r):** View content
- **Write (w):** Modify content
- **Execute (x):** Run file (or enter directory)

**Permission Format:**
```bash
-rwxr-xr--
```
User: rwx, Group: r-x, Others: r--

**Commands:**
```bash
chmod 755 script.sh
chown alice:devteam script.sh
```

---

## 12. Explain rwx permissions for files and directories
**Theory:**
Permissions behave differently for files and directories:

**Files:**
- `r`: Read file content
- `w`: Modify file
- `x`: Execute the file (if script or binary)

**Directories:**
- `r`: List directory contents
- `w`: Create/delete files inside
- `x`: Enter the directory (cd)

**Example:**
```bash
chmod 751 /home/alice
```
This allows owner full access, group read & execute, others only execute.

---

## 13. How would you elaborate the nice command? Explain process niceness
(Refer to Answer 2 - Duplicate question. Detailed explanation of `nice` and `renice` with examples already provided.)

---

## 14. List and Elaborate commands for User Management
**Theory:**
Linux offers various commands for user account handling. Here are some of the most commonly used ones:

**Commands and Examples:**
- `useradd john`: Adds a user
- `passwd john`: Sets password
- `userdel -r john`: Deletes user and home
- `usermod -aG wheel john`: Adds to sudo group
- `id john`: Shows user ID and groups

**Example Use Case:**
```bash
useradd -m david
passwd david
usermod -aG sudo david
```
This creates user `david`, sets a password, and gives him sudo privileges.

---



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



