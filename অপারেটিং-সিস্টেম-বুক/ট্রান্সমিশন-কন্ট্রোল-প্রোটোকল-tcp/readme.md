# TCP কী? (Transmission Control Protocol)

TCP-এর পূর্ণরূপ হলো Transmission Control Protocol (ট্রান্সমিশন কন্ট্রোল প্রোটোকল)। এটি নেটওয়ার্কে দুটি ডিভাইসের মধ্যে নির্ভরযোগ্যভাবে ডেটা আদান-প্রদান করার একটি নিয়ম বা প্রোটোকল।

সহজ ভাষায়, ইন্টারনেটের মাধ্যমে আপনি যখন কাউকে মেসেজ পাঠান, ওয়েবসাইট ব্রাউজ করেন বা কোনো ফাইল ডাউনলোড করেন, তখন TCP নিশ্চিত করতে সাহায্য করে যে ডেটা সঠিকভাবে এবং সঠিক ক্রমে পৌঁছেছে।

## ১. বাস্তব জীবনের উদাহরণ

ধরুন, আপনি আপনার বন্ধুকে একটি ১০০ পৃষ্ঠার বই ডাকযোগে পাঠাবেন।

![Cómo numerar las páginas en Word - Softonic](https://images.openai.com/static-rsc-4/7hh56oDfU2M1KLvFNrcFBIxUUyzUiKjGURUT8lULIks_oIOwY2CgRSL7v8uvvpUDMTD2m6KT2uryo07KD5qVLJ7cv7Vg0_SPKm_cGCrl5i6aQFbuz7oDOpr6aFSZzc5CbZ-VN3sTM3QrybB6eoOkziPYp63tuST6hs8zYlraUvE?purpose=inline)

Add to Favorites

ধাপ ১: ডেটাকে ছোট ছোট অংশে ভাগ করা

বইয়ের পৃষ্ঠাগুলোকে কয়েকটি ছোট প্যাকেটে ভাগ করা হলো। TCP-ও ডেটাকে ছোট ছোট অংশে ভাগ করে পাঠানোর ব্যবস্থা করে।

![Ready to deliver – USPS Employee News](https://images.openai.com/static-rsc-4/JV23Zp9lS3jgkVGcd2jQeEtvn6-vqF5KpElw7_8gCN5msbap-BukhB36kHooEr-OqJL3M3wiNhOXB_uz1tLpAolXCT9_IjsW99sfHyD0pFdevwm5VQQ6JEvT1ekdwBJej7Gfva78V_qkrgC_EtVvLQFTFwOiRm-F4PPxet9AX0E?purpose=inline)

Add to Favorites

ধাপ ২: প্যাকেট পাঠানো

প্রতিটি প্যাকেট গন্তব্যে পাঠানো হয়। নেটওয়ার্কে ডেটার অংশগুলো ভিন্ন পথেও পৌঁছাতে পারে।

![Armenians go to the polls under Russian pressure aimed at preventing a drift toward West - The Journal](https://images.openai.com/static-rsc-4/-kobBrUFLtXUyH_9Y0GKVHKOD4aKVmyx8jdA11fTNwCtV6UDOBB-9jpts5jFnc-_x0z6PRzB9PuaeDpAXuXC72OWRWptFECRAy_M4KkKXGQm_Mk5oHPIYdZ763UPv6uFGJLCbjzO7wnQTvWS7UuIaR3ZFwD1HYa5Sv5fPbcIbrA?purpose=inline)

Add to Favorites

ধাপ ৩: সঠিক ক্রমে সাজানো

প্যাকেটগুলো পৌঁছানোর পর TCP সেগুলো সঠিক ক্রমে সাজাতে সাহায্য করে।

![Delivery mail man giving parcel box to recipient, Young man signing receipt of delivery package from post shipment courier at home](https://images.openai.com/static-rsc-4/YLhurIwzp84wah4Gms21dOpOXSfh0tKyScsgkJvarq1CBirif2OuciJQmMEN28uMWHxlXrEskLzfPYpJksaJiE6EbqovjBlXXxDj6RzEHDZtRpZPu4J59pEjLrGpm2AgmJUuCKdheC6dG9iZ43K5BhfXLeyGtBb_C_rzjSoT5vaE-KVAMFD799BQEQ7y7Qtu?purpose=inline)

Add to Favorites

ধাপ ৪: ডেটা পৌঁছেছে কি না যাচাই করা

প্রয়োজন হলে TCP হারিয়ে যাওয়া ডেটা পুনরায় পাঠায় এবং প্রাপ্তির স্বীকৃতি (Acknowledgment বা ACK) ব্যবহার করে।

এভাবেই TCP নির্ভরযোগ্য ডেটা আদান-প্রদানে সাহায্য করে।

## ২. TCP কীভাবে কাজ করে?

Client (আপনার কম্পিউটার)

ডেটা পাঠানোর অনুরোধ

TCP Connection

Network / Internet

প্যাকেট পরিবহন ও নির্ভরযোগ্যতা ব্যবস্থাপনা

TCP Connection

Server (ওয়েব সার্ভার)

ডেটা গ্রহণ ও উত্তর পাঠানো

TCP সাধারণত নিচের ধাপগুলো অনুসরণ করে:

1. Connection Establishment: ডেটা পাঠানোর আগে সংযোগ স্থাপন করে।

2. Data Transfer: ডেটাকে TCP segment আকারে পাঠায়।

3. Acknowledgment: প্রাপ্ত ডেটার স্বীকৃতি গ্রহণ করে।

4. Retransmission: প্রয়োজন হলে হারিয়ে যাওয়া ডেটা আবার পাঠায়।

5. Connection Termination: কাজ শেষ হলে সংযোগ বন্ধ করে।

## ৩. TCP Three-Way Handshake কী?

TCP সংযোগ স্থাপনের জন্য সাধারণত তিনটি ধাপ ব্যবহৃত হয়। একে Three-Way Handshake বলে।

Client

Server

SYN →

1. Client সংযোগের অনুরোধ করে।

← SYN-ACK

2. Server অনুরোধ গ্রহণ করে এবং নিজের প্রস্তুতির সংকেত দেয়।

ACK →

3. Client স্বীকৃতি পাঠায়। সংযোগ স্থাপিত হয়।

মনে রাখবেন:

* SYN: সংযোগ স্থাপনের অনুরোধ।

* ACK: প্রাপ্তির স্বীকৃতি।

* SYN-ACK: SYN অনুরোধের স্বীকৃতি এবং নিজের SYN পাঠানো।

## ৪. TCP কোথায় ব্যবহার করা হয়?

| ব্যবহার            | TCP কেন ব্যবহৃত হয়?                          |
| ------------------ | --------------------------------------------- |
| HTTP / HTTPS       | ওয়েবসাইটের ডেটা নির্ভরযোগ্যভাবে আদান-প্রদানে |
| SSH                | রিমোট সার্ভারে নিরাপদ শেল সেশন পরিচালনায়     |
| FTP                | ফাইল স্থানান্তরে                              |
| SMTP               | ইমেইল পাঠাতে                                  |
| MySQL / PostgreSQL | নেটওয়ার্কের মাধ্যমে ডেটাবেস সংযোগে           |

গুরুত্বপূর্ণ: HTTPS-এর ক্ষেত্রে TCP সাধারণত ব্যবহৃত হয়, তবে HTTP/3 QUIC ব্যবহার করে, যা UDP-এর ওপর তৈরি।

## ৫. TCP বনাম UDP

TCP এবং UDP—দুটিই Transport Layer-এর প্রোটোকল। কিন্তু তাদের কাজের ধরন আলাদা।

| বৈশিষ্ট্য     | TCP                                        | UDP                                                  |
| ------------- | ------------------------------------------ | ---------------------------------------------------- |
| নির্ভরযোগ্যতা | ডেটা পৌঁছানো ও ক্রম ঠিক রাখার ব্যবস্থা আছে | নিজে থেকে ডেলিভারি নিশ্চিত করে না                    |
| সংযোগ         | Connection-oriented                        | Connectionless                                       |
| গতি ও ওভারহেড | তুলনামূলক বেশি ওভারহেড                     | তুলনামূলক কম ওভারহেড                                 |
| ব্যবহার       | ওয়েব, SSH, ফাইল স্থানান্তর                | DNS, লাইভ স্ট্রিমিং, অনলাইন গেমিংসহ বিভিন্ন ক্ষেত্রে |

## ৬. Linux-এ TCP সংযোগ কীভাবে দেখবেন?

আপনি Ubuntu বা অন্য Linux ডিস্ট্রিবিউশন ব্যবহার করলে টার্মিনালে নিচের কমান্ডগুলো চালাতে পারেন।

কমান্ড ১: TCP সংযোগ দেখুন

Bash

```
ss -t
```

কমান্ড ২: Listening TCP port দেখুন

Bash

```
ss -tln
```

কমান্ড ৩: Listening এবং established TCP socket দেখুন

Bash

```
ss -tan
```

কমান্ডগুলোর অর্থ:

* `-t` = TCP socket দেখায়।

* `-l` = Listening socket দেখায়।

* `-n` = IP address ও port সংখ্যায় দেখায়।

* `-a` = Listening এবং non-listening socket দেখায়।

উদাহরণস্বরূপ, কোনো ওয়েব সার্ভার TCP port `80` অথবা `443`-এ সংযোগ গ্রহণ করছে কি না, তা `ss -tln` দিয়ে পরীক্ষা করা যায়।

## ৭. TCP মনে রাখার সহজ উপায়

TCP-কে একজন দায়িত্বশীল ডাক সরবরাহকারীর সঙ্গে তুলনা করতে পারেন, যে:

* ডেটা পাঠায়।

* প্রাপ্তির স্বীকৃতি যাচাই করে।

* হারানো অংশ পুনরায় পাঠানোর ব্যবস্থা করে।

* ডেটার সঠিক ক্রম বজায় রাখে।

এক কথায়: TCP হলো এমন একটি Transport Layer প্রোটোকল, যা নেটওয়ার্কের মাধ্যমে দুটি প্রান্তের অ্যাপ্লিকেশনের মধ্যে নির্ভরযোগ্য, ক্রমানুসারী এবং ত্রুটি-পরীক্ষিত byte stream আদান-প্রদান করতে সাহায্য করে।
