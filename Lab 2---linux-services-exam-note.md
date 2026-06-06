# Linux Services — Complete Exam-Ready Lab Note

> তোমার নিজের note।
> ব্যাখ্যা ছোট ছোট লাইনে ভাঙা — এক লাইনে এক point।
> Bangla, technical term English-এ।
> Lab-এর প্রতিটা প্রশ্নের উত্তর ভেতরে আছে।

---

## 📑 Topic List (priority map)

🔴 High · 🟡 Medium · 🟢 Low

1. 🟡 Lab Setup — VM ও network
2. 🔴 Nginx Web Server — install ও deploy
3. 🔴 `127.0.0.1` / localhost — কী special
4. 🟡 Web page edit — `nano`/`gedit`, permission error, index file
5. 🔴 Nmap — port scanning
6. 🔴 UFW — Ubuntu firewall
7. 🔴 SSH — remote command-line access
8. 🔴 `/etc/passwd` ও নতুন user (`adduser`)
9. 🟡 SSH revisited — username ও `exit`
10. 🔴 Compressed archives — `wget`, `tar`, `bzip2`
11. 🟡 `scp` — secure copy (network-এ)
12. 🟢 gedit over SSH — X11 forwarding (Challenge)

---

## 1. Lab Setup — VM ও network

🎯 **Exam গুরুত্ব: Medium** — কেন একই IP/MAC, NAT network — concept আসে। ~2–3 marks।

**এক কথায়:**
দুই machine একে অপরের সাথে কথা বলবে।
তাই দুটো VM একই network-এ বসাতে হয়।

### ধারণা (Concept)
- South St campus → VMware, Ubuntu-22.04, password `student`।
- External/online → নিজের laptop-এ **দুটো VM** লাগবে।
- দ্বিতীয়টা = প্রথমটার **clone** (VirtualBox-এ)।

### গভীর ব্যাখ্যা (ছোট ছোট ধাপে)
দুই VM-কে একই network-এ আনতে:
- File → Preferences → Network → NAT network যোগ করো।
- প্রতি VM-এ Settings → Network → "NAT Network" select করো।
- দুটো বন্ধ করে আবার চালু করো।

**Clone করলে সমস্যা:**
- দুই VM-এর **একই IP** দেখাবে।
- কারণ দুটোর **একই MAC address**।
- কারণ clone মানে হুবহু একই — সব দিক থেকে।

**সমাধান:**
- MAC address **re-initialize** করতে হবে।
- তাহলে দুই VM আলাদা IP পাবে।
- এরপর একে অপরকে `ping` করা যাবে।

ক্যাম্পাসে `ip a`-তে `134.115...` দিয়ে শুরু IP না দেখলে → tutor-এর সাহায্য নাও।

### আউটকাম
দুই machine network-এ যুক্ত, একে অপরকে ping করতে পারে।

### টিপস
একই IP/MAC = clone সমস্যা → MAC re-init।
দুই VM = একই NAT network-এ বসাও।

**Exam-এ যেভাবে আসতে পারে:**
"দুই cloned VM-এর একই IP কেন?" → একই MAC address, কারণ clone হুবহু এক; সমাধান MAC re-initialize।

---

## 2. Nginx Web Server — install ও deploy

🎯 **Exam গুরুত্ব: High** 🔴 — service কী, install command, web serving — core। ~4–6 marks।

**এক কথায়:**
Nginx একটা **web server** software।
এটা চালালে তোমার machine একটা website serve করতে পারে।

### ধারণা (Concept)
- **Server** = এমন program যা অন্যদের কিছু "service" দেয়।
- **Web server** = browser-কে web page পাঠায়।
- **Nginx** = জনপ্রিয়, হালকা web server।

### গভীর ব্যাখ্যা (ছোট ধাপে)
Install-এর ধাপ:
- `sudo apt update` → আগে package তালিকা refresh।
- `sudo apt install nginx-full` → Nginx install।
- `sudo apt install nmap` → port scanner (পরে লাগবে)।
- `sudo apt install openssh-server` → SSH server (পরে লাগবে)।

Install হলেই Nginx **চালু** হয়ে যায়।
এখন তোমার machine একটা web page serve করছে।

দেখতে:
- Browser খোলো।
- যাও `http://127.0.0.1`
- Default Nginx welcome page দেখবে।

**Network-এ share:**
- `ip a` দিয়ে নিজের IP বের করো।
- partner-কে সেই IP দাও।
- সে browser-এ তোমার IP লিখলে **তোমার page** দেখবে।
- দুজনের page প্রায় একই (default page)।

