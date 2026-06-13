# ০৬ - লিকুইডিটি কনসেপ্ট (Liquidity Concept)

লিকুইডিটি হলো এমন একটি ঘটনা যেখানে বড় প্লেয়ার (Smart Money, Institution, Bank) ইচ্ছাকৃতভাবে এমন প্রাইস লেভেলে মার্কেট নিয়ে যায় যেখানে সাধারণ ট্রেডারদের Stop Loss বা Pending Orders জমা থাকে। সেই Liquidity সংগ্রহ করার পর মার্কেট বিপরীত দিকে শক্তিশালী মুভ করে।

## লিকুইডিটি আসলে কী?

মার্কেটে বড় অর্ডার একবারে Execute করা কঠিন।

***তাই বড় প্রতিষ্ঠানগুলো প্রথমে লিকুইডিটি খোঁজে:***

* Retail Trader-এর Stop Loss
* Breakout Trader-এর Pending Order
* Equal High / Equal Low
* Previous Day High / Low
* Support / Resistance

`এই জায়গাগুলোতে অনেক Order জমা থাকে।`

***সহজ ভাষায়:***

> লিকুইডিটি  হলো এমন জায়গা যেখানে অনেক Buy Order, Sell Order, Stop Loss এবং Pending Order জমা থাকে।

Institutional Trader-রা এই Order গুলো ব্যবহার করে তাদের বড় Position Execute করে।

## কেন লিকুইডিটি  গুরুত্বপূর্ণ?

ধরুন একটি Bank ৫০ কোটি টাকার Stock কিনতে চায়।

সে কি Market Order দিয়ে একবারে কিনতে পারবে? `না।`

***কারণ:***

>Buyer বেশি Seller কম হলে Price দ্রুত বেড়ে যাবে।
তাই Institution লিকুইডিটি খোঁজে।

## লিকুইডিটি  কোথায় থাকে?

সাধারণত ৫টি জায়গায়।

### 1. Equal High (EQH)

```text
      H
      ▲

      H
      ▲
```

দুটি প্রায় সমান High

***Retail Trader ভাবে:***

> Resistance

***Institution ভাবে:***

> Liquidity Pool

### 2. Equal Low (EQL)

```text
      ▼
      L

      ▼
      L
```

দুটি সমান Low. এখানে অনেক Stop Loss থাকে।

### 3. Previous Day High (PDH)

আগের দিনের সর্বোচ্চ দাম।

```text
Yesterday High
──────────────
```

### 4. Previous Day Low (PDL)

আগের দিনের সর্বনিম্ন দাম।

```text
Yesterday Low
──────────────
```

### 5. Major Swing High / Low

Phase 3-এর Swing Points।

```text
Swing High
     ▲

Swing Low
     ▼
```

এগুলো Liquidity Magnet।

## Buy Side Liquidity (BSL)

এটি High-এর উপরে থাকে।

***ধরুন:***

```text
        Resistance

───────────────
```

অনেক Retail Trader Sell করেছে।

তাদের Stop Loss:

```text
Resistance
───────────────

Stop Loss
▲ ▲ ▲ ▲ ▲
```

এগুলোই Buy Side Liquidity।

## Sell Side Liquidity (SSL)

এটি Low-এর নিচে থাকে।

অনেক Trader Buy করেছে।

তাদের Stop Loss:

```text
Support

───────────────

▼ ▼ ▼ ▼ ▼
Stop Loss
```

এগুলোই Sell Side Liquidity।

## লিকুইডিটি সুইপ (Liquidity Sweep) কী?

এটি Smart Money-এর সবচেয়ে গুরুত্বপূর্ণ Concept।

***বিয়ারিশ লিকুইডিটি সুইপ (Buy Side Liquidity Grab):***

ধরুন মার্কেট একটি High তৈরি করেছে।

```text
            H
            │
        ╭───┴───╮
        │       │
        │       │
        │       │
        ╰───┬───╯
            │
```

* অনেক ট্রেডার এখানে Resistance দেখে Sell করেছে।

* তাদের Stop Loss থাকবে High-এর উপরে।

```text
             Stop Loss Area
             
        -----------------------
                 ↑ ↑ ↑
                 ↑ ↑ ↑

            H
            │
        ╭───┴───╮
        │       │
        │       │
        │       │
        ╰───┬───╯
```

* Institution জানে এখানে প্রচুর Liquidity আছে।

* তাই Price সাময়িকভাবে High-এর উপরে Push করবে।

```text
                  ▲
                  │
                  │ Sweep

            H ────┼────────
                  │
             ╭────┴────╮
             │         │
             ╰────┬────╯
                  │

```

* Stop Loss Trigger হবে।
* Breakout Buyer-রাও Buy করবে।
* এতে Institution Liquidity পাবে।
* তারপর Market হঠাৎ নিচে নামবে।

```text
                  ▲
                  │ Sweep
                  │
            H ────┼────────
                  │
             ╭────┴────╮
             │         │
             ╰────┬────╯
                  │
                  ▼
                  ▼
                  ▼

```

