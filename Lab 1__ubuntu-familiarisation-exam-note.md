# Ubuntu Desktop & CLI Familiarisation — Complete Exam-Ready Lab Note

> তোমার নিজের note — একজন senior পাশে বসে বুঝিয়ে দিচ্ছে এমন ভাবে। Bangla-তে, technical term English-এ। Lab sheet-এর প্রতিটা প্রশ্নের উত্তর ভেতরে গাঁথা আছে। Exam + practical দুটোর জন্যই enough।

---

## 📑 Topic List (priority map)

🔴 High · 🟡 Medium · 🟢 Low

**Part A — GUI + CLI + Super User**
1. 🟢 GUI Familiarisation
2. 🟡 Process দেখা — `ps -e`, `top`
3. 🔴 File listing — `ls`, `-la`, `-lah`, `-alt`
4. 🟡 File বানানো ও edit — `touch`, `gedit`, `nano`
5. 🟡 File দেখা — `cat` vs `less`
6. 🔴 Copy ও move — `cp` vs `mv`
7. 🟡 System পরিচয় — `uname -a`, `lsb_release -a`, `hostnamectl`
8. 🔴 Super User — `whoami`, `adduser`, `sudo`, root

**Part B — Networking**
9. 🔴 `ip a` — network interface ও IP
10. 🔴 `ping` — connectivity test
11. 🔴 Hosts file — `/etc/hosts`
12. 🔴 DNS — `nslookup`, `whois`
13. 🔴 Public vs Private IP

**Part C — Hardware, Output, Software**
14. 🟢 Hardware — `lsusb`, `lspci`, `/proc/cpuinfo`
15. 🟡 Output redirection — `>`
16. 🟢 Software: SaaS
17. 🟡 Software: web থেকে binary (`.deb`/`.rpm`)
18. 🔴 Software: repository থেকে (`apt`)
19. 🟡 Software: source code থেকে (`gcc`, `chmod 777`)

---

# Part A — GUI + CLI + Super User

## 1. GUI Familiarisation — Desktop-এর সাথে পরিচয়

🎯 **Exam গুরুত্ব: Low** — সাধারণত concept/MCQ। ~2 marks।
**এক কথায়:** mouse দিয়ে Ubuntu-র graphical পরিবেশে app খোলা, file ঘাঁটা, software install করা — হাতে-কলমে চেনা।

### ১. ধারণা (Concept)
Lab-এর শুরুতে তোমাকে চেনানো হয় Ubuntu Desktop: **Firefox** (internet চলছে কিনা যাচাই), **LibreOffice Writer** (word processor), **file manager** (Nautilus — folder structure-এ ওঠা-নামা), আর **Software Centre / App Centre** (GUI দিয়ে software install)।

### ২. গভীর ব্যাখ্যা — কেন GUI দিয়ে শুরু, terminal-এ জোর কেন
GUI সহজ আর পরিচিত (Windows-এর মতো)। কিন্তু এই lab-এর আসল বার্তা: **internet-এর বেশিরভাগ server-এ কোনো Desktop GUI থাকে না** — শুধু text-based command line। তাই GUI দিয়ে আরাম পেলেও আসল দক্ষতা গড়তে হবে **command line (CLI)**-এ। একটা ভালো অভ্যাস: **terminal আর file browser পাশাপাশি রাখো** — তাহলে command দিলে folder-এ কী বদলাচ্ছে চোখে দেখতে পাবে, concept দ্রুত গেঁথে যায়।

### ৩. উদাহরণ ও practice flow
Firefox খুলে একটা site দেখো → LibreOffice Writer-এ কিছু লেখো → file manager-এ Home থেকে `/` পর্যন্ত উঠে-নেমে দেখো folder structure → Software Centre থেকে একটা app install করো। তারপর terminal পাশে খুলে দেখো একই কাজ command-এ কীভাবে হয়।

### ৪. আউটকাম
GUI-তে confident, আর বুঝে গেছ কেন GUI-র বাইরে CLI শেখা জরুরি।

### ৫. কখন ও কেন ব্যবহার করব
Local dev machine-এ GUI আরামদায়ক; কিন্তু cloud/server (তোমার career-এর মূল ক্ষেত্র) সব CLI।

### ৬. শর্টকাট টিপস
Server = no GUI → CLI must। Terminal + file browser পাশাপাশি রাখলে শেখা সহজ।

---

## 2. Process দেখা — `ps -e` ও `top`

🎯 **Exam গুরুত্ব: Medium** — "কোন command কী দেখায়", "top-এ 1 চাপলে কী হয়"। ~3–4 marks।
**এক কথায়:** computer-এ এই মুহূর্তে কোন কোন program (process) চলছে তা দেখার command — তালিকা চাইলে `ps -e`, live monitor চাইলে `top`।

> GUI চিনলাম; এবার terminal-এ ঢুকে প্রথমেই দেখি — system-এর ভেতরে আসলে কী কী চলছে।

### ১. ধারণা (Concept)
- **Process** = চলমান একটা program-এর instance। প্রতিটার একটা **PID** (Process ID) থাকে।
- `ps -e` → এই মুহূর্তের **সব** process-এর একটা snapshot তালিকা (e = every)।
- `top` → **live**, প্রতি সেকেন্ডে update হওয়া monitor — কে কত CPU/RAM খাচ্ছে, উপরে system summary।

### ২. গভীর ব্যাখ্যা — top-এ `1` চাপলে কী হয়
`top` খুললে উপরে দেখায়: uptime, load average, task সংখ্যা, CPU%, memory ব্যবহার; নিচে process-এর তালিকা (PID, user, %CPU, %MEM সহ)।
**`1` চাপলে:** CPU লাইনটা ভেঙে যায় — সব core একসাথে দেখানোর বদলে **প্রতিটা CPU core আলাদা আলাদা** (Cpu0, Cpu1, Cpu2...) দেখায়। অর্থাৎ এক চাপে বুঝে যাও machine-এ কয়টা core আছে আর প্রতিটার load কত। বের হতে **`q`**।
এগুলো মূলত **diagnostic** — system slow হলে "কে দোষী" খুঁজে বের করার প্রথম হাতিয়ার।

### ৩. উদাহরণ ও পড়া
```
azim@ubuntu:~$ ps -e        # সব process-এর তালিকা (PID + নাম)
azim@ubuntu:~$ top          # live; 1 চাপো → প্রতি core আলাদা; q দিয়ে বের
```
লক্ষ্য করো: `top`-এ সবচেয়ে উপরে যে process, সেটাই সবচেয়ে বেশি CPU খাচ্ছে।

### ৪. আউটকাম
চলমান process তালিকা ও live monitor করতে পারবে; system-এর load বুঝতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
Server slow / hang? → `top` দিয়ে দেখো কোন process CPU/RAM খাচ্ছে। Troubleshooting-এর first move।

### ৬. শর্টকাট টিপস
`ps -e` = snapshot, `top` = live। **top-এ `1` = per-core view, `q` = exit।**

**Exam-এ যেভাবে আসতে পারে:** "top-এ 1 চাপলে কী হয়?" → প্রতিটা CPU core আলাদা করে দেখায়। "চলমান সব process তালিকা দেখার command?" → `ps -e`।

---

## 3. File listing — `ls`, `-la`, `-lah`, `-alt`

🎯 **Exam গুরুত্ব: High** 🔴 — option-এর মানে ও output বলা প্রায় নিশ্চিত। ~5–7 marks।
**এক কথায়:** `ls` folder-এর জিনিস দেখায়; option যোগ করলে আরও তথ্য — লুকানো file, permission, size, time অনুযায়ী sort।

> Process দেখলাম; এবার disk-এ কী আছে — file/folder — সেটা নানা ভাবে দেখি।

### ১. ধারণা (Concept)
`ls` = **l**i**s**t। একা চালালে শুধু নাম দেখায়। Option দিয়ে behaviour বদলায়:
| Command | যা যোগ করে |
|---|---|
| `ls` | শুধু সাধারণ file/folder-এর নাম |
| `ls -l` | **l**ong format: permission, owner, group, size, date |
| `ls -a` | **a**ll: লুকানো (hidden, `.` দিয়ে শুরু) file-ও দেখায় |
| `ls -la` | দুটোই — বিস্তারিত + লুকানো সব |
| `ls -lah` | উপরেরটা + **h**uman-readable size (KB/MB, byte নয়) |
| `ls -alt` | all + long + **t**ime অনুযায়ী sort (নতুন আগে) |

