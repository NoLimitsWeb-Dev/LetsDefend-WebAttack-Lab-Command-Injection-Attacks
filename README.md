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

<img width="995" height="315" alt="image" src="https://github.com/user-attachments/assets/b4bbe116-a304-48fa-b8ab-a5306024d271" />

* Click Command Injection.

Why is the correct choice:

The payload parameters LS and ?c=whoami directly targets the operating system by attempting to execute the built-in system commands ls and whoami. When an attacker inputs system commands into a web application to force the server to execute them, it is a textbook case of Command Injection.

---

<img width="999" height="528" alt="image" src="https://github.com/user-attachments/assets/ff00e62d-7bac-4ddf-a2ee-5f45f3c3e371" />

Click **Not Planned.**

Why this is the correct choice:

• Host Character: The hostname we verified earlier is EliotPRD (an abbreviation for a Production environment machine), which does not match simulation product names like Verodin, AttackIQ, or Picus.

<img width="1909" height="767" alt="image" src="https://github.com/user-attachments/assets/9fc85ec6-99c5-4a6b-a7c1-71edf8c4cb54" />

• User Context: The activity was associated with user eliot on an internal server machine, and there are no indicators in the LetsDefend mailbox or logs suggesting an official internal penetration test was actively scheduled for this asset during this timeframe.

---

<img width="1002" height="382" alt="image" src="https://github.com/user-attachments/assets/fbaed0d8-0d8a-4ec7-b7c9-fc96e17969ca" />

* Click Company Network → Company Network.

### Why this is the correct:

The malicious traffic I verified in the log management tab shows the source host 172.16.17.46 (EliotPRD) sending a request to 172.16.17.16. Since both of these are private, internal IP addresses belonging to the corporate environment, the traffic is entirely internal.

• Source Address Field: The log explicitly labels the Source IP as 172.16.17.46, which belongs directly to the host machine EliotPRD.

• Destination Address Field: The log labels the Destination IP as 172.16.17.16 (the internal web server hosting the /video/ directory path).

• Directionality (HTTP POST): An HTTP POST request inherently moves from a client (source) to a server (destination). In this case, EliotPRD initiated the connection outbound from itself and targeted the web server to upload/submit the malicious payload (?c=whoami).


### The IP 188.114.96.15 belongs to the original alert (Event ID 117) that I started with. 

Here is the difference:

• 188.114.96.15 (The False Alarm): This traffic went from your host (172.16.17.46) to the Internet. It was the benign search query for the word "skills" on the LetsDefend blog.

• 172.16.17.16 (The Real Attack): This is the different traffic you uncovered during the playbook steps. It went from your host to another internal machine (Company Network → Company Network) containing the malicious whoami payload.

---

<img width="922" height="606" alt="image" src="https://github.com/user-attachments/assets/9ff4b5d0-f5c3-45d1-9ef4-fcc6a6a4bcd3" />

1. Click the Endpoint Security link visible on your playbook screen (or navigate to it via the left sidebar menu).
2. Find the compromised host asset named EliotPRD (172.16.17.46).
3. Locate the Change Status column on the right side and toggle the switch to Isolate/Contain the host machine.
4. Once I successfully completed the isolation toggle on that page and return to this playbook pop-up and click the blue Next button.

<img width="1864" height="867" alt="image" src="https://github.com/user-attachments/assets/fc7bb664-4f66-4a08-8fd0-9320460c024e" />

---

<img width="992" height="509" alt="image" src="https://github.com/user-attachments/assets/2c0f4f29-d7ef-4223-983a-a34ba69af1cd" />

### Artifact
1. Input the following configuration details:
• Value: ?c=whoami

• Comment: Malicious OS command injection payload

• Type: Select URL

2. Click the + icon to create a row, and input the following configuration details:
• Value: 172.16.17.16

• Comment: Target internal web server attacked via command injection

• Type: Select IP Address from the dropdown menu

---

## Click Yes.
<img width="986" height="621" alt="image" src="https://github.com/user-attachments/assets/b595b7dd-96e2-45e3-a4dd-37e63cfee404" />

### Reasons to escalate to Tier 2:

The criteria listed right on the playbook screen clearly state that escalation is mandatory if:

• The attack succeeds: The log showed a 200 OK HTTP response to a web request carrying an active command injection payload (?c=whoami), which means the target server successfully processed the request and ran the command.

<img width="1898" height="892" alt="image" src="https://github.com/user-attachments/assets/bc6cf947-471e-4b42-9bea-5e4fea7262f8" />

• The traffic is Inside \(\rightarrow \) Inside: As we verified, the traffic originated from 172.16.17.46 (internal host) and hit 172.16.17.16 (internal server). This confirms lateral movement inside the local corporate environment.

* Clicking Yes will forward the case to Tier 2 senior analysts for deep forensics and incident response.

---

<img width="983" height="500" alt="image" src="https://github.com/user-attachments/assets/7dd920b8-535e-4bb9-97b5-6fd10ca62978" />

```
Initial alert SOC167 for EventID 117 was triggered by a false positive substring match on the word "skills" during an external blog lookup. However, further cross-examination of log management revealed separate, highly suspicious traffic originating from the same source host (172.16.17.46 / EliotPRD).

The host machine conducted an internal lateral attack targeting web server 172.16.17.16 via an HTTP POST request carrying an OS command injection payload (?c=whoami). The server responded with an HTTP 200 OK status, confirming successful execution. An ancient User-Agent string was utilized during the attack, highlighting automated script activity. 

Due to successful inside-to-inside command injection compromise, the source host EliotPRD has been network-isolated via Endpoint Security, the target server IP was added as an artifact, and the case is being officially escalated to Tier 2 for full incident response and deep forensics.
```