### উদাহরণ
```
sudo apt update
sudo apt install nginx-full
# browser → http://127.0.0.1  → Nginx welcome page
```

### কখন ও কেন
যেকোনো website-এর পেছনে একটা web server থাকে।
Cloud/hosting/security — সব জায়গায় Nginx খুব common।

### টিপস
- Web server = page serve করে।
- install = `sudo apt install nginx-full`।
- install হলেই চালু।

**Exam-এ যেভাবে আসতে পারে:**
"Nginx কী?" → একটা web server, browser-কে web page দেয়।
"install command?" → `sudo apt install nginx-full`।

---

## 3. `127.0.0.1` / localhost — কী special

🎯 **Exam গুরুত্ব: High** 🔴 — lab সরাসরি জিজ্ঞেস করেছে; loopback concept। ~3–5 marks।

**এক কথায়:**
`127.0.0.1` = **নিজের machine-এর ঠিকানা**।
এটা দিয়ে machine নিজের সাথে কথা বলে।

### ধারণা (Concept)
- নাম: **loopback** address।
- নাম-রূপ: **localhost**।
- মানে: "এই computer-টাই"।

### গভীর ব্যাখ্যা (ছোট ধাপে)
কেন special:
- `127.0.0.1` কখনো network-এ যায় না।
- request নিজের machine-এই ঘুরে আসে (loop back)।
- **প্রতিটা** computer-এ এটা থাকে।
- internet না থাকলেও কাজ করে।

তাই:
- `http://127.0.0.1` = নিজের Nginx page।
- partner সেটা দেখতে পায় না (এটা তার কাছে তার নিজের machine বোঝায়)।
- partner-কে দেখাতে হলে তোমার **আসল** IP দাও (`ip a` থেকে)।

**Public vs Private (lab-এর প্রশ্ন):**
- `127.0.0.1` → loopback, নিজের।
- `134.115.x.x` (campus) → **public** IP।
- `192.168.x.x` / `10.x.x.x` → **private** IP (লোকাল network)।

### উদাহরণ
```
ip a
# 127.0.0.1   → loopback (নিজের)
# 192.168.1.x → private (লোকাল)  বা  134.115.x.x → public (campus)
```

### টিপস
`127.0.0.1` = localhost = loopback = নিজের machine।
সব machine-এ থাকে, network ছাড়াই চলে।

**Exam-এ যেভাবে আসতে পারে:**
"`127.0.0.1`-এ special কী?" → এটা loopback/localhost, machine নিজের সাথে কথা বলে; প্রতি machine-এ থাকে।

---

## 4. Web page edit — `nano`/`gedit`, permission, index file

🎯 **Exam গুরুত্ব: Medium** — permission error কীভাবে fix, index priority। ~3–4 marks।

**এক কথায়:**
Nginx-এর page-গুলো থাকে `/var/www/html/`-এ।
এগুলো edit করতে **root permission (`sudo`)** লাগে।

### ধারণা (Concept)
- Default page: `/var/www/html/index.nginx-debian.html`
- নিজের নতুন page: `/var/www/html/index.html`
- edit: `nano` (terminal) বা `gedit` (GUI)।

### গভীর ব্যাখ্যা (ছোট ধাপে)

**Permission error কেন? (lab-এর প্রশ্ন)**
- `/var/www/html/` folder-টা **root**-এর মালিকানায়।
- সাধারণ user হিসেবে save করতে গেলে "Permission denied"।

**কীভাবে fix:**
- command-এর আগে `sudo` দাও।
- যেমন: `sudo nano /var/www/html/index.nginx-debian.html`
- এখন root হিসেবে edit/save হবে।

**nano vs gedit (আবার আসছে — মনে রাখো):**
- nano → terminal-এর ভেতরে, GUI লাগে না।
- gedit → graphical, display লাগে।
- server/remote-এ → nano চলে, gedit চলে না।

**index file কোনটা আগে দেখায়? (lab-এর প্রশ্ন)**
- Nginx আগে খোঁজে `index.html`।
- পেলে সেটাই দেখায়।
- না পেলে `index.nginx-debian.html` দেখায়।
- তাই নতুন `index.html` বানালে → তোমার page-ই default হয়ে যায়।
- পুরোনো `index.nginx-debian.html` রয়ে যায়, শুধু আর দেখায় না।

### উদাহরণ
```
sudo nano /var/www/html/index.nginx-debian.html   # default page edit
sudo nano /var/www/html/index.html                # নতুন page (এটাই আগে দেখাবে)
```

