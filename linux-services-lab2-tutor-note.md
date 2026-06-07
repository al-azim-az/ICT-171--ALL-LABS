# Linux Services (Lab 2) — Tutor Note

> এই note তোমার জন্য একজন tutor-এর মতো লেখা।
> প্রতিটা ধাপে ৩টা প্রশ্নের উত্তর আছে:
> **কেন** করছি → **কী** করছি → **কীভাবে** করছি।
> 🔑 = নতুন শব্দের মানে।
> 📌 = exam point।
> ছোট ছোট লাইন, এক লাইনে এক কথা। তাড়াহুড়ো নেই।

---

# ০. সবচেয়ে আগে — পুরো Lab 2 কেন? (বড় ছবি)

এই একটা জিনিস বুঝলে বাকি সব সহজ হবে।

**একটা প্রশ্ন দিয়ে শুরু করি:**
- Internet-এ website, app — এগুলো কোথায় থাকে?
- উত্তর: কোনো না কোনো **server** computer-এ।
- সেই server-গুলো প্রায় সবই **Linux**, আর তাদের কোনো **GUI (মাউস-পর্দা) নেই**।
- সব চালাতে হয় শুধু লেখা **command** দিয়ে।

**তাই Lab 2-এর উদ্দেশ্য:**
- তোমার Linux machine-কে একটা **server** বানানো শেখা।
- যে server অন্যদের **service** দেয় (যেমন web page)।
- আর সেটা command line দিয়ে **নিরাপদে চালানো ও পাহারা দেওয়া** শেখা।

🔑 **Service কী?**
- কী: একটা program যে অন্যদের কিছু দেয় (web page, login, ইত্যাদি)।
- এই lab-এ তুমি দুটো service চালাবে: web (Nginx) আর remote login (SSH)।

**এক লাইনে পুরো lab:**
> "আমার computer-কে এমন একটা server বানাই যে অন্যদের service দেয় —
> তারপর firewall দিয়ে পাহারা দিই, আর SSH দিয়ে দূর থেকে চালাই।"

এটাই গল্প। এখন ধাপে ধাপে যাই।

---

# ১. Setup — দুই computer কেন লাগবে?

### 🤔 কেন এই ধাপ? (তোমার আসল প্রশ্ন)

ভাবো —
- তুমি একটা server বানালে, যে service দেয়।
- কিন্তু service তো **অন্য কাউকে** দেওয়ার জিনিস।
- একা থাকলে তুমি শুধু **নিজের সাথে** কথা বলতে পারো।
- তাহলে "অন্যকে service দেওয়া" কীভাবে শিখবে?

**উত্তর: দুটো computer লাগবে।**
- একটা = তোমার server (যে দেয়)।
- আরেকটা = client (যে চায়/access করে)।
- তুমি **দুই ভূমিকাই** দেখবে — দেওয়া আর নেওয়া।

এটাই আসল internet-এর ছবি:
```
[ Client ]  ──"page দাও"──►  [ Server ]
 (চায়)      ◄──page পাঠায়──   (দেয়)
```

**কোথায় দ্বিতীয় computer পাবে?**
- ক্যাম্পাসে: তোমার **partner**-এর computer = দ্বিতীয়টা।
- বাড়িতে একা: কারো computer নেই।
- তাই নিজের একটা VM **clone** করে দ্বিতীয় machine বানাও।
- 👉 এই কারণেই "বাড়িতে দুই VM লাগবে"।

### 🔑 দরকারি শব্দ

🔑 **Virtual Machine (VM)**
- কী: তোমার computer-এর ভেতরে software দিয়ে বানানো নকল computer।
- কেন: আসল computer নষ্ট না করে এক্সপেরিমেন্ট করা যায়।
- যেমন: ঘরের ভেতরে খেলনা-ঘর — ভাঙলেও আসল ঘর ঠিক।

🔑 **Network**
- কী: কয়েকটা computer যুক্ত হয়ে কথা বলার ব্যবস্থা।
- কেন: একটা আরেকটার সাথে data দেয়-নেয়।
- যেমন: একটা পাড়া — সবাই কাছাকাছি।

🔑 **Clone**
- কী: একটা VM-এর হুবহু নকল।
- কেন/লাভ: আবার পুরো Ubuntu বসাতে হয় না; এক ক্লিকে দ্বিতীয় machine।
- যেমন: ফটোকপি।