* এটাকে Buy Side Liquidity Sweep বলে।

* এরপর সাধারণত Bearish Move আসে।

***বুলিশ লিকুইডিটি সুইপ (Sell Side Liquidity Grab)***

* এবার Low-এর নিচে Liquidity আছে।

```text
        ╭────┬────╮
        │         │
        ╰────┴────╯
             │
             L

```

* অনেক Buy Trader-এর Stop Loss Low-এর নিচে।

```text
             L
             │
   --------------------
      Stop Loss Area
         ↓ ↓ ↓
         ↓ ↓ ↓

```

* Institution Price নিচে নামিয়ে Liquidity সংগ্রহ করবে।

```text
             L
             │
─────────────┼────────
             ▼
             ▼ Sweep

         ╭───┴───╮
         │       │
         ╰───┬───╯

```

* তারপর Price শক্তিশালীভাবে উপরে উঠবে।

```text
             L
             │
─────────────┼────────
             ▼ Sweep
         ╭───┴───╮
         │       │
         ╰───┬───╯
             ▲
             ▲
             ▲

```

* এটাকে Sell Side Liquidity Sweep বলে।

* এরপর সাধারণত Bullish Move আসে।

### ক্যান্ডেলস্টিক উদাহরণ

***Bearish Sweep:***

```text
            Resistance

                │
                │
        🟩
        🟩
        🟩
        🟩

                🟩
                🟩
                🟩

                🟥
                🟥
                🟥
                🟥
                🟥

```

***ব্যাখ্যা:***

* Price Resistance-এর কাছে যায়।
* একটি Candle Wick দিয়ে High ভাঙে।
* সবাই ভাবে Breakout হয়েছে।
* Candle আবার নিচে Close করে।
* পরের Candle গুলো শক্তিশালী Sell শুরু করে।

***Bullish Sweep:***

```text
                🟥
                🟥
                🟥

                🟥
                🟥

        Support ─────────

                🟥
                 │
                 │
                 ▼

                🟩
                🟩
                🟩
                🟩
                🟩

```

***ব্যাখ্যা:***

* Support-এর নিচে Wick যায়।
* Stop Loss Hit করে।
* Candle আবার Support-এর উপরে Close করে।
* শক্তিশালী Buy Move শুরু হয়।

### Liquidity Sweep চিনার ৫টি উপায়

***1. Equal High / Equal Low***

```text
      H          H
      ▲          ▲
      │          │
──────┴──────────┴────
```

এখানে Liquidity থাকার সম্ভাবনা বেশি।

***2. Long Wick:*** Sweep Candle-এ সাধারণত বড় Wick থাকে।

```text
      │
      │
      │
   ┌──┴──┐
   │     │
   └─────┘
```

***3. Quick Rejection:*** Liquidity নেওয়ার পর Price দ্রুত ফিরে আসে।

***4. Market Structure Shift (MSS):*** Sweep-এর পরে Structure Break হলে Confirmation শক্তিশালী হয়।

***5. Volume Spike:*** অনেক সময় Sweep Candle-এ Volume অস্বাভাবিক বেড়ে যায়।

### প্রফেশনাল ট্রেডার (Professional Traders) কীভাবে ব্যবহার করে?

***সাধারণ খুচরা ব্যবসায়ী (Retail Trader):***

* Breakout দেখেই Entry
* Stop Loss High/Low-এর বাইরে

***পেশাদার ব্যবসায়ী (Professional Trader):***

* Liquidity কোথায় আছে খুঁজে Sweep-এর জন্য অপেক্ষা করে
* Rejection + MSS দেখে Entry নেয়

এই কারণে অনেক সময় আপনি দেখবেন Price আপনার Stop Loss হিট করে, তারপর আপনার Analysis অনুযায়ী Move করে। বেশিরভাগ ক্ষেত্রে এটি Liquidity Sweep-এর ফল।

***সংক্ষেপে:***

Liquidity Sweep হলো এমন একটি মুভ যেখানে Market প্রথমে Stop Loss এবং Pending Order-এর Liquidity সংগ্রহ করে, তারপর আসল Direction-এ শক্তিশালীভাবে Move করে। Smart Money Concept (SMC) ও ICT Trading-এর অন্যতম গুরুত্বপূর্ণ ধারণা এটি।

## লিকুইডিটি গ্র্যাব বনাম লিকুইডিটি সুইপ (Liquidity Grab vs Liquidity Sweep)

অনেকে দুটিকে একই ভাবে।

***Liquidity Grab:***

```text
High Break

▼

Immediate Return
```

* দ্রুত ঘটে।

* এক Candle-এও হতে পারে।

***Liquidity Sweep:***

```text
High Break

কিছু সময় থাকে

তারপর Reverse

সময় বেশি লাগে।
```

## প্রলোভন (Inducement) কী?

Institution আগে Trader-দের Trap করে।

***উদাহরণ:***

```text
HH
HL
HH
HL
```

***সবাই ভাবে:***

