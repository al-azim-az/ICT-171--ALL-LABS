# ICT171 — Lab 3 Exam Note
### Linux Permissions + Filesystem Searching (Exam-Ready)

> এই note টা exam-night revision-এর জন্য বানানো। প্রতিটা topic-এ **Exam গুরুত্ব** আর **এক কথায়** দেওয়া আছে — যাতে কোথায় time দিবে আগেই বুঝে নাও। Question bank-এর প্রশ্ন + slide থেকে সম্ভাব্য প্রশ্ন — দুটোই cover করা।

---

## 📋 Topic List (priority map)

| # | Topic | Exam গুরুত্ব |
|---|---|---|
| 1 | Users (`adduser`, `/etc/passwd`) | 🟡 Medium |
| 2 | Groups (`groupadd`, group membership) | 🟡 Medium |
| 3 | **Reading permissions — `ls -l` string** | 🔴 **High** |
| 4 | **`chmod` + permission number math (700/666/777)** | 🔴 **High — সবচেয়ে দামি** |
| 5 | `chown` / `chgrp` (ownership বদলানো) | 🟡 Medium |
| 6 | `su` / `whoami` (user switch + verify) | 🟡 Medium |
| 7 | sudoers group (challenge) | 🟢 Low |
| 8 | File ops — `mkdir` / `touch` / `rm` | 🟢 Low |
| 9 | Archive extract — `bunzip2` / `tar` | 🟡 Medium |
| 10 | `find -name` (filename search) | 🟡 Medium |
| 11 | `grep -r` / text search | 🟡 Medium |
| 12 | Advanced find — size, dates, `du` | 🟢 Low |
| 13 | **Linux ↔ Windows command map** | 🔴 **High — easy marks** |

---
---

## 1. Users — সিস্টেমে আলাদা আলাদা account

🎯 **Exam গুরুত্ব: Medium** — সরাসরি MCQ কম, কিন্তু permissions বোঝার ভিত্তি। open-answer/scenario প্রশ্নে লাগে।
**এক কথায়:** প্রতিটা user হলো একটা আলাদা পরিচয় (identity), আর Linux প্রতিটা কাজে দেখে "কে করছে"।

### ১. ধারণা (Concept)
- **user কী** — সিস্টেমের একটা account; এর নিজস্ব নাম, home folder, আর permission আছে।
- **কেন দরকার** — একই machine অনেকজন use করলে কে কী করতে পারবে সেটা আলাদা রাখতে হয় (security)।

### ২. গভীর ব্যাখ্যা
- **প্রতিটা user-এর একটা UID আছে** — UID = User ID, একটা সংখ্যা যেটা দিয়ে Linux ভেতরে user-কে চেনে (নাম শুধু মানুষের পড়ার জন্য)।
- **root হলো super-user** — UID 0, সব করতে পারে, কোনো permission আটকায় না।
- **`/etc/passwd`** — একটা text file যেখানে প্রতিটা user-এর তথ্য এক লাইনে থাকে (নাম, UID, home, shell)। নাম `passwd` হলেও আসল password এখানে থাকে না।
- **`less /etc/passwd`** — `less` দিয়ে file টা scroll করে দেখা যায় (q চাপলে বের হও)।

### ৩. command + dry run
```
sudo adduser alice
```
- **`sudo`** — "superuser do"; command টা root-এর ক্ষমতা দিয়ে চালায় (user বানানো admin কাজ, তাই sudo লাগে)।
- **`adduser alice`** — alice নামে নতুন user বানায়, home folder + password prompt সহ।
- **মুছতে** — `sudo deluser alice`।

### ৪. Outcome
- এখন আলাদা আলাদা পরিচয় বানিয়ে প্রতিজনকে আলাদা access দিতে পারবে।

### ৬. শর্টকাট টিপস
- **user = "কে"** · group = "কোন দল" · permission = "কী করতে পারবে"।

---

## 2. Groups — user-দের দল

🎯 **Exam গুরুত্ব: Medium** — Lab 3-এর পুরো সমাধান group-এর উপর দাঁড়ানো। scenario প্রশ্নে আসে।
**এক কথায়:** একই access একসাথে অনেক user-কে দিতে হলে তাদের এক group-এ রাখো, তারপর group-কে permission দাও।