🔑 **NAT network**
- কী: একটা ভার্চুয়াল পাড়া, যেখানে দুই VM-কে বসাও।
- কেন: তখন দুই VM একে অপরকে দেখতে পায়, internet-ও পায়।

### 🪜 ধাপে ধাপে (কী করছি + কেন)

**ধাপ ১ — VM clone করো।**
- কেন: দ্বিতীয় একটা computer দরকার।

**ধাপ ২ — দুটোকে একই NAT network-এ বসাও।**
- কেন: একই পাড়ায় না থাকলে একে অপরকে পাবে না।

**ধাপ ৩ — একটা সমস্যা আসবে (lab-এর প্রশ্ন):**
- দুই VM-এর **একই IP** দেখাবে।
- কেন? কারণ এখানে দুটো নতুন শব্দ —

🔑 **MAC address**
- কী: network card-এর জন্মগত unique নম্বর।
- কেন: network-এ কোন device কে — চেনার জন্য।
- যেমন: আঙুলের ছাপ।

🔑 **IP address**
- কী: network-এ computer-এর ঠিকানা।
- কেন: কোথায়/কে চিঠি পাঠাল জানার জন্য।
- যেমন: বাড়ির ঠিকানা।

- clone হুবহু এক → দুই VM-এর **একই MAC**।
- একই MAC → network আলাদা করতে পারে না → **একই IP**।
- দুই বাড়ির একই আঙুলের ছাপ → ডাকপিয়ন গুলিয়ে ফেলে।

**ধাপ ৪ — সমাধান:**
- একটা VM-এর MAC **re-initialize** করো (নতুন unique নম্বর)।
- এখন আলাদা MAC → আলাদা IP।

**ধাপ ৫ — যাচাই করো:**
```
ip a                          # নিজের IP দেখো
ping <অন্য-computer-এর-IP>     # সাড়া এলে দুজন যুক্ত
```

🔑 **ping**
- কী: ছোট "তুমি আছ?" সংকেত পাঠিয়ে উত্তর আসে কিনা দেখা।
- কাজ: উত্তর এলে = connection ঠিক আছে।

📌 **Exam point:** দুই cloned VM-এর একই IP কেন? → একই MAC (clone হুবহু এক)। সমাধান: MAC re-initialize।

**🧠 এই part এক লাইনে:**
দুই computer দরকার (server + client); একা হলে clone করো; clone-এর MAC/IP আলাদা করো।

---

# ২. Nginx — কেন একটা web server বানাচ্ছি?

### 🤔 কেন এই ধাপ?
- Lab-এর মূল লক্ষ্য = machine-কে server বানানো।
- সবচেয়ে সহজ ও common server = **web server** (যে web page দেয়)।
- তাই আমরা Nginx বসাই — তোমার machine এখন একটা website serve করবে।

### 🔑 দরকারি শব্দ

🔑 **Server** = যে কিছু দেয় (দোকান)।
🔑 **Client** = যে কিছু চায় (খদ্দের — partner-এর browser)।
🔑 **Web server** = যে web page দেয়। Nginx এটা করে।
🔑 **Nginx** = একটা জনপ্রিয় web server software।

### 🪜 ধাপে ধাপে (কী + কেন)

**ধাপ ১ — তালিকা refresh:**
```
sudo apt update
```
🔑 **apt** = software install করার tool।
🔑 **repository** = Ubuntu-র যাচাই করা online software ভাণ্ডার।
- কেন এই command: install-এর আগে দোকানের নতুন stock-লিস্ট নেওয়া, যাতে পুরোনো version না আসে।

**ধাপ ২ — Nginx বসাও:**
```
sudo apt install nginx-full
```
- কেন: এটাই আসল কাজ — web server বসানো।
- install হলেই Nginx চালু → তোমার machine এখন page দেয়।

**ধাপ ৩ — আরও দুটো tool বসাও (পরে লাগবে):**
```
sudo apt install nmap
sudo apt install openssh-server
```
- কেন nmap: পরে দরজা (port) গোনার জন্য।
- কেন openssh-server: পরে SSH (দূর থেকে login) চালু করতে।

🔑 **HTTP** = browser আর web server-এর কথা বলার ভাষা। (দুজন আগে ঠিক করল কোন ভাষায় কথা বলবে।)