### টিপস
- `/var/www/html/` = root-owned → edit-এ `sudo` লাগে।
- Nginx priority: `index.html` আগে, পরে `index.nginx-debian.html`।

**Exam-এ যেভাবে আসতে পারে:**
"web page edit-এ permission error — fix?" → `sudo` দিয়ে edit করো (folder root-owned)।
"index.html আর index.nginx-debian.html — কোনটা দেখাবে?" → index.html আগে।

---

## 5. Nmap — port scanning

🎯 **Exam গুরুত্ব: High** 🔴 — port scan, service চেনা — security core। ~4–6 marks।

**এক কথায়:**
Nmap দেখায় একটা machine-এর কোন কোন **port খোলা**।
খোলা port = সেখানে কোনো service চলছে।

### ধারণা (Concept)
- **Port** = একটা service-এর দরজা/নম্বর।
- যেমন: 80 = web (HTTP), 22 = SSH, 443 = HTTPS।
- **Nmap** = সেই দরজাগুলো scan করে দেখে কোনটা খোলা।

### গভীর ব্যাখ্যা (ছোট ধাপে)

চালানো:
- `nmap <partner-এর-IP>`
- ফলাফলে খোলা port আর সেই service-এর নাম দেখায়।

service চেনা (lab-এর প্রশ্ন):
- port 80 খোলা → web server (Nginx) চলছে।
- port 22 খোলা → SSH চলছে।

**Nginx remove করলে কী বদলায়? (lab-এর প্রশ্ন)**
- `sudo apt remove nginx-full`
- `sudo apt autoremove` (অপ্রয়োজনীয় dependency-ও মোছে)
- আবার `nmap` চালাও।
- **port 80 আর খোলা থাকবে না।**
- কারণ web service বন্ধ → দরজা বন্ধ।

আবার ফেরাতে:
- `sudo apt install nginx-full` → port 80 ফিরে আসে।

মূল শিক্ষা:
- খোলা port = চলমান service।
- service বন্ধ করলে port বন্ধ।

### উদাহরণ
```
nmap 192.168.1.50        # partner-এর port scan
# 80/tcp open http       → Nginx চলছে
# 22/tcp open ssh        → SSH চলছে
sudo apt remove nginx-full   # এরপর 80 আর open থাকবে না
```

### কখন ও কেন
কোন machine-এ কী service খোলা জানা = security audit-এর প্রথম ধাপ।
Attacker-ও এটা করে, defender-ও — তাই জানা জরুরি।

### টিপস
Nmap = খোলা port দেখায়।
80=web, 22=ssh, 443=https।
service বন্ধ → port বন্ধ।

**Exam-এ যেভাবে আসতে পারে:**
"Nmap কী করে?" → machine-এর খোলা port scan করে।
"Nginx remove করলে nmap-এ কী বদলায়?" → port 80 আর খোলা থাকে না, কারণ web service বন্ধ।

---

## 6. UFW — Ubuntu firewall

🎯 **Exam গুরুত্ব: High** 🔴 — firewall enable, allow port, কেন sudo — common। ~5–7 marks।

**এক কথায়:**
UFW = **U**ncomplicated **F**ire**w**all।
এটা ঠিক করে কোন port বাইরে থেকে ঢুকতে দেবে, কোনটা নয়।

### ধারণা (Concept)
- Firewall = network-এর দারোয়ান।
- কোন traffic ঢুকবে/বের হবে — নিয়ম ঠিক করে।
- UFW = Ubuntu-র সহজ firewall tool।

### গভীর ব্যাখ্যা (ছোট ধাপে)

**কেন `sudo` লাগে? (lab-এর প্রশ্ন)**
- Firewall একটা **system-level** নিরাপত্তা setting।
- সাধারণ user এটা বদলাতে পারে না।
- তাই root-power দরকার → `sudo`।

ধাপগুলো:
- `sudo ufw status verbose` → firewall চালু কিনা, কী নিয়ম আছে দেখায়।
- শুরুতে সাধারণত **inactive** (বন্ধ)।

firewall চালু:
- `sudo ufw enable`
- এখন বাইরের সব port **block** (default deny)।

এখন partner `nmap` করলে কী দেখে?
- আগে port 80, 22 খোলা দেখত।
- enable-এর পর → port গুলো **আর দেখায় না / filtered**।
- কারণ firewall সব আটকে দিয়েছে।

একটা দরজা খোলা:
- `sudo ufw allow 80/tcp`
- এখন শুধু port 80 (web) খোলা।
- partner আবার তোমার **webpage দেখতে পারবে**।
- কিন্তু বাকি port (যেমন 22) এখনো block।