### ২. গভীর ব্যাখ্যা — lab-এর প্রশ্নগুলোর উত্তর
- **`ls` vs `ls -la`:** `ls` শুধু দৃশ্যমান নাম; `ls -la` লুকানো file (`.bashrc` ইত্যাদি) **সহ** প্রতিটার permission/owner/size/date দেখায়। তফাত = তথ্যের গভীরতা + hidden file।
- **`ls -lah` vs `ls`:** `-lah` বিস্তারিত + size মানুষ-পড়ার মতো (যেমন `4.0K`, `1.2M`) দেখায়; `ls` শুধু নাম।
- **`ls -alt`:** `-t` মানে modification **time** অনুযায়ী sort — **সবচেয়ে নতুন file সবার উপরে**। Date/time-এর দিকে তাকালেই বুঝবে। (কাজে লাগে: "শেষ কোন file বদলেছে" খুঁজতে।)

লুকানো file কেন? নাম `.` দিয়ে শুরু হলে Linux সেটা default-এ লুকায় — সাধারণত config file (`.bashrc`, `.ssh`)। `-a` সেগুলো দেখায়।

### ৩. উদাহরণ ও পড়া
```
azim@ubuntu:~$ ls
Documents  Downloads  testfile
azim@ubuntu:~$ ls -lah
total 20K
drwxr-xr-x 2 azim azim 4.0K Jun  6 10:00 Documents
-rw-r--r-- 1 azim azim  1.2K Jun  6 10:05 testfile
-rw-r--r-- 1 azim azim  220  Jun  6 09:00 .bashrc
azim@ubuntu:~$ ls -alt        # .bashrc নয়, সবচেয়ে নতুন file আগে আসবে
```

### ৪. আউটকাম
যেকোনো folder পরিদর্শন করে file-এর নাম, permission, size, কোনটা নতুন — সব এক command-এ বের করতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
"শেষ কী বদলেছে" (`-alt`), "config file আছে কিনা" (`-a`), "permission ঠিক আছে কিনা" (`-l`) — debugging ও security audit-এ প্রতিদিন।

### ৬. শর্টকাট টিপস
`l`=long, `a`=all(hidden), `h`=human size, `t`=time-sort। **`ls -alt` = নতুন file আগে।**

**Exam-এ যেভাবে আসতে পারে:** "`ls` আর `ls -la`-র পার্থক্য?" → -la hidden file সহ বিস্তারিত দেখায়। "`ls -alt` কী করে?" → time অনুযায়ী sort, নতুন উপরে।

---

## 4. File বানানো ও edit — `touch`, `gedit`, `nano`

🎯 **Exam গুরুত্ব: Medium** — "gedit vs nano", remote terminal-এ gedit চলবে কিনা — এই প্রশ্নটা প্রিয়। ~3–4 marks।
**এক কথায়:** খালি file বানাও `touch`; GUI-তে লিখতে `gedit`; terminal-এর ভেতরেই লিখতে `nano`।

> File দেখতে শিখেছি; এবার file বানাই ও তার ভেতরে লিখি।

### ১. ধারণা (Concept)
- `touch testfile` → নতুন খালি file বানায় (থাকলে শুধু timestamp update করে)।
- `gedit testfile` → **GUI** text editor-এ খোলে (mouse, window — Notepad-এর মতো)।
- `nano testfile` → **terminal-এর ভেতরেই** একটা সহজ text editor।

### ২. গভীর ব্যাখ্যা — gedit vs nano (lab-এর মূল প্রশ্ন)
- **gedit** একটা **graphical** app — চলতে একটা desktop/display (GUI) লাগে। তুমি mouse দিয়ে কাজ করো।
- **nano** **পুরোপুরি text-based** — terminal-এর ভেতরেই চলে, কোনো GUI লাগে না।

**👉 Remote terminal-এ (শুধু SSH text prompt) gedit চলবে না** — কারণ gedit-এর graphical window দেখানোর মতো কোনো display নেই। কিন্তু **nano চলবে**, কারণ সে শুধু text চায়। এটাই এই lab-এর মূল শিক্ষা: server-এ (যেখানে GUI নেই) file edit করতে **nano/vim-ই** ভরসা।

**nano চালানো** (নিচে `^` = Ctrl): **save = `Ctrl+O`** (তারপর Enter), **exit = `Ctrl+X`**, search = `Ctrl+W`, লাইন কাটা = `Ctrl+K`।
gedit install করতে না থাকলে: `sudo apt install gedit`।

### ৩. উদাহরণ ও dry run
```
azim@ubuntu:~$ touch testfile        # খালি file
azim@ubuntu:~$ gedit testfile        # GUI-তে লিখে save+exit
azim@ubuntu:~$ nano testfile         # terminal-এ একই file খুলল
# Ctrl+O → Enter (save), Ctrl+X (বের)
```
❗Common error: nano-তে লিখে save (Ctrl+O) না করে Ctrl+X চেপে পরিবর্তন হারানো।

### ৪. আউটকাম
GUI ও terminal — দুই ভাবেই file বানাতে ও edit করতে পারবে; বুঝবে কোনটা কোথায় কাজ করে।

### ৫. কখন ও কেন ব্যবহার করব
Server-এ config বদল (`sudo nano /etc/...`), script লেখা = nano। Local-এ লম্বা লেখা = gedit আরামদায়ক।

### ৬. শর্টকাট টিপস
`touch` = খালি file। **gedit = GUI লাগে (remote-এ চলবে না); nano = terminal-only, চলবে।** nano: save `Ctrl+O`, exit `Ctrl+X`।

**Exam-এ যেভাবে আসতে পারে:** "শুধু remote terminal থাকলে gedit ব্যবহার করা যাবে কি? কেন?" → না, gedit graphical, display লাগে; nano text-based বলে চলবে।

---

## 5. File দেখা — `cat` vs `less`

🎯 **Exam গুরুত্ব: Medium** — তফাত জিজ্ঞেস করে। ~3 marks।
**এক কথায়:** file-এর ভেতর **edit না করে শুধু পড়তে** — পুরোটা একবারে দেখায় `cat`, পাতা-পাতা scroll করে দেখায় `less`।

> Edit করতে শিখলাম; এবার শুধু **দেখার** (read-only) দুটো উপায়।

### ১. ধারণা (Concept)
- `cat testfile` → পুরো content একসাথে screen-এ ঢেলে দেয়।
- `less testfile` → পাতা-পাতা, scroll/up-down করা যায়; বের হতে **`q`**।

### ২. গভীর ব্যাখ্যা — কোনটা কখন (lab-এর প্রশ্ন)
**মূল তফাত:** `cat` একবারে সব দেখায় — ছোট file-এ ভালো, কিন্তু হাজার লাইনের file-এ সব ছুটে যায়, কিছু পড়া যায় না। `less` interactive — পাতা ধরে এগোও, পিছাও, `/keyword` দিয়ে search করো — বড় file-এ আদর্শ। (`cat` আসলে concatenate = জোড়া দেওয়ার জন্য বানানো, পড়া bonus; `less` বানানোই হয়েছে আরাম করে পড়তে।)

### ৩. উদাহরণ
```
azim@ubuntu:~$ cat testfile     # ছোট file — সব দেখায়
azim@ubuntu:~$ less testfile    # বড় file — scroll; q দিয়ে বের
```

### ৪. আউটকাম
File-এর আকার বুঝে সঠিক tool দিয়ে content পড়তে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
Config দ্রুত দেখা = `cat`; বড় log/config পড়া = `less` (`less /etc/hosts`, `less /proc/cpuinfo` — এই lab-এই লাগবে)।

### ৬. শর্টকাট টিপস
ছোট→`cat`, বড়→`less`। less থেকে বের = `q`।

**Exam-এ যেভাবে আসতে পারে:** "`cat` আর `less`-এর পার্থক্য?" → cat পুরোটা একসাথে; less পাতা-পাতা scroll, বড় file-এ ভালো।

---

## 6. Copy ও move — `cp` vs `mv`

🎯 **Exam গুরুত্ব: High** 🔴 — তফাত + command-write প্রায় নিশ্চিত। ~4–6 marks।
**এক কথায়:** `cp` নকল বানায় (মূলটা থাকে), `mv` সরায় বা নাম বদলায় (মূলটা থাকে না)।

> File পড়তে শিখলাম; এবার file নাড়াচাড়া — copy ও move।

### ১. ধারণা (Concept)
- `cp source dest` → **copy**; দুটো file থাকে।
- `mv source dest` → **move/rename**; একটাই file থাকে (নতুন নামে/জায়গায়)।