**কেন সব command-এ `sudo`?**
- software বসানো = system-level কাজ।
- এর জন্য root-power লাগে → `sudo`। (পরের part-এ বিস্তারিত।)

📌 **Exam point:** Nginx কী? → web server, page দেয়। Install? → `sudo apt install nginx-full`।

**🧠 এক লাইনে:** Nginx বসিয়ে machine-কে web page-দেওয়া server বানালাম।

---

# ৩. `127.0.0.1` ও IP — কেন নিজের page দেখি, কেন আলাদা IP?

### 🤔 কেন এই ধাপ?
- Server বানালাম। কিন্তু কাজ করছে কিনা **নিজে আগে দেখা** দরকার।
- partner-কে দেখানোর আগে নিজে test করি।
- আর partner-কে দেখাতে হলে নিজের **ঠিকানা (IP)** লাগবে।

### 🔑 দরকারি শব্দ

🔑 **127.0.0.1 (localhost / loopback)**
- কী: "এই computer-টাই" — নিজের ঠিকানা।
- কেন: নিজের server নিজে test করা যায়, internet ছাড়াও।
- যেমন: নিজের ঘরে নিজেকে চিঠি — বাইরে যায় না।

তাই:
```
http://127.0.0.1     # নিজের Nginx page
```
- কেন: partner-কে বিরক্ত না করে নিজে আগে দেখে নিচ্ছি কাজ করছে কিনা।

🔑 **Port (দরজা)**
- কী: computer-এর ভেতরের অনেক "দরজার" নম্বর।
- কেন: এক ঠিকানায় অনেক service চলে; কোন চিঠি কে পাবে — port ঠিক করে।
- যেমন: 80 = web, 22 = SSH।

**তোমার computer = একটা বিল্ডিং:**
```
        তোমার computer (বিল্ডিং)
        IP: 192.168.1.10  ← ঠিকানা
   ┌──────────────────────────────┐
   │ দরজা ৮০  → Nginx (web page)   │
   │ দরজা ২২  → SSH  (remote login)│
   │ দরজা ৪৪৩ → খালি / বন্ধ        │
   └──────────────────────────────┘
```
- IP = বিল্ডিংয়ের ঠিকানা।
- port = ভেতরের কোন দরজা।
- service = দরজার পেছনে বসা লোক।

**এখন একটা গোলমাল (lab-এর প্রশ্ন): দুই জায়গায় আলাদা IP কেন?**

🔑 **Private IP**
- কী: শুধু নিজের পাড়ার ভেতরের ঠিকানা।
- কাজ: `192.168.x.x` / `10.x.x.x`। internet-এ সরাসরি চলে না।
- যেমন: অফিসের room number।

🔑 **Public IP**
- কী: পুরো internet-এ unique ঠিকানা।
- কাজ: ISP দেয়, বাইরের জগৎ এটাই দেখে। `134.115.x.x` এমন।
- যেমন: পুরো বিল্ডিংয়ের street address।

🔑 **NAT**
- কী: ভেতরের অনেক private ঠিকানাকে একটা বাইরের public ঠিকানায় **অনুবাদ**।
- কেন/লাভ: public IP সীমিত; একটা দিয়ে অনেক device চলে; ভেতরের device লুকানো থাকে (নিরাপদ)।
- কাজ: router এটা করে।

**ছবি:**
```
  তোমার পাড়া (private, ভেতরের)         Internet (বাইরের)
 ┌──────────────────────────┐
 │ VM     192.168.1.10       │
 │ Laptop 192.168.1.5        │──►[ Router ]──► 1 public IP
 │ Phone  192.168.1.6        │     (NAT)       134.115.20.7
 └──────────────────────────┘
```
- তাই `ip a` দেখায় `192.168.1.10` (private)।
- website দেখায় `134.115.20.7` (router-এর public)।
- মাঝখানে **NAT** অনুবাদ করছে — তাই আলাদা।

**partner-কে দেখাতে:**
```
ip a        # নিজের IP বের করো, partner-কে দাও
```
- partner browser-এ তোমার IP লিখলে → তোমার page দেখবে।

📌 **Exam point:**
- 127.0.0.1 কী? → loopback, নিজের machine।
- ip a আর website আলাদা IP কেন? → private vs public; NAT অনুবাদ করে।
- private range? → 192.168.x / 10.x / 172.16–31.x।

