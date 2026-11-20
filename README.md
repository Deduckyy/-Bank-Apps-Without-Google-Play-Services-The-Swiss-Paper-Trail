# Bank Apps Without Google Play Services — The Swiss Paper Trail

> Can you use **Swiss banking and payment apps** on a de-googled Android phone? Spoiler: most banks say no. This repo documents one full case — from first support chat to formal complaint — collects evidence, and aims to become a **public comparison of which Swiss banks actually work without mandatory Google services**.

**Author:** deduckyy ([github.com/deduckyy](https://github.com/deduckyy)) · green-hat-in-training, studying cybersecurity (PortSwigger WSA), documenting the journey.

**Case study #1:** TWINT / PostFinance · **Status:** ⏳ Formal complaint sent **20.11.2026**, acknowledged same day, deadline **30.11.2026** → then Swiss Banking Ombudsman.

---

## 🎯 Why this repo exists

De-googled Android is a growing reality: privacy-conscious users, Fairphones with custom OS, microG setups, /e/OS, GrapheneOS, CalyxOS. Yet in Switzerland, using your own money from your own phone increasingly **requires a Google account**:

- **TWINT** (the de facto national payment standard) → Google Play / App Store only
- Most **e-banking apps** → same story
- The justification is always the same: *"security"*

This repo challenges that justification with a documented case, a technical argument, and a formal complaint — and invites everyone to replicate it with their own bank.

## 📖 Case study #1: TWINT on PostFinance (in 30 seconds)

I run a **Fairphone without Google services** — no Google account, no Play Store, by conscious choice. TWINT is *the* payment app in Switzerland: split bills, pay at stores, parking meters, vending machines. But PostFinance distributes the TWINT app **only** via Google Play and Apple's App Store.

That leaves users like me exactly two options:

1. **Create a Google account anyway** (defeating the point of a de-googled phone), or
2. **Sidestep to unofficial APK mirrors** — for a *payments* app. Counterfeit APK = fraudster's dream. The bank's own distribution policy **creates the attack surface**.

And no, "but it's from the official store!" is not a security model. Keep reading.

## 🔐 The technical argument: "store-only" ≠ "secure"

**XcodeGhost (2015)** is the canonical counter-example:

- Chinese developers downloaded a **tampered copy of Xcode** (Apple's official dev tool) from fast third-party mirrors, without verifying the signature (Apple's servers were slow).
- The poisoned compiler silently injected malicious payload into apps built by **honest, legitimate developers**.
- Result: **4,000+ infected apps passed Apple's App Store review** — all of them *correctly code-signed*. Including mega-apps like WeChat.
- The single mandatory distribution channel provided **zero protection** against a supply-chain compromise upstream.

Lessons for a payments app:

- A single mandatory channel is a **single point of failure** for both availability and integrity.
- Real resilience = **independently verifiable artifacts**: direct download of a signed package from the bank's own website, published checksums, alternative recognized channels (e.g. AppGallery, F-Droid).
- Pushing no-Google users toward unverified mirrors is a **self-inflicted attack surface** — especially dangerous for a payment app that fraudsters already love to counterfeit.

**And here's the irony:** PostFinance itself **digitally signs its own customer e-mails** (each message carries a signature notice pointing to postfinance.ch/e-signature). The institution clearly understands the value of signed delivery and independent verifiability — it applies it where convenient, but not to its payment app's distribution. The concept isn't alien to them. That's what makes this policy hard to defend.

## ⚖️ The legal angle (Switzerland)

The revised **Federal Act on Data Protection (nFADP)**, in force since September 2023, requires appropriate **technical and organizational measures** for the protection of personal data (Art. 8). A payments app processes the hottest personal data there is — financial transactions. A distribution strategy that actively channels part of its user base toward unverifiable sources deserves scrutiny under exactly this duty.

(PostFinance is also, let's remember, the former state postal bank — public-service expectations apply.)

## 🗓️ Timeline — Case #1: TWINT / PostFinance

| Date | Event |
|------|-------|
| **11.11.2026, 13:54** | Opened support chat; asked for a contact e-mail. Got offered — among general phone numbers — the *data protection* address as one of the few written channels. |
| **11.11.2026, 13:57–13:58** | Explained the issue: de-googled Fairphone, Play Services requirement blocks app usage. Response: app requires Google Play or App Store; rooted devices unsupported since v4.8.0. Offered phone numbers. |
| **11.11.2026, 14:22–14:25** | Asked to speak with a human; filed a **formal customer complaint** via the chat, requesting a written answer through e-finance Messages. Transfer to Customer Center confirmed (answer promised within 2 business days). Full log: [`evidence/case1-postfinance/chat-log-2026-11-11.txt`](evidence/) |
| **12.11.2026, 16:34** | Case picked up by a Customer Center agent (e-finance). |
| **[~mid Nov 2026]** | Written answer received: *"TWINT PostFinance currently supports installation only via Google Play and Apple Store. We will forward your feedback internally."* The Swiss equivalent of `/dev/null`. 📥 [`evidence/`](evidence/) |
| **19.11.2026** | Follow-up written request bounced by the **data protection autoresponder**: "this address is exclusively for data protection rights, for anything else call us." |
| **20.11.2026, 8:26–8:28** | Phone call (private customers line). Agent confirmed: **another customer had already filed an identical complaint**; could not address technical aspects; offered no alternative written channel beyond "redo what you already did". |
| **20.11.2026** | **Final formal letter** sent to `twint@postfinance.ch`: full history, technical argument, 3 explicit questions, **10-day deadline**, announcement of escalation to the Swiss Banking Ombudsman. 📤 [`letters/`](letters/) |
| **20.11.2026** | **Automated acknowledgment** received: "we will contact you in writing or by phone as soon as possible." Notably: the e-mail itself is **digitally signed by PostFinance** — see the irony section above. 📥 [`evidence/`](evidence/) |
| **30.11.2026** | Deadline expires. Silence → Ombudsman. |

## ❓ The three questions PostFinance has refused to answer

1. Does an **official, secure path** exist (or is one planned) to install and update the TWINT app on Android without a Google account and without Google services?
2. What measures protect customers who, as a direct consequence of the current distribution policy, may resort to **unofficial sources (counterfeit APKs)**?
3. Is this limitation **final**, or under internal evaluation?

Four channels (chat, e-mail, e-finance, phone), zero substantive answers. Not "no" — worse: **no answer to the actual questions**.

## 📊 Swiss banks comparison — *work in progress*

The goal: a public table of Swiss banks/payment providers and whether their apps work **without Google Play Services**. Early signals (to verify — contributions welcome!):

| Bank / Provider | Play Store | Alternative channel? | Works without Play Services? |
|---|---|---|---|
| PostFinance (TWINT) | ✅ | ❌ none found | ❌ (case study #1) |
| neon | ✅ | AppGallery (reported) | ❓ to verify |
| *your bank here* | ? | ? | ? |

Open a PR or an issue with your findings — one row per bank, source required (screenshot, official page, or documented test).

## 🔁 Replicate this with your bank

All my correspondence is in [`templates/`](templates/) — Italian originals, English translations welcome:

- `01-initial-request.md`
- `02-followup-security-argument.md` (the XcodeGhost + nFADP letter)
- `03-final-formal-letter.md` (deadline + ombudsman announcement)

Feel free to open a PR with translations (DE/FR/EN) or adaptations for your bank. If we're several asking the same question, it stops being "one difficult customer" and becomes a **documented pattern**.

## 🛣️ Roadmap

- [x] Exhaust customer channels (e-mail, chat, phone)
- [x] Final formal letter with deadline
- [x] Automated acknowledgment received
- [ ] Response (or silence) — 30.11.2026
- [ ] Swiss Banking Ombudsman filing (free, neutral mediation; requires prior attempts with the institution ✅)
- [ ] Outcome documentation
- [ ] Build the Swiss banks comparison table (contributions welcome)
- [ ] Template translations (EN/DE/FR)

## 🦆 Why document this publicly?

Because "feedback will be forwarded internally" should not be where feedback dies. Transparency is the polite version of accountability. And because if you're reading this from a de-googled phone, you already know why this matters.

*Legal note: all evidence is redacted of personal/customer identifiers. Names of customer-service agents appear only in their capacity as official representatives answering a customer request.*