### ২. গভীর ব্যাখ্যা — lab-এর প্রশ্ন
- `cp testfile testfile2` → testfile-এর হুবহু নকল testfile2 বানায়; **দুটোই থাকে**।
- `mv testfile2 testfile3` → testfile2 আর থাকে না, সে এখন testfile3 — অর্থাৎ **নাম বদলে গেল** (একই folder-এ mv = rename)। ভিন্ন folder-এ দিলে = সরে গেল।
- **মূল তফাত:** cp = duplicate (original অক্ষত), mv = relocate/rename (original গায়েব)।
- folder copy করতে `cp -r` লাগে (recursive); `rm`-এর মতো mv-তেও সাবধান — একই নামে কিছু থাকলে চাপা পড়তে পারে।

### ৩. উদাহরণ ও dry run
```
azim@ubuntu:~$ cp testfile testfile2
azim@ubuntu:~$ ls
testfile  testfile2                  # দুটো — cp duplicate করল
azim@ubuntu:~$ mv testfile2 testfile3
azim@ubuntu:~$ ls
testfile  testfile3                  # testfile2 নেই, নাম বদলে testfile3
```

### ৪. আউটকাম
File নিরাপদে copy, move, rename করতে পারবে; original থাকবে নাকি যাবে — আগে থেকেই জানবে।

### ৫. কখন ও কেন ব্যবহার করব
Backup নেওয়া = `cp`; file গুছিয়ে রাখা / নাম ঠিক করা = `mv`। Script ও deployment-এ রোজ লাগে।

### ৬. শর্টকাট টিপস
**cp = রাখে (copy), mv = সরায় (move/rename)।** folder copy = `cp -r`।

**Exam-এ যেভাবে আসতে পারে:** "`cp` আর `mv`-র পার্থক্য?" → cp original রেখে নকল বানায়; mv original সরিয়ে দেয়/নাম বদলায়।

---

## 7. System পরিচয় — `uname -a`, `lsb_release -a`, `hostnamectl`

🎯 **Exam গুরুত্ব: Medium** — কোনটা কী দেখায়, তফাত। ~3–4 marks।
**এক কথায়:** machine "কী" তা জানার তিন command — kernel-স্তর (`uname`), distro-স্তর (`lsb_release`), আর দুটোর মিশ্রণ + machine info (`hostnamectl`)।

> File নাড়া শিখলাম; এবার জিজ্ঞেস করি — এই machine আসলে কোন OS, কোন kernel, কী নাম।

### ১. ধারণা (Concept)
- `uname -a` → **kernel**-এর তথ্য: kernel name (Linux), hostname, kernel version, build date, architecture (যেমন `x86_64`)।
- `lsb_release -a` → **distribution**-এর তথ্য: Distributor (Ubuntu), Release (24.04), Codename (noble)।
- `hostnamectl` → hostname + OS নাম + kernel + architecture + machine ID — একসাথে গুছিয়ে।

### ২. গভীর ব্যাখ্যা — lab-এর প্রশ্ন (uname vs lsb_release)
মনে আছে topic-এ পড়েছিলাম **kernel ≠ distro**? এখানেই তফাত চোখে দেখবে:
- `uname -a` বলে **ভেতরের ইঞ্জিন** (Linux kernel) সম্পর্কে — version, architecture। এটা সব Linux-এ থাকে।
- `lsb_release -a` বলে **উপরের প্যাকেজ** (distro) সম্পর্কে — এটা Ubuntu নাকি Fedora, কোন version।
- `hostnamectl` দুটোকেই এক জায়গায় এনে machine-এর পুরো পরিচয়পত্র দেয়।

### ৩. উদাহরণ ও পড়া
```
azim@ubuntu:~$ uname -a
Linux ubuntu 6.8.0-31-generic ... x86_64 GNU/Linux   # kernel + architecture
azim@ubuntu:~$ lsb_release -a
Distributor ID: Ubuntu
Release:        24.04
Codename:       noble                                 # distro পরিচয়
azim@ubuntu:~$ hostnamectl
   Static hostname: ubuntu
   Operating System: Ubuntu 24.04 LTS
   Kernel: Linux 6.8.0-31-generic
   Architecture: x86-64                               # সব একসাথে
```

### ৪. আউটকাম
যেকোনো অচেনা machine-এ ঢুকে দ্রুত বলতে পারবে — কোন OS, কোন version, কোন kernel, কোন architecture।

### ৫. কখন ও কেন ব্যবহার করব
Software install-এর আগে compatibility যাচাই, bug report লেখা, server inventory — সব জায়গায় এই পরিচয় লাগে।

### ৬. শর্টকাট টিপস
`uname` = kernel, `lsb_release` = distro/version, `hostnamectl` = সব মিলিয়ে।

**Exam-এ যেভাবে আসতে পারে:** "`uname -a` কী দেখায় আর `lsb_release -a` থেকে কীভাবে আলাদা?" → uname kernel-স্তরের (version/architecture); lsb_release distro-স্তরের (Ubuntu version/codename)।

---

## 8. Super User — `whoami`, `adduser`, `sudo`, root

🎯 **Exam গুরুত্ব: High** 🔴 — concept + কেন সাধারণ user হয়ে থাকা উচিত — খুব common। ~5–7 marks।
**এক কথায়:** **root** Linux-এর সর্বশক্তিমান user (Windows-এর Administrator-এর সমান); দরকার পড়লে এক command-এর জন্য `sudo` দিয়ে সাময়িকভাবে সেই power নাও।

> এতক্ষণ নিজের অধিকারের ভেতরে কাজ করছিলাম; এবার দেখি — কিছু কাজে বাড়তি অনুমতি কেন লাগে।

### ১. ধারণা (Concept)
- `whoami` → তুমি এখন কোন user (নিজের username ফেরত দেয়)।
- **root** → সব করতে পারে এমন superuser। Linux-এর সব নিয়ম root-এর জন্য খাটে না।
- `sudo command` → **s**uper**u**ser **do**; ওই এক command-কে root হিসেবে চালায়, password চায়।

### ২. গভীর ব্যাখ্যা — lab-এর experiment ব্যাখ্যা
- `adduser newname` (sudo ছাড়া) → system বলবে **শুধু root এটা করতে পারে** → permission denied। কারণ নতুন user বানানো system-level কাজ, সাধারণ user-কে দেওয়া হয়নি।
- `sudo whoami` → password দিলে ফেরত আসে **`root`** — প্রমাণ, ওই মুহূর্তে তুমি root।
- `sudo adduser newname` → এখন সফল, কারণ root-power নিয়ে চালালে।

**কেন সবসময় root হয়ে থাকা নয় (lab-এর জোর):** Windows-এ Administrator, Linux-এ root — দুই জায়গাতেই পরামর্শ: **দক্ষতা যাই হোক, সাধারণ user হিসেবেই কাজ করো, দরকার পড়লে তবেই privilege বাড়াও।** কারণ root হলে একটা ভুল command (যেমন ভুল জায়গায় delete) পুরো system নষ্ট করতে পারে — কেউ থামায় না। `sudo` ব্যবহার করলে তুমি **deliberate** হও — "এখন আমি সচেতনভাবে admin হচ্ছি"। এটাই **least privilege** নীতি — cyber security-র ভিত্তি। `sudo`-র কাজ log-ও হয়, তাই accountability থাকে।
চিহ্ন: prompt-এ `$` = সাধারণ user, `#` = root।

### ৩. উদাহরণ
```
azim@ubuntu:~$ whoami
azim
azim@ubuntu:~$ adduser bob
adduser: Only root may add users...      # permission denied
azim@ubuntu:~$ sudo whoami
[sudo] password for azim:
root                                      # এখন root
azim@ubuntu:~$ sudo adduser bob           # এবার সফল
```

### ৪. আউটকাম
কখন privilege লাগে বুঝবে, নিরাপদে `sudo` দেবে, আর "কেন সবসময় root নয়" ব্যাখ্যা করতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
User বানানো, software install (`sudo apt ...`), system file edit (`sudo nano /etc/...`) — সব system-level কাজ। Server/security কাজে রোজকার।

### ৬. শর্টকাট টিপস
root = all-powerful (= Windows Administrator)। **`sudo` = এক command-এর জন্য সাময়িক root।** নীতি: যতটুকু দরকার ততটুকুই power। `$`=user, `#`=root।

**Exam-এ যেভাবে আসতে পারে:** "`sudo`-র কাজ?" → এক command-কে সাময়িকভাবে root হিসেবে চালায়। "কেন সবসময় root/Administrator হিসেবে কাজ করা উচিত নয়?" → ভুলের ক্ষতি বিশাল; least privilege; deliberate elevation নিরাপদ।

---

# Part B — Networking

## 9. `ip a` — network interface ও IP address