* Strong Uptrend

* সবাই Buy করে।

* Institution Liquidity তৈরি করে।

***তারপর:***

```text
BSL Sweep

↓

CHOCH

↓

Sell
```

এটাকে Inducement বলে।

## লিকুইডিটি + বাজার কাঠামো (Liquidity + Market Structure)

এটাই Professional Setup।

***Bullish Setup:***

```text
Sell Side Liquidity

↓

Sweep

↓

CHOCH

↓

Bullish BOS

↓

Buy
```

***Bearish Setup:***

```text
Buy Side Liquidity

↓

Sweep

↓

CHOCH

↓

Bearish BOS

↓

Sell
```

## লিকুইডিটি + ডিমান্ড জোন (Liquidity + Demand Zone)

High Probability Setup

```text
Demand Zone

↓

SSL Sweep

↓

Hammer

↓

BOS

↓

Buy
```

## লিকুইডিটি + সাপ্লাই জোন (Liquidity + Supply Zone)

```text
Supply Zone

↓

BSL Sweep

↓

Shooting Star

↓

BOS

↓

Sell
```

## লিকুইডিটি + ভলিউম (Liquidity + Volume)

প্রফেশনাল ট্রেডার্স ভলিউম দেখবে।

***Bullish:***

```text
SSL Sweep

+

High Volume

=

Strong Buy Signal
```

***Bearish:***

```text
BSL Sweep

+

High Volume

=

Strong Sell Signal
```

## লিকুইডিটি পিরামিড (Liquidity Pyramid)

পেশাদার ট্রেডারের সিদ্ধান্ত প্রবাহ

```text
Trend
 ↓
Structure
 ↓
Supply/Demand
 ↓
Liquidity
 ↓
Sweep
 ↓
CHOCH
 ↓
BOS
 ↓
Entry
```

## ডিএসই ট্রেডার-এর সবচেয়ে বড় ভুল

* ***ভুল 1:*** Support Touch করেই Buy।

* ***ভুল 2:*** Resistance Break করলেই Buy।

* ***ভুল 3:*** Liquidity Sweep-এর জন্য অপেক্ষা না করা।

* ***ভুল 4:*** Equal High/Low Ignore করা।

* ***ভুল 5:*** Stop Loss obvious জায়গায় রাখা।

## প্রফেশনাল লিকুইডিটি চেকলিস্ট( Professional Liquidity Checklist)

চার্ট দেখার সময় নিজেকে জিজ্ঞাসা করুন:

* লিকুইডিটি কোথায়?

* Equal High আছে?

* Equal Low আছে?

* PDH / PDL কোথায়?

* Swing High / Low কোথায়?

* Sweep হয়েছে?

* CHOCH হয়েছে?

* BOS হয়েছে?

## পর্যায় ৬ শেষ হলে আপনার যা পারা উচিত

✅ Buy Side Liquidity (BSL) চিহ্নিত করা

✅ Sell Side Liquidity (SSL) চিহ্নিত করা

✅ Equal High (EQH) ও Equal Low (EQL) খুঁজে বের করা

✅ Liquidity Sweep ও Liquidity Grab আলাদা করা

✅ PDH, PDL এবং Swing Liquidity চিহ্নিত করা

✅ Inducement বুঝতে পারা

✅ Liquidity + Structure + Supply/Demand একসাথে বিশ্লেষণ করা

✅ High Probability Smart Money Entry খুঁজে বের করা

✅ Retail Trap Zone শনাক্ত করা

## পেশাদার ট্রেডিং কাঠামো (পর্যায় ১ → পর্যায় ৬)

```text
Market Structure
        +
Support & Resistance
        +
Supply & Demand
        +
Liquidity
        +
Volume
        +
Risk Management
        +
Psychology
        =
Professional Trading System
```

[০৭ - স্মার্ট মানি কনসেপ্ট (Smart Money Concept - SMC)](../০৭-স্মার্ট-মানি-কনসেপ্ট) থেকে আপনি স্মার্ট মানি-এর আসর এন্ট্রি মডেল শিখবেন। ইনস্টিটিউশনাল ফুটপ্রিন্ট পড়া এবং উচ্চ সম্ভাবনার ট্রেড সেটআপ করা শুরু করা হয়।

![Support Resistance](/_images/stock-market/05_SupplyDemand.png)

| পরের ধাপ | আগের ধাপ |
| -------- | ---------- |
| [০৭ - স্মার্ট মানি কনসেপ্ট (Smart Money Concept - SMC)](../০৭-স্মার্ট-মানি-কনসেপ্ট) | [০৫ - সাপ্লাই ও ডিমান্ড (Supply & Demand)](../০৫-সাপ্লাই-ডিমান্ড) |

>
> [!TIP]
>
> **🙏 অনুরোধ:** কোনো ভুল পেলে আপনি এই বিষয়বস্তুটি সংশোধন করতে পারেন। আপনার অবদান এই প্রজেক্টকে আরও সমৃদ্ধ করবে। ধন্যবাদ।