মূল ধারণা:
- firewall default-এ সব বন্ধ রাখে (নিরাপদ)।
- তুমি দরকারমতো একটা একটা port খোলো।
- এটাই **least privilege**-এর network রূপ।

### উদাহরণ
```
sudo ufw status verbose     # অবস্থা দেখা
sudo ufw enable             # firewall চালু (সব block)
sudo ufw allow 80/tcp       # শুধু web port খোলা
```

### কখন ও কেন
Server-কে আক্রমণ থেকে বাঁচাতে শুধু দরকারি port খোলা রাখা।
বাকি সব বন্ধ = attack surface ছোট।

### টিপস
UFW = firewall।
`enable` = সব বন্ধ; `allow 80/tcp` = web খোলা।
সব command-এ `sudo` (system-level)।

**Exam-এ যেভাবে আসতে পারে:**
"UFW-তে কেন sudo লাগে?" → firewall system-level setting, root-power দরকার।
"`ufw enable`-এর পর nmap-এ কী বদলায়?" → port গুলো block/filtered হয়ে যায়।
"web access ফেরাতে?" → `sudo ufw allow 80/tcp`।

---

## 7. SSH — remote command-line access

🎯 **Exam গুরুত্ব: High** 🔴 — remote access, port 22, firewall সংযোগ। ~5–7 marks।

**এক কথায়:**
SSH দিয়ে তুমি **অন্য একটা machine-এর terminal** নিজের কাছে নিয়ে কাজ করতে পারো।
নিরাপদভাবে, network-এর ওপর দিয়ে।

### ধারণা (Concept)
- SSH = **S**ecure **Sh**ell।
- দূরের machine-এ command-line access দেয়।
- username + password লাগে।
- Port **22** ব্যবহার করে।

### গভীর ব্যাখ্যা (ছোট ধাপে)

login করতে:
- `ssh <partner-এর-IP>`
- password চাইবে।
- ঢুকলে prompt বদলে partner-এর machine দেখাবে।

**কাজ না করলে — firewall সমস্যা? (lab-এর প্রশ্ন)**
- হ্যাঁ হতে পারে।
- UFW যদি চালু থাকে আর port 22 block থাকে → SSH ঢুকবে না।

selectively port খোলা:
- `sudo ufw allow 22/tcp`  (বা `sudo ufw allow ssh`)
- এখন SSH port খোলা → login সম্ভব।

মূল ধারণা:
- প্রতিটা service-এর নিজের port।
- firewall সেই নির্দিষ্ট port খুলে দিতে হয়।
- web = 80, ssh = 22।

### উদাহরণ
```
ssh 192.168.1.50            # partner-এর machine-এ login চেষ্টা
# আটকালে partner-এর machine-এ:
sudo ufw allow 22/tcp       # ssh port খোলা
```

### কখন ও কেন
Internet-এর প্রায় সব server SSH দিয়ে manage হয় (GUI নেই)।
তোমার cloud/security career-এর রোজকার tool।

### টিপস
SSH = দূরের terminal, port 22, secure।
আটকালে → `sudo ufw allow 22/tcp`।

**Exam-এ যেভাবে আসতে পারে:**
"SSH কী?" → দূরের machine-এ secure command-line access।
"SSH কোন port?" → 22।
"SSH ঢুকছে না কেন হতে পারে?" → firewall port 22 block করেছে; `ufw allow 22/tcp` দিয়ে খোলো।

---

## 8. `/etc/passwd` ও নতুন user (`adduser`)

🎯 **Exam গুরুত্ব: High** 🔴 — passwd file পড়া, adduser-এ কী বদলায়। ~4–6 marks।

**এক কথায়:**
`/etc/passwd` = system-এর **সব user-এর তালিকা**।
নতুন user বানালে এখানে একটা নতুন লাইন যোগ হয়।

### ধারণা (Concept)
- `/etc/passwd` একটা সাধারণ text file।
- প্রতিটা লাইন = একটা user account।
- দেখতে: `less /etc/passwd`।

### গভীর ব্যাখ্যা (ছোট ধাপে)

একটা লাইনের গঠন (`:` দিয়ে ভাগ):
```
azim:x:1000:1000:Azim:/home/azim:/bin/bash
```
- `azim` → username
- `x` → password (আসলটা `/etc/shadow`-এ লুকানো)
- `1000` → UID (user ID)
- `1000` → GID (group ID)
- `Azim` → comment/পুরো নাম
- `/home/azim` → home folder
- `/bin/bash` → default shell