**🧠 এক লাইনে:** নিজের page নিজে দেখি (127.0.0.1); partner-কে দিই আসল IP; ভেতরের IP private, বাইরের public, NAT মাঝে।

---

# ৪. Page edit — কেন permission লাগে, কেন `sudo`?

### 🤔 কেন এই ধাপ?
- Default page-টা তো Nginx-এর।
- আমরা চাই **নিজের** কিছু লিখতে।
- তাই page-এর HTML বদলাই।
- কিন্তু একটা বাধা আসে — সেটাই শেখার আসল জিনিস।

### 🔑 দরকারি শব্দ

🔑 **Owner**
- কী: প্রতিটা file/folder-এর একজন মালিক।
- কেন: কে বদলাতে পারবে — নিয়ন্ত্রণ।
- যেমন: ঘরের চাবি যার কাছে।

🔑 **root**
- কী: সবচেয়ে ক্ষমতাবান user — সব পারে।
- যেমন: বিল্ডিংয়ের master-key মালিক।

page থাকে `/var/www/html/`-এ। এর owner = **root**।
- কেন root? কারণ এটা system-এর গুরুত্বপূর্ণ জায়গা, যেন যে কেউ বদলাতে না পারে।

### 🪜 ধাপে ধাপে

**ধাপ ১ — সাধারণভাবে edit চেষ্টা:**
```
nano /var/www/html/index.nginx-debian.html
```
- "Permission denied" আসবে।

🔑 **Permission**
- কী: কে কী করতে পারবে তার অনুমতি।
- কেন denied: তুমি এই folder-এর owner নও (owner root)।

**ধাপ ২ — fix:**
```
sudo nano /var/www/html/index.nginx-debian.html
```
🔑 **sudo** = এক command-এর জন্য সাময়িক root-power।
- কেন: root হিসেবে চালালে এখন save হবে।

🔑 **nano vs gedit:**
- nano: terminal-এর ভেতরে, GUI লাগে না → server/remote-এ চলে।
- gedit: graphical, display লাগে → remote terminal-এ চলে না।

**ধাপ ৩ — নতুন page বানাও:**
```
sudo nano /var/www/html/index.html
```

🔑 **index file (lab-এর প্রশ্ন)**
- কী: address-এ গেলে যে page প্রথমে দেখায়।
- নিয়ম: Nginx আগে `index.html` খোঁজে, না পেলে `index.nginx-debian.html`।
- তাই নতুন `index.html` বানালে তোমারটাই default; পুরোনোটা থাকে, শুধু আর দেখায় না।

📌 **Exam point:**
- permission error fix? → `sudo` (folder root-owned)।
- remote-এ gedit চলবে? → না; nano চলবে।
- কোন index আগে? → `index.html`।

**🧠 এক লাইনে:** system file edit-এ root-power (`sudo`) লাগে; Nginx `index.html` আগে দেখায়।

---

# ৫. Nmap — কেন বাইরে থেকে দরজা গুনি?

### 🤔 কেন এই ধাপ?
- তোমার server কাজ করছে।
- কিন্তু **বাইরে থেকে কী কী দেখা যাচ্ছে?**
- কোন দরজা খোলা = কোন service বাইরে উন্মুক্ত।
- এটা জানা security-র প্রথম ধাপ (নিজেরটা জানো, নয়তো রক্ষা করবে কী করে)।

### 🔑 দরকারি শব্দ

🔑 **Nmap**
- কী: বাইরে থেকে দেখার যন্ত্র — কোন port (দরজা) খোলা।
- কাজ: প্রতিটা দরজায় টোকা দিয়ে দেখে কে সাড়া দেয়।

🔑 **open / closed / filtered:**
- open = দরজায় service আছে, সাড়া দেয়।
- closed = দরজা আছে, কেউ নেই।
- filtered = দারোয়ান (firewall) আটকেছে।

### 🪜 ধাপে ধাপে

**ধাপ ১ — partner-এর machine scan:**
```
nmap <partner-এর-IP>
```
- কেন: দেখা কোন দরজা খোলা, কোন service চলছে।

**ফল পড়া (lab-এর প্রশ্ন):**
- `80/tcp open http` → Nginx চলছে।
- `22/tcp open ssh` → SSH চলছে।
- port নম্বর দেখেই service চেনা যায়।

