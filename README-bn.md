# Fix Termux Sudo/Root (Tsu Patcher)

![Views](https://komarev.com/ghpvc/?username=msrofficial-fix-termux-root&label=Repository%20Views&color=0e75b6&style=flat)
![Termux](https://img.shields.io/badge/Termux-Android-green?logo=android)
![Root](https://img.shields.io/badge/Root-Required-red)
![Sudo](https://img.shields.io/badge/Fix-Sudo%2FTsu-blue)

**[📖 Read in English (ইংরেজিতে পড়ুন)](README.md)**

**অ্যান্ড্রয়েড Termux-এ "No superuser binary detected" বা "Root permission denied" সমস্যার একটি সম্পূর্ণ এবং স্বয়ংক্রিয় সমাধান।**

**Keywords:** `termux root fix bangla`, `termux sudo error`, `tsu patcher`, `kernelsu termux fix`, `magisk termux root`, `termux su binary not found`, `android terminal root`, `msrofficial`

---

## সমস্যাটি কী?
অনেক সময় আপনার অ্যান্ড্রয়েড ডিভাইসটি রুট করা থাকলেও (বিশেষ করে KernelSU বা Magisk-এর লেটেস্ট ভার্সনে), Termux-এর ডিফল্ট `tsu` প্যাকেজটি রুট বাইনারি (`su`) খুঁজে পায় না। এর কারণ হলো, ডিফল্ট স্ক্রিপ্টটি `/debug_ramdisk/su` বা সিস্টেমলেস রুট মেথডগুলোর অন্যান্য কাস্টম লোকেশনে সার্চ করে না।

এই রিপোজিটরিতে একটি অটোমেটেড স্ক্রিপ্ট দেওয়া হয়েছে যা সম্ভাব্য সব রুট ম্যাট্রিক্স স্ক্যান করে, সমস্যা তৈরি করা বাইনারিগুলো সরিয়ে ফেলে এবং টার্মিনালে সঠিকভাবে `sudo` এনভায়রনমেন্ট সেটআপ করে।

---

## পদ্ধতি ১: অটোমেটেড স্ক্রিপ্ট (প্রস্তাবিত)
এটি সবচেয়ে দ্রুত এবং নির্ভরযোগ্য পদ্ধতি। এটি `fix.sh` স্ক্রিপ্টটি ডাউনলোড করে, রুট লোকেশন স্ক্যান করে, প্রয়োজনীয় প্যাকেজ ইনস্টল করে এবং আপনাকে সরাসরি রুট শেল-এ নিয়ে যায়।

আপনার Termux টার্মিনালে নিচের কমান্ডটি রান করুন:

```bash
curl -sO [https://raw.githubusercontent.com/kafi5/fix-termux-root/main/fix.sh](https://raw.githubusercontent.com/kafi5/fix-termux-root/main/fix.sh) && chmod +x fix.sh && ./fix.sh
```

---

## পদ্ধতি ২: এক লাইনের কমান্ড (বিকল্প)
যদি অটোমেটেড স্ক্রিপ্ট কাজ না করে, অথবা আপনি `tsu` প্যাকেজ রিমুভ না করে শুধু সেটিকে ফিক্স করতে চান, তবে নিচের কমান্ডটি রান করুন। এটি ফাইলের একটি ব্যাকআপ রাখবে এবং সাথে সাথে ফিক্স করে দিবে:

```bash
pkg update && pkg install tsu -y
cp $PREFIX/bin/tsu $PREFIX/bin/tsu.bak && sed -i 's|^SU_BINARY_SEARCH=.*|SU_BINARY_SEARCH=("/system/xbin/su" "/system/bin/su" "/debug_ramdisk/su" "/sbin/su")|' $PREFIX/bin/tsu && echo "tsu patched successfully"
```

**যাচাইকরণ:** কমান্ড শেষে `tsu` বা `sudo su` টাইপ করুন। যদি `#` প্রম্পট দেখতে পান, তার মানে আপনি সফলভাবে রুট এক্সেস পেয়েছেন।

---

## পদ্ধতি ৩: ম্যানুয়াল এডিট (বিকল্প)
আপনি যদি পুরো প্রক্রিয়াটি নিজে ম্যানুয়ালি করতে চান, তবে একটি টেক্সট এডিটর ব্যবহার করে কনফিগারেশন ফাইলটি এডিট করতে পারেন।

### ১. প্রয়োজনীয় টুলস এবং রিপোজিটরি ইনস্টল করুন:
```bash
pkg install tsu nano file root-repo sudo -y
```

### ২. ন্যানো (nano) এডিটর দিয়ে tsu ফাইলটি ওপেন করুন:
```bash
nano $PREFIX/bin/tsu
```

### ৩. ফিক্স অ্যাপ্লাই করুন:
স্ক্রল করে নিচে নামুন এবং `SU_BINARY_SEARCH=` দিয়ে শুরু হওয়া লাইনটি খুঁজে বের করুন। এরপর ব্র্যাকেটের ভেতরে `"/debug_ramdisk/su"` যুক্ত করুন। পরিবর্তন করার পর লাইনটি ঠিক নিচের মতো দেখাবে:

```text
SU_BINARY_SEARCH=("/system/xbin/su" "/system/bin/su" "/debug_ramdisk/su")
```

### ৪. সেভ করুন এবং বের হয়ে আসুন:
* `CTRL + X` চাপুন
* `Y` চাপুন (Yes এর জন্য)
* `Enter` চাপুন

---

## তৈরি এবং রক্ষণাবেক্ষণে
ডেভেলপ এবং মেইনটেইন করেছেন **RHK {kafi}**। যেকোনো আপডেট বা সাপোর্টের জন্য যোগাযোগ করতে পারেন:

* **GitHub:** [kafi5](https://github.com/kafi5)
* **Telegram:** [@rhktech1m](https://t.me/rhktech1m)
* **Telegram Channel:** [@duomodhub](https://t.me/duomodhub)