লক্ষ্য করো:
- উপরে অনেক "system user"-ও আছে (root, daemon ইত্যাদি)।
- এগুলো program চালানোর জন্য, মানুষের জন্য নয়।

নতুন user বানানো:
- `sudo adduser bob`
- password set করতে বলবে।
- (root লাগে — তাই `sudo`)।

**কী বদলায়? (lab-এর প্রশ্ন)**
- আবার `less /etc/passwd` দেখো।
- নিচে `bob`-এর একটা **নতুন লাইন** যোগ হয়েছে।
- নতুন UID, নতুন home `/home/bob`।

### উদাহরণ
```
less /etc/passwd            # সব user
sudo adduser bob            # নতুন user + password
less /etc/passwd            # bob-এর নতুন লাইন দেখবে
```

### কখন ও কেন
Server-এ access ব্যবস্থাপনা = user বানানো/মোছা।
কে কী করতে পারবে — সব এই user system-এর ভিত্তিতে।

### টিপস
`/etc/passwd` = user তালিকা (password নয়, সেটা `/etc/shadow`)।
লাইন গঠন: username:x:UID:GID:comment:home:shell।
`sudo adduser` = নতুন লাইন যোগ।

**Exam-এ যেভাবে আসতে পারে:**
"`/etc/passwd`-এ কী থাকে?" → সব user account-এর তালিকা ও তথ্য।
"adduser-এর পর passwd file-এ কী বদলায়?" → নতুন user-এর একটা লাইন যোগ হয়।
"password কি passwd file-এ থাকে?" → না, `x` থাকে; আসল password `/etc/shadow`-এ।

---

## 9. SSH revisited — username ও `exit`

🎯 **Exam গুরুত্ব: Medium** — username দিয়ে login, logout। ~2–4 marks।

**এক কথায়:**
SSH-এ নাম না দিলে **তোমার নিজের** username দিয়ে চেষ্টা করে।
আলাদা user হতে → `username@ip`।

### ধারণা (Concept)
- `ssh <ip>` → তোমার current username ব্যবহার করে।
- `ssh bob@<ip>` → `bob` নামে login।
- বের হতে → `exit`।

### গভীর ব্যাখ্যা (ছোট ধাপে)

default behaviour:
- `ssh 192.168.1.50`
- system ধরে নেয় username = তুমি যে নামে বসে আছ।
- partner-এর machine-এ সেই নাম না থাকলে login fail।

আলাদা user দিয়ে:
- partner-এর কাছে তার নতুন user-এর নাম+password নাও।
- `ssh bob@192.168.1.50`
- bob-এর password দিয়ে login।

logout:
- `exit` → partner-এর machine থেকে বের।
- prompt আবার তোমার machine দেখাবে।
- দুবার চাপলে আরও পেছনে (nested হলে)।

### উদাহরণ
```
ssh bob@192.168.1.50        # bob নামে login
# কাজ শেষে:
exit                        # logout, নিজের machine-এ ফেরা
```

### টিপস
নাম না দিলে = নিজের username।
আলাদা user = `user@ip`।
logout = `exit`।

**Exam-এ যেভাবে আসতে পারে:**
"নির্দিষ্ট user দিয়ে SSH?" → `ssh username@ip`।
"SSH session থেকে বের হতে?" → `exit`।

---

## 10. Compressed archives — `wget`, `tar`, `bzip2`

🎯 **Exam গুরুত্ব: High** 🔴 — tar/bzip2 command, ratio, কেন compress। ~5–8 marks।

**এক কথায়:**
অনেক file একসাথে **এক bundle** (tar) করি।
তারপর সেটা ছোট করি (bzip2 compress)।

### ধারণা (Concept)
- `wget <url>` → web থেকে file download।
- `tar` → অনেক file জোড়া দিয়ে এক archive (bundle)।
- `bzip2` → সেই archive-কে compress (ছোট) করা।

⚠️ গুরুত্বপূর্ণ তফাত:
- **tar = bundling** (একসাথে বাঁধা), নিজে তেমন ছোট করে না।
- **bzip2 = compression** (আসল ছোট করা)।
- দুটো আলাদা কাজ, দুই ধাপ।

### গভীর ব্যাখ্যা (ছোট ধাপে)

download:
- `wget https://www.gutenberg.org/files/76/76-0.txt`
- (তিনটা বই এভাবে নামাও)

গুছানো:
- `mkdir books` → folder বানাও।
- `mv 76-0.txt books/` (তিনটা file books-এ move করো)।