**ধাপ ২ — Nginx সরিয়ে আবার scan (lab-এর প্রশ্ন):**
```
sudo apt remove nginx-full
sudo apt autoremove
```
- কেন: প্রমাণ করা — service বন্ধ করলে দরজাও বন্ধ হয়।
- receptionist চলে গেল → দরজা ৮০-তে কেউ নেই।
- আবার nmap → **port 80 আর open নয়**।

**ধাপ ৩ — ফেরাও:**
```
sudo apt install nginx-full
```

📌 **Exam point:** Nmap কী করে? → খোলা port scan। Nginx remove করলে? → port 80 আর open নয় (service বন্ধ)।

**🧠 এক লাইনে:** Nmap বাইরে থেকে দেখায় কোন service উন্মুক্ত; service বন্ধ = দরজা বন্ধ।

---

# ৬. UFW — কেন firewall লাগবে?

### 🤔 কেন এই ধাপ?
- Nmap-এ দেখলে — তোমার দরজা বাইরে থেকে খোলা।
- যে কেউ ঢুকতে পারে = বিপদ।
- তাই একজন **দারোয়ান** দরকার, যে অবাঞ্ছিত লোক আটকাবে।
- শুধু দরকারি দরজা খোলা রাখবে।

### 🔑 দরকারি শব্দ

🔑 **Firewall**
- কী: network-এর দারোয়ান — কে ঢুকবে ঠিক করে।
- কেন/লাভ: অবাঞ্ছিত প্রবেশ আটকানো।
- যেমন: গেটের দারোয়ান।

🔑 **UFW** = Ubuntu-র সহজ firewall tool।

🔑 **tcp** = data পাঠানোর একটা নিয়ম (নিশ্চিত করে সব ঠিকঠাক পৌঁছেছে)। `80/tcp` = দরজা ৮০, tcp নিয়মে।

### 🪜 ধাপে ধাপে

**কেন সব command-এ `sudo` (lab-এর প্রশ্ন)?**
- firewall system-level নিরাপত্তা; সাধারণ user বদলাতে পারে না; root লাগে।

**ধাপ ১ — অবস্থা দেখো:**
```
sudo ufw status verbose
```
- শুরুতে সাধারণত inactive (দারোয়ান নেই)।

**ধাপ ২ — দারোয়ান বসাও:**
```
sudo ufw enable
```
- এখন **সব দরজা বন্ধ** (default deny)।
- partner nmap করলে port গুলো **filtered/block** দেখাবে।

🔑 **default deny:** firewall চালু হলে default-এ সব বন্ধ; তুমি দরকারিটা খোলো। কম দরজা খোলা = কম ঝুঁকি।

**ধাপ ৩ — দরকারি দরজা খোলো:**
```
sudo ufw allow 80/tcp
```
- কেন: web page তো partner-কে দেখাতে চাই।
- এখন দরজা ৮০ খোলা, বাকি বন্ধ।

📌 **Exam point:**
- UFW-তে কেন sudo? → system-level, root লাগে।
- ufw enable-এর পর nmap? → port block/filtered।
- web ফেরাতে? → `sudo ufw allow 80/tcp`।

**🧠 এক লাইনে:** firewall দারোয়ান; চালু = সব বন্ধ; দরকারি port `allow` করো।

---

# ৭. SSH — কেন দূর থেকে access?

### 🤔 কেন এই ধাপ?
- আসল server-গুলো অন্য শহরে/দেশে data center-এ থাকে।
- তুমি সশরীরে সেখানে যেতে পারবে না।
- তাই দরকার — **দূর থেকে** তার terminal নিয়ে কাজ করা।
- সেটাই SSH।

### 🔑 দরকারি শব্দ

🔑 **SSH**
- কী: দূর থেকে অন্য computer-এর terminal নিয়ে কাজ।
- কাজ: দরজা **২২** ব্যবহার করে; username+password লাগে।
- যেমন: রিমোট দিয়ে দূরের বাড়ির কাজ চালানো।

🔑 **Encryption** (SSH-এর "Secure")
- কী: data-কে গোপন কোডে বদলানো, যাতে মাঝপথে কেউ পড়তে না পারে।
- কেন: password/command নিরাপদ থাকে।
- যেমন: তালা দেওয়া বাক্সে চিঠি।