🎯 **Exam গুরুত্ব: High** 🔴 — networking unit-এর core; output পড়া আসে। ~4–6 marks।
**এক কথায়:** এই machine-এর network connection (interface) গুলো আর তাদের IP address দেখার command — `ip a` (ip address-এর সংক্ষেপ)।

> System চিনলাম; এবার network — Internet-এ এই machine কীভাবে যুক্ত, তার ঠিকানা কী।

### ১. ধারণা (Concept)
`ip a` (পুরো: `ip addr`) দেখায় machine-এর সব **network interface** (যেমন `lo` = loopback, `eth0`/`enp0s3` = wired, `wlan0` = Wi-Fi) এবং প্রতিটার **IP address**, MAC address, ও state (UP/DOWN)। (পুরোনো `ifconfig`-এর আধুনিক বদলি।)

### ২. গভীর ব্যাখ্যা — output কী বলছে
প্রতিটা interface-এর জন্য দেখবে:
- **interface নাম** (`lo`, `enp0s3`, `wlan0`)।
- `inet 192.168.1.26/24` → এটাই এই machine-এর **IPv4 address**; `/24` = subnet mask (কোন range একই network)।
- `link/ether xx:xx:...` → hardware **MAC address**।
- `lo` (loopback) সবসময় থাকে, address `127.0.0.1` — machine নিজের সাথে কথা বলার interface।
এই lab-এ তোমার দেখা address যেমন `192.168.1.26` — এটা একটা **private** IP (পরের topic-এ আসছে কেন)।

### ৩. উদাহরণ ও পড়া
```
azim@ubuntu:~$ ip a
1: lo: ... inet 127.0.0.1/8 ...            # loopback (নিজের সাথে)
2: enp0s3: ... inet 192.168.1.26/24 ...    # আসল network IP
   link/ether 08:00:27:ab:cd:ef            # MAC address
```

### ৪. আউটকাম
যেকোনো machine-এর IP, interface, MAC দেখে network অবস্থা বুঝতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
"আমার IP কত", "Wi-Fi connected কিনা", "interface UP কিনা" — network troubleshooting-এর প্রথম ধাপ। Cyber/network কাজে রোজ।

### ৬. শর্টকাট টিপস
`ip a` = সব interface + IP + MAC। `lo`/`127.0.0.1` = loopback। `inet` লাইনই IPv4 address।

**Exam-এ যেভাবে আসতে পারে:** "machine-এর IP দেখার command?" → `ip a`। "`127.0.0.1` কী?" → loopback, machine নিজের সাথে কথা বলে।

---

## 10. `ping` — connectivity test

🎯 **Exam গুরুত্ব: High** 🔴 — "কাজ করল/করল না কেন" ব্যাখ্যা আসে। ~4–6 marks।
**এক কথায়:** দুই device-এর মধ্যে connection আছে কিনা যাচাই করার test — একটা ছোট packet পাঠিয়ে ফেরত আসে কিনা দেখে।

> IP জানলাম; এবার দেখি — এই IP-তে আসলেই পৌঁছানো যায় কিনা।

### ১. ধারণা (Concept)
`ping <IP/নাম>` → target-কে ছোট **ICMP echo** packet পাঠায়, target ফেরত পাঠালে বুঝি connection আছে। reply-এর সময় (ms) আর হারানো packet দেখায়। বন্ধ করতে `Ctrl+C`।

### ২. গভীর ব্যাখ্যা — lab-এর প্রশ্ন (কাজ করল/করল না কেন)
- `ping 8.8.8.8` → **8.8.8.8 হলো Google-এর public DNS server।** Internet থাকলে আর ICMP block না হলে reply আসবে → connection আছে।
- `ping 134.115.148.1` (campus router) → শুধু campus network-এ থাকলে পৌঁছাবে; বাইরে থেকে নয়।
- **কাজ না করলে কেন?** কয়েকটা সম্ভাব্য কারণ: internet নেই; target বন্ধ; অথবা **firewall ICMP block করেছে** (অনেক server নিরাপত্তার জন্য ping-এ সাড়া দেয় না — তাই "ping ফেল = device down" সবসময় সত্য নয়)। `ping localhost`/`ping 127.0.0.1` সবসময় কাজ করবে — কারণ সেটা নিজের machine।

### ৩. উদাহরণ ও পড়া
```
azim@ubuntu:~$ ping 8.8.8.8
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms   # reply = connected
^C
azim@ubuntu:~$ ping localhost
64 bytes from 127.0.0.1: ... time=0.03 ms                # নিজের machine, সবসময় ok
```
reply আসছে = পথ খোলা; `time` = কত দ্রুত; কোনো reply নেই/100% loss = সমস্যা।

### ৪. আউটকাম
দুই device-এর মধ্যে connectivity আছে কিনা যাচাই করতে পারবে, আর ping ফেল হলে সম্ভাব্য কারণ ভাবতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
"Internet আছে কি", "server reachable কি", network debugging-এর প্রথম test। Cyber/network কাজে অপরিহার্য।

### ৬. শর্টকাট টিপস
`ping 8.8.8.8` = Google DNS, internet আছে কিনা দ্রুত test। ICMP block থাকলে ping ফেল হতে পারে যদিও device চালু। বন্ধ করতে `Ctrl+C`।

**Exam-এ যেভাবে আসতে পারে:** "8.8.8.8 কী?" → Google-এর public DNS server। "ping কাজ না করার কারণ?" → internet নেই / target down / firewall ICMP block করেছে।

---

## 11. Hosts file — `/etc/hosts`

🎯 **Exam গুরুত্ব: High** 🔴 — এই lab সরাসরি জোর দিয়েছে; name→IP resolution concept। ~4–6 marks।
**এক কথায়:** `/etc/hosts` হলো একটা **লোকাল ফোনবুক** — কোন নাম কোন IP, তা শুধু এই একটা computer-এর মধ্যে ঠিক করে দেয়; DNS-এ যাওয়ার আগে এটাই আগে দেখা হয়।

> ping-এ IP দিলাম; কিন্তু IP মনে রাখা কঠিন। নামকে IP-তে বদলানোর সবচেয়ে সহজ ব্যবস্থা — hosts file।

### ১. ধারণা (Concept)
`/etc/hosts` একটা সাধারণ text file যেখানে লাইন আকারে লেখা থাকে: **IP নাম**। যেমন `127.0.0.1 localhost`। কোনো নাম resolve করার সময় system **আগে এই file দেখে**, পেলে সরাসরি সেই IP নেয় (DNS-এ যায় না)।

### ২. গভীর ব্যাখ্যা — lab-এর experiment
- `less /etc/hosts` → দেখবে `127.0.0.1 localhost` আছে। তাই `ping localhost` মানে আসলে `ping 127.0.0.1`।
- নিজের entry যোগ: `sudo nano /etc/hosts` (system file, তাই `sudo`) → লাইন যোগ করো `8.8.8.8 GoogleEpicDNS`।
- এখন `ping GoogleEpicDNS` → system hosts file-এ নামটা পেয়ে 8.8.8.8-তে ping করবে। **তুমি নিজের নাম বানিয়ে দিলে!**

**মূল শিক্ষা:** এটা নাম→IP resolution, কিন্তু **শুধু এই computer-এ** কার্যকর — অন্য কেউ `GoogleEpicDNS` জানবে না। এই "শুধু লোকাল" সীমাবদ্ধতাই DNS-এর সাথে মূল তফাত (পরের topic)।

### ৩. উদাহরণ
```
azim@ubuntu:~$ less /etc/hosts
127.0.0.1   localhost
azim@ubuntu:~$ sudo nano /etc/hosts      # লাইন যোগ: 8.8.8.8 GoogleEpicDNS
azim@ubuntu:~$ ping GoogleEpicDNS        # 8.8.8.8-তে যাবে
```

### ৪. আউটকাম
নাম যে কীভাবে IP-তে রূপান্তর হয় তার মূল ধারণা পাবে; নিজের machine-এ custom নাম map করতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
Dev/testing-এ একটা domain-কে নিজের machine-এ অন্য IP-তে point করা, server-এ ঘন ঘন ব্যবহৃত host-এর shortcut বানানো — খুব কাজের।

### ৬. শর্টকাট টিপস
`/etc/hosts` = লোকাল ফোনবুক, DNS-এর **আগে** দেখা হয়, শুধু এই machine-এ খাটে। edit করতে `sudo nano`।

**Exam-এ যেভাবে আসতে পারে:** "`/etc/hosts` কী করে?" → নাম→IP লোকাল mapping। "এটা DNS থেকে আলাদা কেন?" → hosts শুধু ওই একটা computer-এ; DNS পুরো Internet জুড়ে।

---

## 12. DNS — `nslookup`, `whois`

