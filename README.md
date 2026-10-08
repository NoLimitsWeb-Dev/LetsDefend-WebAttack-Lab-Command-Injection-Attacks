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

### * Click **No**

<img width="985" height="542" alt="image" src="https://github.com/user-attachments/assets/075bbd02-aae0-43dd-b99e-9f4ee736ed2f" />

The playbook asks if there is different traffic coming from the source device (172.16.17.46). Looking closely at the log results:

• Same Destination: Every single log entry shows communication going to the exact same destination IP address (188.114.96.15).

• Same Protocol/Type: Every entry is categorized under the same log Type (Proxy) over port 443 (HTTPS).

• Same Activity: This entire list represents the exact same browsing session to the LetsDefend blog server where the benign search occurred.

<img width="1913" height="845" alt="image" src="https://github.com/user-attachments/assets/5448af78-b0c4-40d4-a565-62ee95d6f11b" />

Because there are no connections to unexpected external servers, no weird scanning traffic, and no malicious command-and-control (C2) behavior originating from the host, there is no different/unrelated traffic to worry about.

---

<img width="993" height="559" alt="image" src="https://github.com/user-attachments/assets/8c40b4b0-8634-4c76-8151-360af83222e3" />

```
The alert for EventID 117 (SOC167 - LS Command Detected in Requested URL) is a False Positive. The alert was triggered due to a signature match on the string "ls" within the requested URL parameter (https://letsdefend.io/blog/?s=skills). 

The string is part of the benign search query "skills" and does not contain any malicious command injection or directory traversal payloads. The traffic represents normal outbound web browsing from internal host EliotPRD (172.16.17.46) to the destination IP (188.114.96.15). No further remediation action is required.
```

---

<img width="973" height="431" alt="image" src="https://github.com/user-attachments/assets/275870d2-d6c4-46ed-8f6d-22ba26e2bb48" />

Let's Click **Confirm & Close** to submit the playbook and finalize investigation.