🔑 **openssh-server** = যে software দরজা ২২-এ SSH চালু রাখে (Part 2-এ install করা)।

### 🪜 ধাপে ধাপে

**ধাপ ১ — login চেষ্টা:**
```
ssh <partner-এর-IP>
```
- দরজা ২২ দিয়ে ঢোকো, password দাও।
- prompt বদলে partner-এর machine দেখাবে।

**ধাপ ২ — না ঢুকলে (lab-এর প্রশ্ন): firewall?**
- হ্যাঁ। দারোয়ান (UFW) যদি দরজা ২২ বন্ধ রাখে → পৌঁছাবে না।
```
sudo ufw allow 22/tcp        # দরজা ২২ খোলা
```

🔑 **মূল ধারণা:** কাজ করতে **দুটোই** লাগে —
- service থাকা (openssh-server),
- দরজা খোলা (ufw allow 22)।

📌 **Exam point:** SSH কী/কোন port? → secure remote terminal, port 22। ঢুকছে না কেন? → firewall 22 block; `ufw allow 22/tcp`।

**🧠 এক লাইনে:** SSH = দরজা ২২ দিয়ে নিরাপদে দূরের machine চালানো; service + খোলা দরজা দুটোই লাগে।

---

# ৮. User ও `/etc/passwd` — কেন নতুন user?

### 🤔 কেন এই ধাপ?
- একটা server-এ অনেকজন কাজ করতে পারে।
- প্রত্যেকের আলাদা পরিচয় দরকার (যেন একজন আরেকজনের জিনিস না ঘাঁটে)।
- তাই user বানানো ও user system বোঝা জরুরি।

### 🔑 দরকারি শব্দ

🔑 **User account**
- কী: একজনের আলাদা পরিচয় (username+password)।
- কেন: প্রত্যেকের file/setting আলাদা ও নিরাপদ।
- যেমন: এক বিল্ডিংয়ে আলাদা flat।

🔑 **/etc/passwd**
- কী: সব user-এর তালিকা রাখা file।
- যেমন: বাসিন্দাদের রেজিস্টার খাতা।

### 🪜 ধাপে ধাপে

**ধাপ ১ — তালিকা দেখো:**
```
less /etc/passwd
```
একটা লাইন:
```
azim:x:1000:1000:Azim:/home/azim:/bin/bash
```
- `azim` → username
- `x` → password এখানে নেই (নিচে দেখছি কোথায়)
- `1000` → UID, `1000` → GID
- `/home/azim` → home folder
- `/bin/bash` → login shell

🔑 **UID** = প্রতিটা user-এর unique নম্বর। system নাম নয়, নম্বর দিয়ে চেনে। root-এর UID = 0।

🔑 **/etc/shadow** = আসল password (encrypt করা) যেখানে থাকে।
- কেন আলাদা: passwd সবাই পড়তে পারে, তাই password আলাদা গোপন file-এ (শুধু root পড়ে)।

**ধাপ ২ — নতুন user বানাও:**
```
sudo adduser bob             # নতুন user + password
less /etc/passwd             # bob-এর নতুন লাইন দেখবে
```
- কেন `sudo`: user বানানো system-level, root লাগে।

📌 **Exam point:** /etc/passwd-এ কী? → user তালিকা (password নয়, সেটা /etc/shadow)। adduser-এর পর? → নতুন user-এর একটা লাইন যোগ।

**🧠 এক লাইনে:** passwd = বাসিন্দার তালিকা (UID দিয়ে চেনা; password shadow-এ); adduser নতুন বাসিন্দা যোগ করে।

---

# ৯. SSH আবার — কোন নামে, কীভাবে বের হই?

### 🤔 কেন এই ধাপ?
- partner-এর নতুন user বানালে।
- এবার সেই **নির্দিষ্ট user** হয়ে login করে দেখি।
- আর কাজ শেষে ঠিকভাবে বের হওয়া শিখি।

### 🪜 ধাপে ধাপে

**ধাপ ১ — নাম ছাড়া:**
```
ssh 192.168.1.50
```
- নাম না দিলে system ধরে নেয় **তোমার নিজের** username।
- partner-এর machine-এ সেই নাম না থাকলে fail।