> **আগের সাথে link:** user বানানো শিখলাম (#1), এখন সেই user-দের দল বানিয়ে একসাথে manage করব।

### ১. ধারণা
- **group কী** — কয়েকজন user-এর একটা নামকরা set; permission এক জায়গায় দিলে পুরো দল পেয়ে যায়।
- **কেন দরকার** — ১০জনকে আলাদা আলাদা permission না দিয়ে, একটা group-কে একবার permission দিলেই হয়।

### ২. গভীর ব্যাখ্যা
- **`/etc/group`** — কোন group-এ কোন user আছে তার তালিকা রাখা file।
- **প্রতিটা file-এর একটা owner আর একটা group থাকে** — তাই permission তিন ভাগে দেওয়া যায়: owner / group / others (এটাই #3-এর মূল কথা)।
- **Lab 3-এর logic** — Alice, Bob-কে একটা group-এ রেখে folder-টার group-permission ঠিক করলেই দুজনই access পায়; Mallory group-এ না থাকলে কিছুই পায় না।

### ৩. command
```
sudo groupadd awesomegroup
sudo adduser bob awesomegroup
sudo deluser bob awesomegroup
```
- **`groupadd awesomegroup`** — নতুন খালি group বানায়।
- **`adduser bob awesomegroup`** — bob-কে ওই group-এ ঢোকায় (এখানে adduser-এর কাজ user বানানো নয়, বরং group membership দেওয়া)।
- **`delgroup`** — পুরো group মুছে ফেলে।

### ৬. শর্টকাট টিপস
- **`/etc/passwd` = user list · `/etc/group` = group list।**
- মনে রাখার ট্রিক: "একই access অনেককে → group।"

---

## 3. Reading permissions — `ls -l` string 🔴

🎯 **Exam গুরুত্ব: HIGH** — সরাসরি আসে। MCQ-তে permission string-এর অংশ চিহ্নিত করতে বলে। (Q15: "others" অংশ · Q28: group owner অংশ)
**এক কথায়:** `ls -l`-এর প্রথম ১০টা অক্ষর পড়লেই বোঝা যায় file টা কী ধরনের আর কে কী করতে পারে।

> **আগের সাথে link:** owner/group আলাদা — তাই permission-ও owner/group/others আলাদা করে দেখানো হয়। সেটাই এখন পড়তে শিখি।

### ১. ধারণা
- **`ls -l` কী** — folder-এর ভেতরের জিনিস **long format**-এ দেখায়: permission, owner, group, size, date, নাম।
- **কেন** — কে কোন file-এ কী করতে পারবে সেটা চোখে দেখার জন্য।

### ২. গভীর ব্যাখ্যা — string টা ভাঙো
উদাহরণ লাইন:
```
-rw-rw-r-- 1 david david 0 Jan 1 12:00 afile
```
permission অংশ = `-rw-rw-r--`, মোট ১০ অক্ষর। এভাবে ভাঙে:

| অংশ | মান | মানে |
|---|---|---|
| ১ম অক্ষর | `-` | file type (`-` = file, `d` = directory, `l` = link) |
| পরের ৩টা | `rw-` | **owner** (যে বানিয়েছে) |
| মাঝের ৩টা | `rw-` | **group** (file-এর group-এর সদস্যরা) |
| শেষ ৩টা | `r--` | **others** (বাকি সবাই / "any user") |

- **`r`** = read (পড়া) · **`w`** = write (লেখা/বদলানো) · **`x`** = execute (চালানো)।
- **`-`** মানে ওই অনুমতি নেই।
- **`1 david david`** — এখানে প্রথম `david` = **owner**, দ্বিতীয় `david` = **group**। (Q28-এর উত্তর: দ্বিতীয় নামটা group owner।)

### ৩. উদাহরণ ও mapping
- **owner** = file-এর মালিক, সাধারণত যে বানিয়েছে।
- **group** = ওই file-এর group-এ থাকা সবাই।
- **others** = system-এর বাকি যেকোনো user — Q15-এর "permissions for any user accessing the file" মানে এই **others** অংশ।

### ৬. শর্টকাট টিপস
- মনে রাখো ক্রম: **owner → group → others** (বাঁ থেকে ডান)।
- প্রথম অক্ষরটা permission নয়, **type** — ভুল কোরো না।

### Exam-এ যেভাবে আসতে পারে
- "`-rw-rw-r--`-এ green-circle করা শেষ অংশ কী?" → **others / any user-এর permission**।
- "owner-এর পরের নামটা কী?" → file-এর **group**।

---

## 4. `chmod` + permission number math 🔴 (সবচেয়ে দামি)

🎯 **Exam গুরুত্ব: HIGH (highest value)** — number ↔ rwx রূপান্তর প্রায় নিশ্চিত আসে। (Q21: chmod 700 · P2-Q12: chmod 666 · উত্তরের ব্যাখ্যায় 777/444/555 ঘোরে)
**এক কথায়:** `chmod` permission বদলায়; আর প্রতিটা সংখ্যা আসলে rwx-এর একটা code — **r=4, w=2, x=1**, যোগ করলেই সংখ্যা।

> **আগের সাথে link:** #3-এ permission *পড়তে* শিখলাম, এখন সেটা *বদলাতে* শিখি।

### ১. ধারণা
- **`chmod` কী** — "change mode"; file/folder-এর permission পাল্টায়।
- **কেন** — কাউকে access দিতে বা আটকাতে।

### ২. গভীর ব্যাখ্যা — number কীভাবে কাজ করে
প্রতিটা অনুমতির একটা মান আছে (এটাই পুরো খেলার চাবি):

| অনুমতি | মান | কেন |
|---|---|---|
| `r` read | **4** | binary 100 |
| `w` write | **2** | binary 010 |
| `x` execute | **1** | binary 001 |

- **নিয়ম** — একটা group-এর rwx যোগ করলেই ওই অঙ্কের সংখ্যা।
- যেমন `rwx` = 4+2+1 = **7** · `rw-` = 4+2 = **6** · `r-x` = 4+1 = **5** · `r--` = **4** · `---` = **0**।
- **৩-অঙ্কের কোড** = owner · group · others (#3-এর সেই তিন ভাগ)।

পুরো reference table (এটা মুখস্থ রাখলে যেকোনো প্রশ্ন solve):

| সংখ্যা | rwx | মানে |
|---|---|---|
| 7 | `rwx` | সব |
| 6 | `rw-` | read + write |
| 5 | `r-x` | read + execute |
| 4 | `r--` | শুধু read |
| 0 | `---` | কিছুই না |

### ৩. উদাহরণ ও dry run (exam-এর আসল প্রশ্নগুলো)
```
chmod 700 afile     → rwx ------ ------   (শুধু owner সব পারে)
chmod 666 afile     → rw- rw- rw-         (সবাই read+write, কেউ execute না)
chmod 777 afile     → rwx rwx rwx         (সবাই সব — বিপজ্জনক)
chmod 444 afile     → r-- r-- r--         (সবাই শুধু read)
chmod 755 afile     → rwx r-x r-x         (owner সব, বাকিরা read+execute)
```
- **Q21 case** — file ছিল `-rw-rw-r--`, execute করতে গিয়ে "Permission denied"। কারণ `rw-` = read+write আছে, **x নেই**। user নিজেই owner, তাই `chmod 700` দিয়ে owner-কে `rwx` দিলেই চলে (sudo লাগে না, কারণ owner নিজে)।
- **P2-Q12 case** — "সবাই read+write পারবে, কেউ execute না" → প্রতি ভাগে `rw-` = 6 → **`chmod 666`**।

### ৪. Outcome
- এখন যেকোনো permission চাহিদা সংখ্যায় রূপান্তর করতে পারবে আর উল্টোটাও।

### ৬. শর্টকাট টিপস
- **r=4, w=2, x=1** — শুধু এটা মনে রাখো, বাকি সব বের করা যায়।
- দ্রুত মনে: **7=all, 6=rw, 5=r+x, 4=read only, 0=none**।
- **777 = খুলে দেওয়া, 700 = শুধু নিজে** — exam-এ এই দুটো বেশি আসে।
- **🔗 binary connection (Q20):** rwx = `111` = 7, rw- = `110` = 6 — তাই permission আসলে octal/binary। Q20 (10110→22) যে binary logic, সেই একই logic এখানে কাজ করছে।

### Exam-এ যেভাবে আসতে পারে
- "এই permission পেতে কোন chmod?" → প্রতি ভাগ যোগ করো।
- "chmod 755 মানে কী?" → `rwxr-xr-x`।

---

## 5. `chown` / `chgrp` — মালিকানা বদলানো

🎯 **Exam গুরুত্ব: Medium** — Lab 3-এ explicit ব্যবহার, slide থেকে "কোন command owner বদলায়?" আসতে পারে।
**এক কথায়:** `chmod` permission বদলায়; `chown`/`chgrp` বদলায় file-টা **কার** ও **কোন group-এর**।

> **আগের সাথে link:** permission তিন ভাগ owner/group/others — সেই owner আর group কে, সেটাই এখন set করি।

### ১. ধারণা
- **`chown`** — "change owner"; file-এর owner বদলায়।
- **`chgrp`** — "change group"; file-এর group বদলায়।

### ২. command + `-R`
```
sudo chown alice afile           # owner → alice
sudo chgrp awesomegroup afile    # group → awesomegroup
sudo chown -R alice /home/shared # পুরো folder + ভেতরের সব
```
- **`-R`** — recursive; folder-এর ভেতরের সব file/sub-folder-এও একসাথে apply (Lab 3-এ ১০টা file একসাথে বদলাতে এটাই লাগে)।
- **combined form** — `chown alice:awesomegroup afile` দিয়ে owner আর group একসাথে।

### ৬. শর্টকাট টিপস
- **chmod = কী করতে পারবে · chown = কে owner · chgrp = কোন group।**
- বড় folder-এ সবসময় `-R` মনে রাখো।

---

## 6. `su` / `whoami` — user switch ও verify

🎯 **Exam গুরুত্ব: Medium** — Lab verify করার মূল tool; "অন্য user হয়ে test করবে কীভাবে?" আসতে পারে।
**এক কথায়:** `su` দিয়ে অন্য user হয়ে যাও, `whoami` দিয়ে নিশ্চিত হও তুমি এখন কে।

### command
```
su alice -s /bin/bash    # alice হয়ে যাও, bash shell দিয়ে
whoami                    # এখন কে আছি?
exit                      # আগের user-এ ফিরে যাও
```
- **`su`** — "switch user"; অন্য user-এর পরিচয়ে shell চালায়।
- **`-s /bin/bash`** — কোন shell ব্যবহার হবে তা বলে দেয় (`/bin/bash` = সাধারণ Linux shell)।
- **`whoami`** — current user-এর নাম দেখায় — permission ঠিকঠাক কাজ করছে কিনা **সেই user হয়ে** যাচাই করতে।

### ৬. শর্টকাট টিপস
- permission সবসময় **ওই user হয়ে** test করো — না হলে আসল ছবি পাবে না।
- **su = হও · whoami = কে আছি দেখো।**

---

## 7. sudoers group (Lab challenge)

🎯 **Exam গুরুত্ব: Low** — সরাসরি কম, কিন্তু "কীভাবে কাউকে admin access দিবে?" হিসেবে আসতে পারে।
**এক কথায়:** কাউকে `sudo` group-এ ঢোকালে সে root-এর মতো admin কাজ করতে পারে — permission আর তাকে আটকায় না।

### ব্যাখ্যা
- **sudoers** — যারা `sudo` চালানোর অনুমতি পায়; Ubuntu-তে এরা `sudo` নামের group-এ থাকে।
- **Mallory-কে যোগ** — `sudo adduser mallory sudo`।
- **প্রভাব** — আগে Mallory কিছুই পারত না; এখন `sudo` দিয়ে সে যেকোনো file পড়তে/বদলাতে পারে, কারণ sudo সাধারণ permission **bypass** করে।
- **নিরাপত্তা শিক্ষা** — তাই sudo access খুব সাবধানে দিতে হয়; ভুল লোক পেলে পুরো system ঝুঁকিতে।

### ৬. শর্টকাট টিপস
- **sudo group = master key** — permission rule আর খাটে না।

---

## 8. File ops — `mkdir` / `touch` / `rm`

🎯 **Exam গুরুত্ব: Low** — সহজ, এক-লাইন উত্তর; দ্রুত mark।
**এক কথায়:** folder বানাও (`mkdir`), খালি file বানাও (`touch`), মুছে ফেলো (`rm`)।

```
mkdir /home/shared          # 'shared' folder বানাও
touch /home/shared/file1    # খালি file বানাও
rm -r /home/shared          # folder + ভেতরের সব মুছে ফেলো
```
- **`mkdir`** — "make directory"।
- **`touch`** — খালি file বানায় (বা থাকলে timestamp আপডেট করে)।
- **`rm`** — remove; **`-r`** = recursive, folder ও ভেতরের সব মুছতে লাগে।
- ⚠️ **`rm -r` সাবধানে** — Linux confirm চায় না, একবার গেলে ফেরত নেই।

---

## 9. Archive extract — `bunzip2` / `tar` (Part B শুরু)

🎯 **Exam গুরুত্ব: Medium** — ".tar.bz2 কীভাবে খুলবে?" সরাসরি আসতে পারে।
**এক কথায়:** `.tar.bz2` দুই স্তরে প্যাক করা — আগে `bunzip2` দিয়ে চাপ খোলো, পরে `tar` দিয়ে box খোলো।

> **আগের সাথে link:** এতক্ষণ permission নিয়ে কাজ করলাম; Part B-তে file *খুঁজে বের করা* শিখব — তার আগে file-গুলো extract করতে হবে।

### ১. ধারণা
- **archive কী** — অনেক file একসাথে এক box-এ রাখা (এক file)।
- **`tar`** — "tape archive"; অনেক file জোড়া দিয়ে এক `.tar` বানায় (compress করে না, শুধু জোড়ে)।
- **`bzip2`/`bunzip2`** — compression tool; size ছোট করে (`.bz2`)। তাই `.tar.bz2` = আগে tar করা, তারপর bzip2 দিয়ে চাপা।

### ২. command (ক্রম গুরুত্বপূর্ণ)
```
bunzip2 Gutenberg.tar.bz2     # ১. চাপ খোলো → পাবে Gutenberg.tar
tar -xvf Gutenberg.tar        # ২. box খোলো → আসল file-গুলো
```
- **`-x`** = extract · **`-v`** = verbose (কী বের হচ্ছে দেখায়) · **`-f`** = file নাম দিচ্ছি।
- **এক ধাপে বিকল্প** — `tar -xjvf Gutenberg.tar.bz2` (`j` = bzip2 handle করে)।

### ৬. শর্টকাট টিপস
- মনে রাখো: **`.tar.bz2` খুলতে → bunzip2 পরে tar -xvf।**
- `xvf` = e**X**tract **V**erbose **F**ile।

---

## 10. `find -name` — filename দিয়ে খোঁজা

🎯 **Exam গুরুত্ব: Medium** — search command চেনা থাকতে হয়।
**এক কথায়:** `find` কোথা থেকে খুঁজবে বললে, নাম মিলিয়ে file বের করে দেয়।

```
find /path -name "*.txt"
```
- **`find`** — filesystem-এ file খোঁজে।
- **`/path`** — কোথা থেকে খোঁজা শুরু হবে।
- **`-name "*.txt"`** — নাম মেলাও; **`*`** = যেকোনো কিছু (wildcard), তাই সব `.txt`।

### ৬. টিপস
- **`-name` = নাম দিয়ে · `-type f` = শুধু file (folder বাদ)।**

---

## 11. `grep -r` / text search — ভেতরের লেখা খোঁজা

🎯 **Exam গুরুত্ব: Medium** — file-এর *ভেতরে* কিছু খোঁজার মূল tool।
**এক কথায়:** `find` নাম খোঁজে; `grep` file-এর **ভেতরের লেখা** খোঁজে।

> **আগের সাথে link:** #10-এ নাম দিয়ে খুঁজলাম; এখন ভেতরের content দিয়ে খুঁজি।

```
grep -r "verdigris" /path           # folder-এর সব file-এ "verdigris" খোঁজো
grep -r -C 3 foo README.txt          # match-এর আগে-পরে ৩ লাইন দেখাও
```
- **`grep`** — কোনো লেখা (string) যে লাইনে আছে সেটা দেখায়।
- **`-r`** — recursive; folder-এর ভেতরের সব file-এ খোঁজে।
- **`-C 3`** — context; match-এর চারপাশের ৩ লাইন দেখায় (পুরো বাক্য বুঝতে)।
- **বিকল্প** — `find /path -type f -exec grep -H 'text' {} \;` — প্রতি file-এ grep চালায়, `-H` মানে কোন file-এ পাওয়া গেল তার নাম দেখায়।

**Lab Q1 logic:** "verdigris" ৯ বার আছে → `grep -rc "verdigris" /path` (`-c` = count) বা সব match গুনে।

### ৬. টিপস
- **find = নাম · grep = ভেতরের লেখা।** এই পার্থক্যটা exam-এ key।

---

## 12. Advanced find — size, date, `du`

🎯 **Exam গুরুত্ব: Low** — কম আসে, lab question-এর জন্য রাখা; দ্রুত চোখ বুলিয়ে যাও।
**এক কথায়:** `find`-কে শর্ত দিয়ে size/date দিয়েও file বের করা যায়, আর `du` দিয়ে সবচেয়ে বড় file পাওয়া যায়।

```
find /path -type f -size 68c              # ঠিক 68 byte (c = bytes)
find /path -type f -size +512k            # 512k-এর বড়
du -a /path | sort -n -r | head -n 20     # সবচেয়ে বড় ফাইল ২০টা
```
- **`-size 68c`** — `c` = bytes, ঠিক ৬৮ byte।
- **`+512k`** — `+` = এর বেশি, `k` = kilobytes।
- **`du`** — "disk usage"; কোন file কত জায়গা নেয়।
- **`sort -n -r`** — সংখ্যা অনুসারে (`-n`) উল্টো ক্রমে (`-r`, বড় আগে)।
- **`head`** — উপরের কয়েকটা লাইন।
- **frequency খোঁজা** — `sed -e 's/\s/\n/g' < file | sort | uniq -c | sort -nr | head` → প্রতিটা word আলাদা লাইনে ভাঙে, count করে, বেশি-আসা গুলো উপরে দেখায় (অস্বাভাবিক IP/username ধরতে কাজে দেয়)।

### ৬. টিপস
- **size unit:** `c`=byte · `k`=KB · `M`=MB।
- **pipe `|`** — এক command-এর output পরের command-এ পাঠায় (find → sort → head একটা চেইন)।

---

## 13. Linux ↔ Windows command map 🔴

🎯 **Exam গুরুত্ব: HIGH (easy marks)** — matching প্রশ্নে সরাসরি আসে। (P2-Q5 পুরোটা · Q29: ls=dir) — মুখস্থ থাকলে free marks।
**এক কথায়:** একই কাজের Linux আর Windows command জোড়ায় জোড়ায় মনে রাখো।

| কাজ | Linux | Windows |
|---|---|---|
| list করা | `ls` | `dir` |
| network info | `ifconfig` | `ipconfig` |
| copy | `cp` | `copy` |
| move | `mv` | `move` |
| process list | `ps -e` | `tasklist` |
| process kill | `kill` | `taskkill` |
| text edit | `gedit` | `notepad` |
| delete | `rm` | `del` |
| text search | `grep` | `findstr` |

### ৬. শর্টকাট টিপস
- দ্রুত মনে: **ls↔dir, rm↔del, cp↔copy, mv↔move, grep↔findstr।**
- Linux-এ `ls -la` = detailed + hidden file (Windows `dir`-এর সবচেয়ে কাছাকাছি)।

---
---

## সারাংশ (Full Summary — exam-night skim)

1. **user** = "কে" (account, UID); `adduser` / `deluser`; তথ্য `/etc/passwd`-এ।
2. **group** = user-এর দল; `groupadd`, `adduser bob grp`; তালিকা `/etc/group`-এ।
3. **`ls -l` string** = `[type][owner][group][others]`; প্রথম অক্ষর type, পরে ৩×৩ rwx। 🔴
4. **`chmod` number** = r4 w2 x1 যোগ করো; 7=rwx 6=rw 5=rx 4=r 0=none; 700/666/777। 🔴
5. **`chown`/`chgrp`** = owner/group বদলায়; বড় folder-এ `-R`।
6. **`su` / `whoami`** = অন্য user হও / কে আছি verify করো।
7. **sudoers** = `sudo` group-এ দিলে permission bypass, full admin।
8. **`mkdir`/`touch`/`rm -r`** = folder/খালি file/মুছে ফেলা।
9. **extract** = `.tar.bz2` → `bunzip2` তারপর `tar -xvf`।
10. **`find -name`** = নাম দিয়ে খোঁজা (`*` wildcard)।
11. **`grep -r`** = ভেতরের লেখা খোঁজা; `-C` context লাইন।
12. **advanced find** = `-size`, date sort, `du | sort -nr | head`।
13. **Linux↔Windows** = ls/dir, rm/del, cp/copy, mv/move, grep/findstr। 🔴

---

## বাস্তব ব্যবহার টেবিল (Lab → Project)

| Topic | Lab-এ যা শিখলাম | Real Project-এ কোথায় লাগে |
|---|---|---|
| users/groups | Alice/Bob/Mallory বানানো | server-এ team access control |
| chmod/chown | shared folder permission | web server file security (777 কখনো না) |
| su/whoami | user হয়ে test | deploy script-এ identity check |
| sudoers | Mallory-কে sudo | admin access management |
| tar/bunzip2 | Gutenberg খোলা | backup/restore, software download |
| find/grep | file ও text খোঁজা | log analysis, debugging, IP খোঁজা |

---

## রিফ্লেকশন (assignment style)

In this lab I learned how Linux controls access by separating *who* a user is, *which group* they belong to, and *what* they are allowed to do through file permissions. By creating Alice, Bob, and Mallory and managing them through group membership, I saw how a single group permission can grant or deny access to many users at once, which is far more practical than setting permissions per user. The `chmod` numeric system finally made sense once I understood that read, write, and execute map to 4, 2, and 1 — so any permission set is just a sum I can compute. The second half of the lab showed me how to actually *find* things on a real filesystem: `find` for filenames, `grep` for text inside files, and tools like `du` and `sort` to investigate size and frequency. Together these skills connect directly to real work — securing a server and investigating logs both depend on exactly this control and search ability.

---

## ⏱️ দ্রুত রিভিশন + সম্ভাব্য প্রশ্ন (শেষ ৩০ মিনিট)

**সবচেয়ে আগে মুখস্থ করো (🔴):**
- **r=4, w=2, x=1** → 7=rwx, 6=rw-, 5=r-x, 4=r--, 0=---
- `ls -l` string ক্রম: **type → owner → group → others**
- Linux↔Windows: **ls/dir, rm/del, cp/copy, mv/move, ps -e/tasklist, kill/taskkill, grep/findstr, gedit/notepad, ifconfig/ipconfig**

**সম্ভাব্য exam প্রশ্ন + এক-লাইন উত্তর:**
- "File `-rw-rw-r--`, execute করতে গিয়ে denied — fix?" → execute নেই; **`chmod 700`** (user owner, sudo লাগবে না)।
- "সবাই read+write পারবে, execute না — কোন chmod?" → **`chmod 666`**।
- "`chmod 755` মানে?" → `rwxr-xr-x`।
- "`ls -l`-এর শেষ ৩ অক্ষর কাদের?" → **others / any user**।
- "owner-এর পরের নামটা?" → file-এর **group**।
- "Linux-এ `dir`-এর সমতুল্য?" → **`ls`**।
- "`.tar.bz2` কীভাবে খুলবে?" → **`bunzip2` তারপর `tar -xvf`**।
- "file-এর ভেতরে একটা শব্দ খুঁজবে?" → **`grep -r "word" /path`**।
- "নাম দিয়ে file খুঁজবে?" → **`find /path -name "*.txt"`**।
- "কাউকে admin access দিবে?" → **`adduser <user> sudo`**।

**মূল আত্মবিশ্বাসের লাইন:** "Permission মানে owner/group/others × r/w/x। সংখ্যা = ওই rwx-এর যোগফল। এটুকু পারলে chmod-এর সব প্রশ্ন পারি।"