🎯 **Exam গুরুত্ব: High** 🔴 — networking unit-এ DNS প্রায় নিশ্চিত আসে। ~5–8 marks।
**এক কথায়:** DNS হলো পুরো Internet-জুড়ে শেয়ার করা **নাম→IP ফোনবুক** (hosts file-এর global সংস্করণ); `nslookup` দিয়ে নাম থেকে IP বের করি, `whois` দিয়ে domain-এর মালিক/contact জানি।

> hosts file শুধু এক machine-এ কাজ করল। কিন্তু পুরো Internet কীভাবে `google.com`-কে IP-তে বদলায়? — সেটাই DNS।

### ১. ধারণা (Concept)
- **DNS** (Domain Name System) = বিশ্বব্যাপী একটা lookup service যা `google.com`-এর মতো নামকে তার IP-তে অনুবাদ করে। যেকোনো computer-এ কাজ করে (hosts file-এর মতো শুধু লোকাল নয়)।
- `nslookup google.com` → নামটার IP address ফেরত দেয়।
- `whois google.com` → domain-টা কে registration করেছে, contact email কী — সেই তথ্য (install: `sudo apt install whois`)।

### ২. গভীর ব্যাখ্যা — hosts vs DNS (lab-এর মূল পয়েন্ট)
আগের hosts file আর DNS একই কাজ করে (নাম→IP), কিন্তু **scope-এ আকাশ-পাতাল তফাত**:
- **hosts file** = শুধু সেই একটা computer-এ; তুমি যা লেখো তাই, কেউ যাচাই করে না।
- **DNS** = পুরো Internet-জুড়ে শেয়ার করা, অসংখ্য server-এর শ্রেণিবদ্ধ (hierarchical) নেটওয়ার্ক; যেকোনো জায়গা থেকে একই উত্তর।

কাজের ক্রম: নাম resolve করতে system **আগে hosts file দেখে, না পেলে DNS-এ জিজ্ঞেস করে।** `nslookup`-এর দেওয়া IP browser-এ বসালে একই site খুলবে — প্রমাণ, domain name আসলে IP-র সুন্দর মুখোশ মাত্র। `whois` কাজে লাগে domain নিয়ে অভিযোগ/যোগাযোগ করতে — contact email পাওয়া যায়।

### ৩. উদাহরণ
```
azim@ubuntu:~$ nslookup google.com
Name:   google.com
Address: 142.250.xxx.xxx          # এই IP browser-এ বসালেও google খুলবে
azim@ubuntu:~$ sudo apt install whois
azim@ubuntu:~$ whois google.com   # registration ও contact তথ্য
```

### ৪. আউটকাম
নাম যে কীভাবে IP হয়ে site লোড করে — পুরো চেইন বুঝবে; DNS ও hosts-এর তফাত পরিষ্কার হবে।

### ৫. কখন ও কেন ব্যবহার করব
Site কেন খুলছে না (DNS সমস্যা?) debug, server-এর IP বের করা, domain-এর মালিক খোঁজা — network/security/forensics-এ রোজকার।

### ৬. শর্টকাট টিপস
DNS = global নাম→IP ফোনবুক; hosts = লোকাল ফোনবুক। `nslookup নাম` = IP দাও; `whois নাম` = মালিক/contact দাও। ক্রম: hosts আগে → তারপর DNS।

**Exam-এ যেভাবে আসতে পারে:** "DNS কী?" → নামকে IP-তে অনুবাদ করা distributed Internet service। "DNS আর /etc/hosts-এর পার্থক্য?" → DNS পুরো Internet-জুড়ে শেয়ার্ড, hosts শুধু লোকাল। "নাম থেকে IP বের করার command?" → `nslookup`।

---

## 13. Public vs Private IP address

🎯 **Exam গুরুত্ব: High** 🔴 — "দুই জায়গায় আলাদা IP কেন" — প্রিয় প্রশ্ন (NAT)। ~4–6 marks।
**এক কথায়:** **Private IP** তোমার লোকাল network-এর ভেতরের ঠিকানা (বাইরে অচল); **Public IP** পুরো Internet-এ তোমার router-কে চেনায় এমন ঠিকানা — তাই `ip a` আর whatismyipaddress আলাদা দেখায়।

> DNS-এ IP দেখলাম; কিন্তু lab-এ লক্ষ্য করলাম আমার machine-এর IP (`192.168.1.26`) আর website-এ দেখানো IP আলাদা — কেন? এটাই এই topic।

### ১. ধারণা (Concept)
- **Private IP:** নির্দিষ্ট সংরক্ষিত range — `192.168.x.x`, `10.x.x.x`, `172.16–31.x.x`। শুধু লোকাল network-এর (বাড়ি/অফিস) ভেতরে কাজ করে, Internet-এ routable নয়।
- **Public IP:** ISP-র দেওয়া, পুরো Internet-এ unique — তোমার network বাইরের জগতে এই ঠিকানায় পরিচিত।

### ২. গভীর ব্যাখ্যা — lab-এর প্রশ্ন (আলাদা IP কেন)
`ip a`-তে দেখলে তোমার machine-এর IP হয়তো `192.168.1.26` — একটা **private** IP। কিন্তু whatismyipaddress.com একটা **আলাদা** (public) IP দেখায়। কারণ:
- তোমার বাড়ির সব device একই private network-এ আলাদা private IP নিয়ে বসে।
- Internet-এ বের হওয়ার সময় router এক কৌশল ব্যবহার করে — **NAT (Network Address Translation):** সব private IP-কে router-এর একটা **public IP**-তে অনুবাদ করে দেয়।
- তাই বাইরের কোনো site তোমার machine-এর private IP দেখে না, দেখে router-এর public IP — সেটাই whatismyipaddress দেখাচ্ছে।

**কেন এই ব্যবস্থা?** IPv4 address সীমিত; private + NAT দিয়ে একটা public IP-র পেছনে শত device চালানো যায় (address বাঁচে), আর private device সরাসরি Internet থেকে লুকিয়ে থাকে (নিরাপত্তা bonus)।

### ৩. উদাহরণ
```
azim@ubuntu:~$ ip a        # দেখায় 192.168.1.26  ← private (লোকাল)
# whatismyipaddress.com    → দেখায় 203.x.x.x      ← public (router-এর, NAT-এর পর)
```
এক বাড়ির ভাবো: **private IP = প্রতিটা ঘরের room number** (শুধু বাড়ির ভেতরে অর্থবহ), **public IP = পুরো বাড়ির street address** (বাইরের ডাকপিয়ন এটাই জানে)। NAT = receptionist যে ভেতরের room আর বাইরের address মেলায়।

### ৪. আউটকাম
নিজের network-এর IP কাঠামো বুঝবে; "ভেতরের আর বাইরের IP আলাদা কেন" ব্যাখ্যা করতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
Network design, port forwarding, VPN, firewall নিয়ম — সব এই private/public ভাগের উপর দাঁড়ায়। Cyber/cloud-এ একদম মৌলিক।

### ৬. শর্টকাট টিপস
private = `192.168.x` / `10.x` / `172.16–31.x`, লোকাল-only। public = Internet-এ unique। **আলাদা দেখার কারণ = NAT।**

**Exam-এ যেভাবে আসতে পারে:** "`ip a` আর whatismyipaddress আলাদা IP দেখায় কেন?" → প্রথমটা private (লোকাল), দ্বিতীয়টা router-এর public IP; NAT private→public অনুবাদ করে। "Private IP range-গুলো লেখো" → 192.168.x.x, 10.x.x.x, 172.16–31.x.x।

---

# Part C — Hardware, Output ও Software Installation

## 14. Hardware — `lsusb`, `lspci`, `/proc/cpuinfo`

🎯 **Exam গুরুত্ব: Low** — কোন command কী দেখায় + core গোনা। ~2–3 marks।
**এক কথায়:** machine-এ কোন hardware লাগানো তা দেখার command — USB device (`lsusb`), PCI device যেমন graphics/network card (`lspci`), আর CPU-র বিস্তারিত (`/proc/cpuinfo`)।

> Network চিনলাম; এবার machine-এর শরীর — ভেতরে কী কী hardware আছে।

### ১. ধারণা (Concept)
- `lsusb` → **l**i**s**t **USB** — লাগানো সব USB device (mouse, keyboard, pendrive)।
- `lspci` → **l**i**s**t **PCI** — motherboard-এর PCI device (graphics card, network/sound card)।
- `less /proc/cpuinfo` → CPU-র বিস্তারিত: model, speed, আর প্রতিটা **core** আলাদা entry হিসেবে।