bundle বানানো:
- `tar cf books.tar books`
- `c` = create, `f` = file (archive-এর নাম)।
- এখন `books.tar` = তিন file এক bundle।

compress করা:
- `bzip2 books.tar`
- তৈরি হয় `books.tar.bz2` (আসল `.tar` চলে যায়)।

ফলাফল দেখা:
- `ls -la`
- `books.tar.bz2`-এর size দেখো।
- তিন file-এর মোট size-এর সাথে তুলনা করো।

**compression ratio (lab-এর প্রশ্ন):**
- ratio = compressed size ÷ original size।
- text file খুব ভালো compress হয়।
- তাই `.bz2` মূল size-এর অনেক ছোট হবে।
- (যেমন মূল 2 MB → হয়তো 0.6 MB মানে ~70% সাশ্রয়)।

খুলে ফেলা (decompress):
- `bunzip2 books.tar.bz2` → আবার `books.tar`।
- `tar -xvf books.tar` → file গুলো বের।
- `x` = extract, `v` = verbose (নাম দেখায়), `f` = file।

**কেন compress করি? (lab-এর প্রশ্ন)**
- server-এ অনেক file আলাদা আলাদা সরানো ধীর ও ঝামেলা।
- এক bundle + ছোট size = দ্রুত, সহজ transfer।
- বিশেষ করে network-এর ওপর দিয়ে পাঠাতে।

### উদাহরণ
```
wget https://www.gutenberg.org/files/76/76-0.txt
mkdir books
mv 76-0.txt books/
tar cf books.tar books      # bundle
bzip2 books.tar             # compress → books.tar.bz2
ls -la                      # size তুলনা
# খুলতে:
bunzip2 books.tar.bz2
tar -xvf books.tar
```

### কখন ও কেন
Backup, log archive, file transfer — সর্বত্র।
এক archive পাঠানো অনেক file পাঠানোর চেয়ে সহজ/দ্রুত।

### টিপস
- `wget` = download।
- `tar cf` = bundle (c=create, f=file)।
- `bzip2` = compress; `bunzip2` = খোলা।
- `tar -xvf` = extract (x=extract, v=verbose, f=file)।
- tar=বাঁধা, bzip2=ছোট করা — দুই আলাদা কাজ।

**Exam-এ যেভাবে আসতে পারে:**
"tar আর bzip2-এর তফাত?" → tar অনেক file এক bundle করে; bzip2 সেটা compress করে।
"archive খোলার command?" → `bunzip2` তারপর `tar -xvf`।
"অনেক file কেন compress করে পাঠাই?" → এক bundle + ছোট size, network-এ transfer সহজ ও দ্রুত।

---

## 11. `scp` — secure copy (network-এ)

🎯 **Exam গুরুত্ব: Medium** — cp vs scp, directory copy। ~3–5 marks।

**এক কথায়:**
`scp` = `cp`-এর মতো, কিন্তু **network-এর ওপর দিয়ে** দুই machine-এর মধ্যে।
নিরাপদভাবে (SSH ব্যবহার করে)।

### ধারণা (Concept)
- `cp` → একই machine-এ copy।
- `scp` → দুই Internet-যুক্ত machine-এর মধ্যে copy।
- format: `scp <source> <destination>`।

### গভীর ব্যাখ্যা (ছোট ধাপে)

একটা file পাঠানো:
- `scp file.txt bob@192.168.1.50:/home/bob/`
- মানে: file.txt পাঠাও bob-এর machine-এর ওই path-এ।
- password চাইবে (SSH-এর মতো)।

পুরো folder পাঠানো:
- `scp -r myfolder bob@192.168.1.50:/home/bob/`
- `-r` = recursive (folder + ভেতরের সব)।
- (মনে আছে? `cp`-তেও folder copy-তে `-r` লাগে)।

absolute path:
- `/` দিয়ে শুরু path = absolute (পুরো ঠিকানা)।
- যেমন `:/home/bob/files/`।

আটকে গেলে:
- `man scp` → পুরো manual পড়ো।

### উদাহরণ
```
scp report.txt bob@192.168.1.50:/home/bob/    # এক file
scp -r books bob@192.168.1.50:/home/bob/       # পুরো folder
```

### কখন ও কেন
Server-এ file আপলোড/ডাউনলোড, backup সরানো — রোজকার।
SSH চলে এমন যেকোনো machine-এ কাজ করে।

### টিপস
`scp` = network-এর cp, secure।
folder = `-r`।
format: `scp source user@ip:/path`।

