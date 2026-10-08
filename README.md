# LetsDefend-WebAttack-Lab-Command-Injection-Attacks

## How to Detect and prevent different types of Web Attacks: SQL Injection,  Cross Site Scripting,  Command Injection,  IDOR,  RFI & LFI and File Upload (Web Shell)

## Practice with SOC Alerts
---
### 🔗117 - SOC167 - LS Command Detected in Requested URL
---

<img width="1914" height="596" alt="image" src="https://github.com/user-attachments/assets/4164fa47-9e0a-47c3-a18c-998bfe4fed51" />

---

* Click on the Details to have more details of the alert 
<img width="1916" height="796" alt="image" src="https://github.com/user-attachments/assets/07a68374-5171-4e32-a1c5-4fa703537dff" />

---

* Click **Continue Button** to Create ticket for EventID: 117

<img width="1918" height="736" alt="image" src="https://github.com/user-attachments/assets/fb128219-e800-4096-9b84-f1641436375b" />

---

* When next window showing ```The ticket has been created successfully.``` pops up,
* Click OK 
<img width="778" height="538" alt="image" src="https://github.com/user-attachments/assets/3e67b2a4-5d15-4b23-bdfc-e52172a1a70b" />

---

* Click the blue **Start Playbook!** button.
<img width="1919" height="567" alt="image" src="https://github.com/user-attachments/assets/b6115677-d13d-4a67-9b6c-fddc968054d1" />

---

<img width="1915" height="741" alt="image" src="https://github.com/user-attachments/assets/a63b390c-93fd-44d8-9773-c1aaacd3374e" />

---

<img width="996" height="591" alt="image" src="https://github.com/user-attachments/assets/c55e87f2-ddc6-49d3-830c-f74a345aa823" />

...

<img width="1894" height="970" alt="image" src="https://github.com/user-attachments/assets/4ed806ec-4929-49a9-a3ff-535b0a424569" />

• Source IP (172.16.17.46): This is marked as a private internal IP address belonging to the host EliotPRD. It returns a 0/92 clean score as expected for a local corporate asset.

---

<img width="1884" height="840" alt="image" src="https://github.com/user-attachments/assets/c8ce5a41-4d12-42eb-8827-6586725bdd9e" />


• Destination IP (188.114.96.15): This is an external IP owned by Cloudflare, Inc., which hosts the official LetsDefend blog. It shows a 0/92 clean score from security vendors, with only one single vendor (URLQuery) flagging it as "Suspicious" (likely a generic tag for shared hosting/CDN addresses or testing artifacts).
These reputation checks further support that the traffic is safe internal user activity rather than a malicious outbound connection.

---

<img width="1902" height="853" alt="image" src="https://github.com/user-attachments/assets/bbad29aa-8610-4dbf-82ef-9c42e658dadf" />
<img width="1853" height="856" alt="image" src="https://github.com/user-attachments/assets/ae792507-00b2-469e-8042-051f649df6cd" />

• Hostname: EliotPRD

• Domain: letsdefend.local

• IP Address: 172.16.17.46

• Operating System: Ubuntu 16.04.4

• Primary User: eliot

• Last Login: 2022-02-26 21:00:01

---

<img width="984" height="621" alt="image" src="https://github.com/user-attachments/assets/dc96c25e-0ef5-4378-9a9e-9abbb298250e" />

---

* Click **Log Management**
* Click **Basic**
<img width="1907" height="891" alt="image" src="https://github.com/user-attachments/assets/1e15ba6a-0f58-4b3c-9751-5ee5e652d1d3" />

---

* Type in one of the IOC, eg. **Source IP Address** ```172.16.17.46``` to Filter the logs
<img width="1918" height="886" alt="image" src="https://github.com/user-attachments/assets/d4ed6571-27c4-4c5a-b0f1-f434573ee08d" />

---

* Click <img width="43" height="36" alt="image" src="https://github.com/user-attachments/assets/db15e860-8964-4e46-a959-d36d08b506b2" /> to view the **Raw Log** in details


<img width="1902" height="877" alt="image" src="https://github.com/user-attachments/assets/446ef341-ffd1-46e5-bec1-b7bd656803ae" />

• Request URL: https://letsdefend.io/blog/?s=skills

• HTTP Response Status: 200 (The request was successful and standard content was returned).

• Analysis: The payload parameter ?s=skills is entirely benign. 

The alert logic generated a false alarm because the letters "ls" sit consecutively inside the word "skills". There are no OS command characters, matching operators, or malicious encodings present.

---

<img width="995" height="421" alt="image" src="https://github.com/user-attachments/assets/e199061e-e5a5-4d12-8965-9962b81b6799" />

### **NON-MALICIOUS**
• Substring Matching Error: The security alert logic triggered a false alarm strictly because the two characters "ls" appear consecutively inside the legitimate English word "skills" (?s=skills).

• No Injection Payload: There are no actual Linux command operators, special characters (like ;, |, &&), or encoding patterns that would indicate an attempt to escape the application and execute commands on the server.

• Legitimate Destination & Activity: The destination IP (188.114.96.15) belongs to Cloudflare CDN hosting the official, trusted LetsDefend training blog. The corporate user (eliot) was simply performing a routine search query on the platform's own blog website.

• Clean Reputation Checks: Both the source host and destination web server returned a completely clean 0/92 reputation score on VirusTotal, proving no malicious infrastructure or active command-and-control (C2) communication was involved.

---

<img width="982" height="456" alt="image" src="https://github.com/user-attachments/assets/47b9ddfc-6bdc-473c-92df-8fdf9c961e1c" />

### * Click **Yes**

Reasons:
My previous screenshot, there was another alert right next to this one under Event ID 118 involving the exact same host (EliotPRD / 172.16.17.46). That separate alert explicitly flagged a ```whoami``` command in a requested URL, which means there is indeed additional, different traffic originating from this source address that needs to be scrutinized.

<img width="1898" height="892" alt="image" src="https://github.com/user-attachments/assets/03b6549c-d527-4249-822f-9ffba1cb5934" />

### Let's verify, why **"YES"**
Check Log Management
1. Navigate back to the Log Management tab on the left sidebar menu.
2. In the search box at the top, enter the host IP address: 172.16.17.46.
3. Look through the list of log entries around the same timestamp (2022-02-26/27).
4. You will see traffic rows showing requests to destination IP addresses. Look for any log entries containing a different signature, such as a request body or URL containing the string whoami (which relates directly to the other alert, Event ID 118).

---

<img width="1006" height="423" alt="image" src="https://github.com/user-attachments/assets/84761066-d6f1-4b5a-bd68-3cb915ebe245" />

<img width="1898" height="892" alt="image" src="https://github.com/user-attachments/assets/03b6549c-d527-4249-822f-9ffba1cb5934" />

This log shows a Malicious event. Here is the technical breakdown of what is happening:

• Targeted Request: The host is sending an HTTP POST request to an internal IP address (172.16.17.16/video/).

• The Payload: The parameter ?c=whoami is an explicit attempt to execute the Linux whoami command on that web server. Unlike our previous "skills" false alarm, this is a literal command intended to see which user account the web server is running under.

• Successful Execution: The HTTP Response Status is 200, meaning the command request was accepted, processed, and responded to successfully by the server.

• Suspicious User-Agent: The request uses an ancient Internet Explorer 6 user-agent (MSIE 6.0; Windows NT 5.1), which is standard behavior for automated hacking tools or malicious scripts trying to disguise traffic.

---