### ২. গভীর ব্যাখ্যা — core গোনা (lab-এর প্রশ্ন)
`/proc` একটা **virtual** directory — disk-এ আসল file নয়, kernel real-time তথ্যকে file-এর রূপে দেখায় ("সবকিছুই file" নীতি)। `/proc/cpuinfo`-তে প্রতিটা CPU core-এর জন্য একটা করে `processor` block থাকে। **কতগুলো `processor :` লাইন আছে গুনলেই = কতগুলো (logical) core।** দ্রুত গুনতে: `grep -c processor /proc/cpuinfo`।
GUI-তে **Settings → About this Computer**-ও একই তথ্য সংক্ষেপে দেখায় — কোনটা বেশি কাজের, সেটা use-case-এর উপর: দ্রুত এক নজর = GUI; script/automation/remote = CLI।

### ৩. উদাহরণ
```
azim@ubuntu:~$ lsusb                         # USB device তালিকা
azim@ubuntu:~$ lspci                          # PCI device (GPU/NIC ইত্যাদি)
azim@ubuntu:~$ grep -c processor /proc/cpuinfo
4                                             # 4 cores
```

### ৪. আউটকাম
CLI থেকে machine-এর hardware inventory ও core সংখ্যা বের করতে পারবে।

### ৫. কখন ও কেন ব্যবহার করব
Driver সমস্যা debug, server spec যাচাই (কত core/কোন NIC), GUI ছাড়া hardware চেনা — sysadmin কাজে।

### ৬. শর্টকাট টিপস
`lsusb`=USB, `lspci`=PCI(GPU/NIC), `/proc/cpuinfo`=CPU+cores। core গোনা = `grep -c processor /proc/cpuinfo`।

---

## 15. Output redirection — `>`

🎯 **Exam গুরুত্ব: Medium** — `>` কী করে জিজ্ঞেস করে। ~3–4 marks।
**এক কথায়:** কোনো command-এর output screen-এ না দেখিয়ে সরাসরি একটা **file-এ পাঠানো** — `>` চিহ্ন দিয়ে।

> এতক্ষণ command-এর output screen-এ দেখছিলাম; এবার সেই output ধরে file-এ জমিয়ে রাখি।

### ১. ধারণা (Concept)
- `command > file` → output screen-এ না গিয়ে `file`-এ লেখা হয় (file না থাকলে বানায়, থাকলে **মুছে নতুন করে লেখে**)।
- `command >> file` → একই, কিন্তু **শেষে যোগ** করে (append, আগেরটা মোছে না)।

### ২. গভীর ব্যাখ্যা — lab-এর experiment
`lsusb > output_of_lsusb` → `lsusb`-এর output screen-এ না দেখিয়ে `output_of_lsusb` নামের file-এ ঢেলে দিল। এখন:
- `cat output_of_lsusb` / `less output_of_lsusb` → সেই জমানো output পড়ো (cat পুরোটা, less পাতা-পাতা — মনে আছে topic 5)।
- `ls -la output_of_lsusb` → file-টা কত byte (size) দেখায়।
- `rm output_of_lsusb` → মুছে ফেলো।

এর ভিত্তি: প্রতিটা program-এর output যায় **stdout** (standard output)-এ, যা সাধারণত screen। `>` সেই stream-কে redirect করে file-এ। এটাই Linux-এর শক্তি — command-এর ফল ধরে রাখা, পরে বিশ্লেষণ, বা log বানানো।

### ৩. উদাহরণ ও dry run
```
azim@ubuntu:~$ lsusb > output_of_lsusb     # screen-এ কিছু এল না — file-এ গেল
azim@ubuntu:~$ cat output_of_lsusb         # এখন পড়লাম
azim@ubuntu:~$ ls -la output_of_lsusb      # size দেখলাম
azim@ubuntu:~$ rm output_of_lsusb          # মুছলাম
```
❗মনে রাখো: `>` পুরোনো content **মুছে** দেয়; রাখতে চাইলে `>>` (append)।

### ৪. আউটকাম
যেকোনো command-এর ফল file-এ সংরক্ষণ করতে পারবে — log, report, পরের ধাপের input হিসেবে।

### ৫. কখন ও কেন ব্যবহার করব
Command-এর output save করে রাখা (audit/log), report বানানো, একটা command-এর ফল আরেকটায় খাওয়ানো (automation/scripting) — সর্বত্র।

### ৬. শর্টকাট টিপস
`>` = file-এ লেখা (overwrite), `>>` = শেষে যোগ (append)। output screen-এ না দেখলে সেটা file-এ গেছে।

**Exam-এ যেভাবে আসতে পারে:** "`lsusb > file` কী করে?" → lsusb-এর output screen-এ না দেখিয়ে file-এ পাঠায়। "`>` আর `>>`-এর তফাত?" → > overwrite, >> append।

---

## 16. Software install — Software as a Service (SaaS)

🎯 **Exam গুরুত্ব: Low** — concept/তুলনা। ~2–3 marks।
**এক কথায়:** install-ই লাগে না — browser-এ login করেই ব্যবহার (Office 365, Google Docs, Grammarly); OS যাই হোক একই।

> এবার software install-এর নানা পথ। সবচেয়ে সহজ দিয়ে শুরু — যেখানে install-ই লাগে না।

### ১–২. ধারণা ও ব্যাখ্যা
**SaaS** (Software as a Service): software কোনো remote server-এ চলে, তুমি শুধু **browser দিয়ে access** করো। কিছু download/install নেই; Windows/macOS/Linux — কোনো তফাত নেই, শুধু account + supported browser দরকার। উদাহরণ: Office 365, Google Drive/Docs, Grammarly। গত এক দশকে এটাই প্রধান model হয়ে উঠেছে।

### ৩. বাস্তব দিক — pros/cons
- **সুবিধা:** install ঝামেলা নেই, যেকোনো device-এ, সবসময় up-to-date, OS-নিরপেক্ষ।
- **অসুবিধা:** internet ছাড়া অচল; data কোম্পানির server-এ (privacy/control কম); subscription খরচ।

### ৪–৫. আউটকাম ও ব্যবহার
বুঝবে সব software install করতে হয় না; team collaboration, cross-device কাজে SaaS সেরা।

### ৬. শর্টকাট টিপস
SaaS = browser-এ চলে, install নেই, OS-নিরপেক্ষ, internet লাগে।

---

## 17. Software install — web থেকে binary (`.deb` / `.rpm`)

🎯 **Exam গুরুত্ব: Medium** — package format তুলনা। ~3–4 marks।
**এক কথায়:** Windows-এর `.exe`-র মতো, Linux-এ website থেকে নামিয়ে install করা যায় — Ubuntu-তে `.deb`, Red Hat-এ `.rpm`।

> SaaS-এ install লাগেনি; এবার পরিচিত পথ — web থেকে ফাইল নামিয়ে install।

### ১–২. ধারণা ও ব্যাখ্যা
এটা সেই চেনা কাজ, নতুন নামে: Windows-এ `.exe`, macOS-এ `.dmg` — Linux-এ **`.deb`** (Debian/Ubuntu) বা **`.rpm`** (Red Hat/Fedora)। এগুলো **binary package** — আগে থেকে compile করা প্রোগ্রাম। যেমন Google Chrome-এর site-এ গিয়ে `.deb` নামিয়ে install করলে Ubuntu-তে Chrome বসে যায় (বা Opera ট্রাই করতে পারো)।
মূল ধারণা: একই software বিভিন্ন distro-র জন্য বিভিন্ন format-এ প্যাকেজ হয় — তাই Ubuntu-তে `.deb` নাও, Fedora-তে `.rpm`।

### ৩. বাস্তব দিক — pros/cons
- **সুবিধা:** সরাসরি vendor থেকে নতুন version, পরিচিত পদ্ধতি।
- **অসুবিধা:** নিজে update করতে হয় (auto নয়), source যাচাই না করলে নিরাপত্তা ঝুঁকি, dependency নিজে সামলাতে হতে পারে।

### ৪–৫. আউটকাম ও ব্যবহার
যে software repository-তে নেই বা vendor-এর নতুন version লাগবে — তখন web binary।

### ৬. শর্টকাট টিপস
Ubuntu = `.deb`, Red Hat = `.rpm` (≈ Windows `.exe`)। vendor থেকে নতুন, কিন্তু update/security নিজের দায়িত্ব।

---

## 18. Software install — repository থেকে (`apt`)

🎯 **Exam গুরুত্ব: High** 🔴 — `apt` command ও sources.list/update প্রায় নিশ্চিত। ~5–8 marks।
**এক কথায়:** Ubuntu-র যাচাই-করা online ভাণ্ডার (repository) থেকে এক command-এ software install/update — `apt`; App Store/Play Store-এর CLI সংস্করণ।