**Exam-এ যেভাবে আসতে পারে:**
"cp আর scp-র তফাত?" → cp একই machine-এ; scp network-এ দুই machine-এর মধ্যে, SSH দিয়ে secure।
"scp-তে folder পাঠাতে?" → `scp -r`।

---

## 12. gedit over SSH — X11 forwarding (Challenge)

🎯 **Exam গুরুত্ব: Low** — concept; কেন GUI app remote-এ আটকায়। ~2–3 marks।

**এক কথায়:**
SSH-এ ঢুকে `gedit` চালাতে গেলে সাধারণত **ব্যর্থ** হয়।
কারণ gedit graphical, কিন্তু SSH শুধু text দেয়।

### ধারণা (Concept)
- SSH = text-based remote shell।
- gedit = graphical app, একটা **display** দরকার।
- text-only session-এ display নেই → gedit খুলতে পারে না।

### গভীর ব্যাখ্যা (ছোট ধাপে)

কেন fail করে (Challenge 2-এর উত্তর):
- gedit-এর window দেখানোর জায়গা (X display) নেই।
- তাই error দেয় / খোলে না।

কীভাবে কাজ করানো যায়:
- `ssh -X bob@ip` দিয়ে login করো।
- `-X` = **X11 forwarding**।
- এটা remote GUI-কে তোমার screen-এ পাঠায়।
- তাহলে gedit তোমার machine-এ দেখাবে (ধীর হতে পারে)।

মূল শিক্ষা:
- remote-এ text কাজ = সহজ (nano)।
- remote-এ GUI = বাড়তি setup (X forwarding) লাগে।
- তাই server-এ সবসময় nano/vim ব্যবহার করা হয়।

### উদাহরণ
```
ssh bob@192.168.1.50        # এখানে gedit → fail
ssh -X bob@192.168.1.50     # X forwarding → gedit চলতে পারে
```

### টিপস
সাধারণ SSH-এ GUI app চলে না (display নেই)।
চালাতে `ssh -X` (X11 forwarding)।
তাই remote-এ nano ভালো, gedit নয়।

**Exam-এ যেভাবে আসতে পারে:**
"SSH-এ gedit কেন চলে না?" → SSH text-only, display নেই; gedit GUI app।
"কীভাবে চালানো যায়?" → `ssh -X` (X11 forwarding)।

---

# সারাংশ (Full Summary)

1. **Setup:** দুই VM একই NAT network-এ; clone হলে একই MAC→একই IP, তাই MAC re-init।
2. **Nginx:** web server; `sudo apt install nginx-full`; install হলেই page serve করে।
3. **127.0.0.1:** loopback/localhost = নিজের machine; সব machine-এ থাকে।
4. **Web edit:** `/var/www/html/` root-owned → `sudo` লাগে; Nginx আগে `index.html` দেখায়।
5. **Nmap:** খোলা port scan; 80=web, 22=ssh; service বন্ধ→port বন্ধ।
6. **UFW:** firewall; `enable`=সব block; `allow 80/tcp`=web খোলা; system-level তাই `sudo`।
7. **SSH:** secure remote terminal; port 22; আটকালে `ufw allow 22/tcp`।
8. **/etc/passwd:** সব user-এর তালিকা (password নয়); adduser=নতুন লাইন।
9. **SSH revisited:** নাম না দিলে নিজের username; আলাদা=`user@ip`; logout=`exit`।
10. **Archives:** `wget`=download; `tar cf`=bundle; `bzip2`=compress; খোলা `bunzip2`+`tar -xvf`; tar≠compress।
11. **scp:** network-এর cp, secure; folder=`-r`; `scp source user@ip:/path`।
12. **gedit over SSH:** text-only বলে fail; `ssh -X` (X11 forwarding) লাগে।

---

# বাস্তব ব্যবহার টেবিল (Lab → Real Project)

| Topic | Lab-এ শিখলাম | Career-এ কোথায় লাগে |
|---|---|---|
| VM/network setup | দুই machine connect | lab/test environment বানানো |
| Nginx | web server চালানো | website/app hosting, cloud deploy |
| 127.0.0.1 | loopback বোঝা | লোকাল testing |
| web edit + sudo | root-owned file edit | server config বদলানো |
| Nmap | port scan | security audit, recon |
| UFW | firewall নিয়ম | server hardening, attack surface কমানো |
| SSH | remote access | প্রতিটা cloud server manage |
| /etc/passwd | user system | access ব্যবস্থাপনা |
| scp | network file copy | server-এ deploy/backup সরানো |
| tar/bzip2/wget | archive ও download | backup, log archive, transfer |
| X11 forwarding | remote GUI | দূরের GUI app চালানো |