**ধাপ ২ — নির্দিষ্ট user দিয়ে:**
```
ssh bob@192.168.1.50
```
- format: `username@ip`।
- bob-এর password দিয়ে ঢোকো।

**ধাপ ৩ — বের হও:**
```
exit
```
🔑 **exit** = session বন্ধ করে নিজের machine-এ ফেরা।
- prompt আবার তোমার machine দেখাবে।
- ভুলে দুবার ঢুকলে দুবার `exit`; prompt দেখে বুঝবে কোথায় আছ।

📌 **Exam point:** নির্দিষ্ট user দিয়ে SSH? → `ssh user@ip`। বের হতে? → `exit`।

**🧠 এক লাইনে:** নাম না দিলে নিজের; আলাদা = `user@ip`; বের হতে `exit`।

---

# ১০. Compressed archives — কেন বেঁধে ছোট করি?

### 🤔 কেন এই ধাপ?
- ভাবো server থেকে ১০০টা file অন্য machine-এ পাঠাতে হবে।
- একটা একটা পাঠানো ধীর ও ঝামেলা।
- তাই সব **এক bundle** করে, **ছোট** করে পাঠাই।
- দ্রুত ও সহজ।

### 🔑 দরকারি শব্দ

🔑 **wget**
- কী: command line দিয়ে web থেকে file নামানোর tool।
- কেন/লাভ: browser ছাড়াই download (server-এ আদর্শ)।

🔑 **tar**
- কী: অনেক file একসাথে **এক bundle** করা।
- মনে রাখো: tar শুধু **বাঁধে**, ছোট করে না।
- যেমন: অনেক বই এক স্যুটকেসে।

🔑 **bzip2 / Compression**
- Compression = file ছোট করা (পুনরাবৃত্তি সংক্ষেপে লেখে)।
- bzip2 = bundle-টাকে ছোট করার tool।
- মনে রাখো: bzip2-ই আসল **ছোট করার** কাজ।
- যেমন: ভরা স্যুটকেস চেপে ছোট করা।

### 🪜 ধাপে ধাপে

**ধাপ ১ — file নামাও:**
```
wget https://www.gutenberg.org/files/76/76-0.txt
```
- কেন wget: server-এ browser নেই, তাই command দিয়ে download।

**ধাপ ২ — গুছাও:**
```
mkdir books
mv 76-0.txt books/           # তিনটা বই books-এ move
```

**ধাপ ৩ — bundle বানাও:**
```
tar cf books.tar books       # c=create, f=file নাম
```
- কেন: অনেক file → এক জিনিস, সরানো সহজ।

**ধাপ ৪ — ছোট করো:**
```
bzip2 books.tar              # হয় books.tar.bz2
```
- কেন: size কমলে পাঠানো দ্রুত।

**ধাপ ৫ — ফল দেখো:**
```
ls -la                       # size তুলনা করো
```
🔑 **Compression ratio** = ছোট size ÷ আসল size। text বলে অনেক ছোট হয়।

**ধাপ ৬ — খুলতে চাইলে:**
```
bunzip2 books.tar.bz2        # আবার books.tar
tar -xvf books.tar           # x=extract, v=verbose, f=file
```

📌 **Exam point:** tar আর bzip2-এর তফাত? → tar bundle করে, bzip2 compress করে। খোলা? → `bunzip2` তারপর `tar -xvf`। কেন compress? → এক bundle + ছোট size, transfer সহজ/দ্রুত।

**🧠 এক লাইনে:** tar বাঁধে, bzip2 ছোট করে; খুলতে bunzip2 + tar -xvf।

---

# ১১. scp ও gedit-over-SSH — কেন network-এ copy?

### 🤔 কেন এই ধাপ?
- bundle তো বানালাম। কিন্তু আরেক machine-এ **পাঠাব কীভাবে**?
- পেনড্রাইভ ছাড়া, network দিয়ে সরাসরি — নিরাপদে।
- সেটাই scp।

### 🔑 দরকারি শব্দ

🔑 **scp**
- কী: `cp`-এর মতো copy, কিন্তু **দুই computer-এর মধ্যে network-এ**।
- কেন: ভেতরে SSH ব্যবহার করে → secure; দরজা ২২ লাগে।
- যেমন: এক বাড়ি থেকে আরেক বাড়িতে তালা দেওয়া বাক্সে কুরিয়ার।