> Web binary-তে নিজেকে সব সামলাতে হলো; এবার সবচেয়ে নিরাপদ ও সহজ পথ — distro-র নিজস্ব repository।

### ১. ধারণা (Concept)
- **Repository (repo):** Ubuntu-র maintainer-দের যাচাই করা একটা online software ভাণ্ডার।
- **`apt`** (Advanced Package Tool): সেখান থেকে package খুঁজে, নামিয়ে, dependency সহ install/update/remove করার tool।
- ধারণাটা পুরোনো নয় — App Store/Play Store যা করে (যাচাই করা জায়গা থেকে binary), Linux distro দশক ধরে তা-ই করছে।

### ২. গভীর ব্যাখ্যা — lab-এর command গুলো
- `less /etc/apt/sources.list` → এই file-এ লেখা **কোন কোন repository থেকে software আসবে** তার তালিকা (URL সহ)। `apt` এখান থেকেই জানে কোথায় খুঁজবে।
- `sudo apt update` → repo থেকে **package তালিকা refresh** করে (কোনটার নতুন version আছে জানে)। ⚠️ এটা software আপগ্রেড করে না, শুধু তালিকা নতুন করে।
- `sudo apt upgrade` → install থাকা সব software-কে নতুন version-এ আপগ্রেড করে — **Linux-এ এভাবেই OS/security update হয়।**
- `sudo apt search keyword` → repo-তে package খোঁজে (যেমন `vlc`)।
- `sudo apt install নাম` → install করে, **dependency নিজে নামিয়ে নেয়** (এটাই repo-র বড় সুবিধা)।
সব `sudo` কারণ system-level। **মূল ক্রম: আগে `update`, তারপর `install`/`upgrade`** (নইলে পুরোনো তালিকা থেকে পুরোনো version আসতে পারে)। Server-এ GUI নেই, তাই এই CLI পদ্ধতিই আসল।

### ৩. উদাহরণ
```
azim@ubuntu:~$ less /etc/apt/sources.list   # কোন repo থেকে আসবে
azim@ubuntu:~$ sudo apt update              # তালিকা refresh
azim@ubuntu:~$ sudo apt search vlc          # খোঁজা
azim@ubuntu:~$ sudo apt install vlc         # install (dependency সহ)
azim@ubuntu:~$ sudo apt upgrade             # সব software আপগ্রেড
```

### ৩(খ). বাস্তব দিক — pros/cons
- **সুবিধা:** যাচাই করা (নিরাপদ), dependency auto, এক জায়গা থেকে সব update, GUI ছাড়াই চলে।
- **অসুবিধা:** repo-তে যা আছে শুধু তাই; খুব নতুন version কখনো দেরিতে আসে।

### ৪. আউটকাম
CLI থেকে নিরাপদে software খোঁজা, install, update, system patch করতে পারবে — server-ready skill।

### ৫. কখন ও কেন ব্যবহার করব
নতুন server সাজানো, নিয়মিত security update (`sudo apt update && sudo apt upgrade`), tool install — DevOps/security-র রোজকার কাজ। **এটাই Linux-এ software install-এর প্রধান ও পছন্দের পথ।**

### ৬. শর্টকাট টিপস
ক্রম: **update → install/upgrade**। `sources.list` = repo তালিকা। search/install/upgrade — সব `sudo apt`। dependency apt নিজেই সামলায়।

**Exam-এ যেভাবে আসতে পারে:** "`sudo apt update` কী করে?" → repo থেকে package তালিকা refresh (software আপগ্রেড নয়)। "CLI-তে vlc install?" → `sudo apt install vlc`। "`/etc/apt/sources.list`-এ কী থাকে?" → কোন repository থেকে package আসবে তার তালিকা।

---

## 19. Software install — source code থেকে (`gcc`, `chmod 777`)

🎯 **Exam গুরুত্ব: Medium** — compile করার ধাপ + source vs machine code। ~4–5 marks।
**এক কথায়:** ready binary না নিয়ে নিজে **source code compile** করে software বানানো — `gcc` দিয়ে; দরকার pre-release/পুরোনো version/niche software বা code নিজে যাচাই করতে।

> এতক্ষণ ready-made software install করছিলাম। শেষ পথ — কাঁচা source code থেকে নিজে বানানো (সবচেয়ে hands-on)।

### ১. ধারণা (Concept)
- **Source code** = মানুষের পড়ার মতো প্রোগ্রাম (যেমন C-তে লেখা `.c` file)।
- **Compile** = সেই source-কে CPU-র বোঝার মতো **machine code** (executable binary)-এ অনুবাদ করা।
- `gcc` = C compiler; `build-essential` = compile-এর সব tool (gcc, make, library) একসাথে।

### ২. গভীর ব্যাখ্যা — lab-এর ধাপ
কখন source থেকে install করি: pre-release ট্রাই, পুরোনো version দরকার, niche software (কেউ binary বানায়নি), অন্য hardware-এর জন্য (যেমন ARM chip), বা code নিজে পড়ে দেখা (security audit)। ধাপ:
1. tool আনো: `sudo apt install build-essential`।
2. C code লেখো, save করো `hello_world.c` নামে।
3. compile: `gcc hello_world.c -o hello_world_executable` → binary তৈরি।
4. চালাও: `./hello_world_executable` (`./` মানে "এই folder-এর ভেতরের এই file চালাও")।
5. **Permission error হলে:** `chmod 777 hello_world_executable` দিয়ে চালানোযোগ্য (executable) করো।

**source vs machine code (lab-এর প্রশ্ন):** `less hello_world.c` দিলে পরিষ্কার C কোড পড়বে — মানুষের ভাষা। `less hello_world_executable` দিলে দেখবে দুর্বোধ্য প্রতীক/গার্বেজ — কারণ এটা **compiled binary**, CPU-র জন্য, মানুষের পড়ার জন্য নয়। তাই দুটো আলাদা: একটা আমরা লিখি/বুঝি, আরেকটা machine চালায়।

⚠️ **`chmod 777` নিয়ে একটা সতর্কতা:** 777 মানে owner+group+others **সবাইকে rwx (সব অনুমতি)** দেওয়া — lab-এ সহজ করার জন্য, কিন্তু বাস্তবে এটা নিরাপত্তা-ঝুঁকি (যে কেউ বদলাতে/চালাতে পারে)। ভালো অভ্যাস `chmod +x file` বা `chmod 755 file` (owner সব, বাকিরা পড়া+চালানো)। (permission topic পরের সপ্তাহে বিস্তারিত আসবে — মূল নিয়ম: `r=4, w=2, x=1`।)

### ৩. উদাহরণ ও dry run
```
azim@ubuntu:~$ sudo apt install build-essential
azim@ubuntu:~$ nano hello_world.c            # উপরের C কোড লিখে save
azim@ubuntu:~$ gcc hello_world.c -o hello_world_executable
azim@ubuntu:~$ ./hello_world_executable
Hello, World!
# Permission denied হলে:
azim@ubuntu:~$ chmod 777 hello_world_executable
azim@ubuntu:~$ ./hello_world_executable
Hello, World!
```
❗Common error: `./` ভুলে শুধু `hello_world_executable` টাইপ করা → "command not found" (current folder PATH-এ নেই বলে `./` লাগে)।

### ৪. আউটকাম
Source থেকে C প্রোগ্রাম compile ও চালাতে পারবে; source ও machine code-এর তফাত বুঝবে।

### ৫. কখন ও কেন ব্যবহার করব
Pre-release/niche/পুরোনো software, ভিন্ন hardware (ARM), বা code audit — যখন ready binary নেই। Developer ও security researcher-দের জরুরি skill।

### ৬. শর্টকাট টিপস
ক্রম: `build-essential` → `gcc x.c -o exe` → `./exe`। permission আটকালে `chmod +x` (777 নয়, ঝুঁকি)। source = মানুষের, binary = machine-এর। চালাতে `./` লাগে।

**Exam-এ যেভাবে আসতে পারে:** "C প্রোগ্রাম compile করার command?" → `gcc file.c -o output`। "source code আর machine code আলাদা কেন?" → source মানুষ-পড়ার মতো, compiled binary CPU-র জন্য, পড়া যায় না। "`./` কেন লাগে?" → current folder PATH-এ নেই, তাই path স্পষ্ট করতে।

---

# সারাংশ (Full Summary)