---

# রিফ্লেকশন (Reflection — assignment style)

এই lab-এ আমি শিখেছি একটা Linux machine কীভাবে **service** দেয় — Nginx দিয়ে web page serve করা থেকে শুরু করে SSH দিয়ে remote access পর্যন্ত।

সবচেয়ে গুরুত্বপূর্ণ ছিল security-র তিনটি স্তর একসাথে দেখা:
- Nmap দিয়ে বোঝা কোন দরজা (port) খোলা।
- UFW দিয়ে সেই দরজা নিয়ন্ত্রণ করা।
- SSH দিয়ে নিরাপদে দূরের machine চালানো।

আমি বুঝেছি প্রতিটা service-এর একটা port আছে, আর নিরাপত্তা মানে শুধু দরকারি port খোলা রেখে বাকি সব বন্ধ রাখা — এটাই least privilege-এর network রূপ।

`/etc/passwd`, `adduser`, আর file permission আমাকে দেখিয়েছে Linux কীভাবে user-ভিত্তিক access নিয়ন্ত্রণ করে। আর tar/bzip2/scp শিখিয়েছে GUI ছাড়াই file গুছিয়ে, ছোট করে, network-এ নিরাপদে সরানো — যা server জগতে রোজকার।

যেহেতু আমার লক্ষ্য cloud ও security engineering, এই lab আমার পথের একদম কেন্দ্রে — web service, firewall, remote access আর access control — এগুলোই আমার ভবিষ্যৎ কাজের মূল উপাদান।

---

# দ্রুত রিভিশন + সম্ভাব্য প্রশ্ন (শেষ ৩০ মিনিট)

### 🔴 সবচেয়ে বেশি নম্বরের জায়গা
- **Service + port + firewall চেইন:** Nginx (80) → Nmap (port scan) → UFW (allow/deny) → SSH (22)। এই গল্পটা একসাথে বোঝো।
- **127.0.0.1** = loopback (concept প্রশ্ন)।
- **/etc/passwd** গঠন + adduser-এ কী বদলায়।
- **tar vs bzip2** (bundle vs compress) + খোলার command।
- **permission error fix** = `sudo` (root-owned file)।

### সম্ভাব্য প্রশ্ন + এক-লাইন উত্তর
1. Nginx কী? → web server, page serve করে।
2. 127.0.0.1-এ special? → loopback/localhost, নিজের machine, সব machine-এ থাকে।
3. web page edit-এ permission error fix? → `sudo` দিয়ে edit (folder root-owned)।
4. index.html vs index.nginx-debian.html? → index.html আগে দেখায়।
5. Nmap কী করে? → খোলা port scan করে।
6. Nginx remove করলে nmap-এ? → port 80 আর খোলা থাকে না।
7. UFW-তে কেন sudo? → firewall system-level, root লাগে।
8. ufw enable-এর পর nmap? → port block/filtered।
9. web access ফেরাতে? → `sudo ufw allow 80/tcp`।
10. SSH কী? কোন port? → secure remote terminal; port 22।
11. SSH আটকালে? → firewall 22 block; `ufw allow 22/tcp`।
12. /etc/passwd-এ কী? → user তালিকা (password নয়, সেটা /etc/shadow)।
13. adduser-এ কী বদলায়? → passwd-এ নতুন লাইন যোগ।
14. নির্দিষ্ট user দিয়ে SSH? logout? → `ssh user@ip`; `exit`।
15. tar vs bzip2? → tar bundle করে; bzip2 compress করে।
16. archive খোলা? → `bunzip2` তারপর `tar -xvf`।
17. কেন compress করে পাঠাই? → এক bundle + ছোট size, transfer সহজ/দ্রুত।
18. cp vs scp? → scp network-এ দুই machine-এর মধ্যে, secure।
19. scp-তে folder? → `scp -r`।
20. SSH-এ gedit কেন চলে না? কীভাবে চালাবে? → text-only, display নেই; `ssh -X`।

> 💡 শেষ টিপ:
> command-write প্রশ্নে syntax পরিষ্কার লেখো।
> "কেন/পার্থক্য" প্রশ্নে এক লাইনে মূল কারণ দাও।
> এই lab-এর প্রাণ = **service চালানো + firewall দিয়ে নিয়ন্ত্রণ + SSH দিয়ে remote access**।
> এই তিনটা একসাথে গেঁথে নিলে বেশিরভাগ প্রশ্ন cover হয়ে যাবে।