### 🪜 ধাপে ধাপে

**এক file পাঠাও:**
```
scp file.txt bob@192.168.1.50:/home/bob/
```
- `:`-এর পরেরটা = গন্তব্যের path।

**পুরো folder:**
```
scp -r books bob@192.168.1.50:/home/bob/
```
- `-r` = recursive (folder + ভেতরের সব)।

### 🔑 gedit over SSH (Challenge 2)

🔑 **X11 forwarding (`ssh -X`)**
- সমস্যা: SSH-এ ঢুকে `gedit` চালালে fail।
- কেন: SSH শুধু text; gedit graphical, display দরকার।
- সমাধান: `ssh -X bob@192.168.1.50` → remote-এর GUI তোমার screen-এ আসে (ধীর)।
- শিক্ষা: তাই server-এ সবসময় nano/vim, gedit নয়।

📌 **Exam point:** cp আর scp-র তফাত? → scp network-এ দুই machine-এর মধ্যে, SSH দিয়ে secure। SSH-এ gedit কেন চলে না/কীভাবে? → text-only, display নেই; `ssh -X`।

**🧠 এক লাইনে:** scp = SSH দিয়ে network-এ copy; remote GUI চালাতে `ssh -X`।

---

# 🎯 পুরো Lab 2 — গল্পের ক্রম (কেন → কী)

1. **Setup:** server + client দরকার → দুই computer (clone, MAC/IP আলাদা)।
2. **Nginx:** machine-কে server বানালাম (web page দেয়)।
3. **127.0.0.1 + IP:** নিজে test করলাম; partner-কে IP দিলাম; private/public/NAT বুঝলাম।
4. **Page edit:** নিজের page লিখলাম; root-owned তাই `sudo`।
5. **Nmap:** বাইরে থেকে দেখলাম কোন দরজা খোলা।
6. **UFW:** দারোয়ান বসিয়ে দরজা নিয়ন্ত্রণ করলাম।
7. **SSH:** দূর থেকে নিরাপদে ঢুকলাম (দরজা ২২)।
8. **User/passwd:** নতুন user বানালাম; user system বুঝলাম।
9. **SSH আবার:** নির্দিষ্ট user হয়ে ঢুকলাম, `exit` করলাম।
10. **Archives:** অনেক file বেঁধে (tar) ছোট করলাম (bzip2)।
11. **scp:** সেই bundle নিরাপদে অন্য machine-এ পাঠালাম।

**দুটো বড় সত্য ভুলো না:**
- কাজ করতে **service থাকা + firewall দরজা খোলা** — দুটোই লাগে।
- open port = সম্ভাব্য প্রবেশপথ → কম খোলা = বেশি নিরাপদ।

---

# 💌 তোমার জন্য একটা Final Note

Azim, একটা কথা মন দিয়ে শোনো।

এই lab তোমার কঠিন লেগেছে — এটা **স্বাভাবিক**।
- কারণ এখানে অনেকগুলো নতুন abstract জিনিস একসাথে।
- server, port, firewall — এগুলো চোখে দেখা যায় না।
- তাই প্রথমবার ধরতে কষ্ট হয়।

কিন্তু খেয়াল করো — তুমি **হাল ছাড়োনি**।
- বারবার জিজ্ঞেস করেছ "কেন, কীভাবে"।
- এটাই একজন ভালো engineer-এর আসল গুণ।
- যে শুধু মুখস্থ করে না, **বুঝতে চায়**।

একটা সহজ মনে রাখার সূত্র:
> তোমার computer = একটা বিল্ডিং।
> ঠিকানা = IP, দরজা = port, দরজার পেছনে = service।
> দারোয়ান = firewall, দূর থেকে ঢোকা = SSH।

পরীক্ষার আগে শুধু এই note-এর:
- প্রতিটা **🧠 এক লাইনে** আর
- প্রতিটা **📌 Exam point** —
এই দুটো পড়লেই পুরোটা মাথায় ফিরে আসবে।

ধীরে শেখা মানে দুর্বল নয়।
ধীরে কিন্তু **গভীরে** শেখা — সেটাই বেশিদিন থাকে।
তুমি ঠিক পথেই আছ। এগিয়ে যাও। 🚀

— তোমার পাশে থাকা একজন।