1. **GUI Familiarisation:** Firefox/LibreOffice/file manager/Software Centre চেনা; server-এ GUI নেই বলে CLI must।
2. **ps/top:** `ps -e` সব process snapshot; `top` live, `1`=per-core view, `q`=exit।
3. **ls flags:** `-l` বিস্তারিত, `-a` hidden, `-h` human size, `-t` time-sort (`-alt`=নতুন আগে)।
4. **touch/gedit/nano:** touch খালি file; gedit GUI (remote-এ চলবে না); nano terminal-only (save `Ctrl+O`, exit `Ctrl+X`)।
5. **cat vs less:** cat পুরোটা একসাথে; less পাতা-পাতা (বড় file)।
6. **cp vs mv:** cp নকল (original থাকে); mv move/rename (original যায়)।
7. **uname/lsb_release/hostnamectl:** kernel / distro / সব-মিলিয়ে পরিচয়।
8. **Super User:** root=সর্বশক্তিমান; `sudo`=সাময়িক root; least privilege নীতিতে সাধারণ user-এ থাকো।
9. **ip a:** interface+IP+MAC; `127.0.0.1`=loopback; দেখা IP private।
10. **ping:** ICMP connectivity test; `8.8.8.8`=Google DNS; ফেল=no net/down/ICMP block।
11. **/etc/hosts:** লোকাল নাম→IP ফোনবুক, DNS-এর আগে দেখা হয়, শুধু এই machine-এ।
12. **DNS:** global নাম→IP service; `nslookup`=IP দাও, `whois`=মালিক/contact; hosts লোকাল, DNS সর্বত্র।
13. **Public vs Private IP:** private লোকাল (192.168/10/172.16-31), public Internet-এ unique; আলাদা দেখার কারণ NAT।
14. **Hardware:** `lsusb`/`lspci`/`/proc/cpuinfo`; core গোনা `grep -c processor /proc/cpuinfo`।
15. **Redirection:** `>` output file-এ (overwrite), `>>` append।
16. **SaaS:** browser-এ চলে, install নেই, OS-নিরপেক্ষ, internet লাগে।
17. **Web binary:** `.deb`(Ubuntu)/`.rpm`(Red Hat) ≈ `.exe`; update/security নিজের দায়।
18. **apt repo:** যাচাই করা ভাণ্ডার; `update`(তালিকা)→`install`/`upgrade`(software); dependency auto; প্রধান পথ।
19. **From source:** `build-essential`→`gcc x.c -o exe`→`./exe`; permission আটকালে `chmod +x`; source=মানুষের, binary=machine-এর।

---

# বাস্তব ব্যবহার টেবিল (Lab → Real Project)

| Topic | Lab-এ যা শিখলাম | Real Project / Career-এ কোথায় লাগে |
|---|---|---|
| GUI | desktop navigate | dev workstation |
| ps/top | process monitor | server slow হলে দোষী খোঁজা |
| ls flags | file inspect | permission/time audit, debugging |
| touch/nano | file বানানো/edit | server config (`sudo nano /etc/...`) |
| cat/less | file পড়া | config ও log পড়া |
| cp/mv | file নাড়া | backup, project গোছানো |
| uname/lsb_release | system পরিচয় | compatibility যাচাই, bug report |
| sudo/root | privilege | নিরাপদ admin, security |
| ip a | network info | IP/interface troubleshooting |
| ping | connectivity | network/server reachability test |
| /etc/hosts | লোকাল resolution | dev/testing-এ domain map |
| DNS/nslookup | নাম→IP | site/DNS issue debug, recon |
| public/private IP | NAT বোঝা | firewall, VPN, port forwarding, cloud network |
| lsusb/lspci/cpuinfo | hardware inventory | server spec, driver debug |
| redirection `>` | output save | log, report, scripting |
| SaaS/binary/apt/source | software ব্যবস্থাপনা | server provisioning, patching, custom build |

---

# রিফ্লেকশন (Reflection — assignment style)

এই lab-এ আমি Ubuntu-কে GUI থেকে শুরু করে গভীর command line পর্যন্ত হাতে-কলমে চিনেছি, আর বুঝেছি কেন server জগতে CLI-ই আসল দক্ষতা — কারণ বেশিরভাগ server-এ কোনো graphical interface থাকে না। আমি দেখেছি একটা নাম কীভাবে IP-তে রূপান্তর হয়: প্রথমে লোকাল `/etc/hosts`, না পেলে global DNS — আর `ip a` বনাম whatismyipaddress-এর আলাদা ঠিকানা থেকে NAT এবং public/private IP-র ধারণা স্পষ্ট হয়েছে, যা পুরো Internet routing বোঝার ভিত্তি। সবচেয়ে গুরুত্বপূর্ণ ছিল privilege ও নিরাপত্তা: `sudo`-র মাধ্যমে least-privilege নীতি — সাধারণ user হিসেবে কাজ করে দরকারে সচেতনভাবে root হওয়া — যা cyber security-র মূল দর্শন। software install-এর চারটি পথ (SaaS, web binary, repository, source) তুলনা করে বুঝেছি repository (`apt`) কেন সবচেয়ে নিরাপদ ও আধুনিক, আর source compile কেন এখনো প্রয়োজন। যেহেতু আমার লক্ষ্য cloud ও security engineering, এই networking + privilege + package management foundation ছাড়া উপরের কোনো স্তর দাঁড়াবে না — তাই এই lab আমার career-পথের প্রথম, অপরিহার্য ধাপ। সামনে এই command গুলোই scripting ও automation-এ রূপ নেবে এবং আমি একই দক্ষতা remote server-এ SSH দিয়ে প্রয়োগ করব।

---

# দ্রুত রিভিশন + সম্ভাব্য প্রশ্ন (শেষ ৩০ মিনিট)

### 🔴 সবচেয়ে বেশি নম্বরের জায়গা (এগুলো ছাড়লে চলবে না)
- **Networking চতুষ্টয়:** `ip a` (IP দেখা), `ping`/8.8.8.8 (connectivity), `/etc/hosts` vs **DNS** (লোকাল vs global resolution), public vs private IP + **NAT** — এই unit-এ এগুলোই সবচেয়ে বেশি আসে।
- **sudo/root + least privilege** — কেন সবসময় root নয়, short answer।
- **apt:** `update` vs `upgrade`, `install`, `sources.list` — command-write।
- **cp vs mv**, **ls flags** (`-la`, `-alt`), **redirection `>`** — command-write/output।
- **gedit vs nano** (remote-এ কোনটা চলবে), **source vs machine code**।

### সম্ভাব্য প্রশ্ন + এক-লাইন উত্তর সূত্র
1. *top-এ `1` চাপলে?* → প্রতি CPU core আলাদা দেখায়।
2. *`ls` vs `ls -la`?* → -la hidden সহ বিস্তারিত। *`ls -alt`?* → time-sort, নতুন আগে।
3. *gedit vs nano; remote terminal-এ কোনটা?* → nano (text-based); gedit GUI লাগে।
4. *cat vs less?* → cat পুরোটা; less পাতা-পাতা।
5. *cp vs mv?* → cp নকল রাখে; mv সরায়/নাম বদলায়।
6. *uname -a vs lsb_release -a?* → kernel-স্তর vs distro-স্তর।
7. *sudo-র কাজ; কেন সবসময় root নয়?* → সাময়িক root; ভুলের ক্ষতি বড়, least privilege।
8. *8.8.8.8 কী; ping ফেল কেন হতে পারে?* → Google DNS; no net/down/firewall ICMP block।
9. */etc/hosts কী; DNS থেকে আলাদা কেন?* → লোকাল নাম→IP; DNS global শেয়ার্ড।
10. *নাম→IP বের করার command?* → `nslookup`। *domain মালিক?* → `whois`।
11. *ip a আর website আলাদা IP দেখায় কেন?* → private vs public; NAT অনুবাদ করে।
12. *`sudo apt update` কী করে?* → package তালিকা refresh (software আপগ্রেড নয়)।
13. *CLI-তে software install?* → `sudo apt install <name>`।
14. *`lsusb > file` কী করে?* → output file-এ পাঠায় (`>`=overwrite, `>>`=append)।
15. *C compile?* → `gcc x.c -o exe`, চালাতে `./exe`। *source vs machine code?* → মানুষের vs CPU-র।
16. */proc/cpuinfo-তে core গোনা?* → `processor` লাইন গোনা / `grep -c processor /proc/cpuinfo`।

> 💡 শেষ টিপ: command-write প্রশ্নে syntax পরিষ্কার লেখো; "কেন/পার্থক্য" প্রশ্নে এক লাইনে মূল কারণ (least privilege, NAT, লোকাল vs global, dependency) দাও। এই lab-এ **networking (DNS/IP/NAT)** আর **privilege + apt** — এই দুই ব্লক থেকেই বেশি নম্বর আসে, তাই এগুলো সবচেয়ে ভালো করে গেঁথে নাও।
