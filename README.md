<div align="center">

# 🔓 Password Cracking Lab

**Recovering weak passwords from locked PDFs with John the Ripper and browser-based tools**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-red)
![Tool](https://img.shields.io/badge/Tool-John%20the%20Ripper-black)
![OS](https://img.shields.io/badge/OS-Kali%20Linux-blue)
![Technique](https://img.shields.io/badge/Technique-Dictionary%20Attack-orange)
![Skill](https://img.shields.io/badge/Skill-Ethical%20Hacking-darkgreen)
![NetworkWalks](https://img.shields.io/badge/Program-NetworkWalks-red)

</div>

---

## 📌 Project Overview

This is my **Week 3 project (Modules 1 and 2)** for the NetworkWalks Cybersecurity Internship.

The goal is to recover the password of a protected PDF in a controlled lab, using two different methods against the same file, and to understand **why weak passwords fail so quickly**.

| Module | Method | Where it runs |
|---|---|---|
| **1** | John the Ripper (JTR) | Command line on Kali Linux |
| **2** | NetworkWalks Hash Calculator + Password Cracker | In the browser, nothing to install |

Both methods extract the PDF's `$pdf$...` hash and recover the **same password**.

## ⚠️ Authorization and Scope

All testing was done **only on training files provided by the NetworkWalks lab exercise**, inside my own isolated Kali Linux virtual machine. No third-party files, systems or accounts were involved. This repo is for education and defensive learning only.

> Lab flags and recovered passwords are partly masked in this README so the exercise answers are not handed out to others.

---

## 🧰 Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux (VM) | Lab operating system |
| `pdf2john` | Extracts a crackable hash from a protected PDF |
| John the Ripper (`john`) | Dictionary attack against the extracted hash |
| NetworkWalks Hash Calculator | Browser-based hash extraction (parsed locally, no upload) |
| NetworkWalks Password Cracker | Browser-based dictionary attack (built-in 100-password list) |

Links: [Hash Calculator](https://networkwalks.com/hash-calculator/) · [Password Cracker](https://networkwalks.com/password-cracker/)

---

## 🧪 Module 1: Password Cracking with John the Ripper

John the Ripper comes pre-installed on Kali Linux, so I ran it from the command line instead of using the Windows/Johnny GUI route from the lab handout.

### Step 1: Extract the hash from the locked PDF

```bash
pdf2john "My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf" > hash1.txt
```

> 📸 `screenshots/01-pdf2john-extract.png`

### Step 2: Run John against the hash

```bash
john hash1.txt
```

John first tries its `single` mode, then moves on to its default wordlist (`/usr/share/john/password.lst`).

> 📸 `screenshots/02-john-run.png`

### Step 3: Display the cracked result

```bash
john --show hash1.txt
```

```
Downloads/My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf:p*******1

1 password hash cracked, 0 left
```

> 📸 `screenshots/03-john-show.png`

**Result:** cracked almost instantly. The password sits early in John's default wordlist because it is a well-known common password.

---

## 🌐 Module 2: Password Cracking with NetworkWalks Tools

### Step 1: Extract the hash with the Hash Calculator

I loaded the locked PDF into the Hash Calculator. It returned the `$pdf$...` hash and parsed the file locally in the browser (no upload).

> 📸 `screenshots/04-hash-calculator.png`

### Step 2: Crack it with the Password Cracker

I pasted the hash into the Password Cracker and started the dictionary attack. The tool matched the password after trying **91 of its 100 built-in candidates**.

> 📸 `screenshots/05-password-cracker.png`

### Step 3: Verify

I opened the PDF with the recovered password and captured the lab's flag.

> 📸 `screenshots/06-pdf-opened-and-flag.png`

**Result:** the same password as Module 1, found by a completely different tool.

---

## ➕ Additional Practice: Three More Locked PDFs

Using the same `pdf2john` + `john` workflow, I extracted and cracked three more locked PDFs from the lab's flag-capture exercises. Each was cracked in under a second against John's default wordlist.

| File | Hash file | Recovered password |
|---|---|---|
| My Locked PDF1.pdf | `hash2.txt` | `g*******k` |
| My Locked PDF2.pdf | `hash3.txt` | `p*******1` |
| My Locked PDF3.pdf | `hash4.txt` | `1******x` |

Full terminal output: [`evidence/jtr-terminal-log.txt`](evidence/jtr-terminal-log.txt)

> 📸 `screenshots/07-extended-jtr-practice.png`

### 🚩 Flags Captured

I captured **4 flags** from the NetworkWalks flag-capture system after entering each recovered password. The flag values are not shown here on purpose.

> 📸 `screenshots/08-flag-confirmations.png`

---

## 📊 Results Summary

| Test | Tool | Time to crack | Wordlist |
|---|---|---|---|
| Module 1 | John the Ripper | < 1 second | John default `password.lst` |
| Module 2 | NetworkWalks Cracker | Instant (91 / 100 tries) | Built-in 100-word list |
| Extra PDF 1 | John the Ripper | < 1 second | John default `password.lst` |
| Extra PDF 2 | John the Ripper | < 1 second | John default `password.lst` |
| Extra PDF 3 | John the Ripper | < 1 second | John default `password.lst` |

---

## 💡 What I Learned

- **The tool was not the strong part, the password was the weak part.** Both a command-line tool and a simple browser tool succeeded in well under a second, because the password exists in common-password lists.
- **Protected does not mean safe from offline attacks.** The PDF's password is checked against a derived hash, but that hash can be extracted and attacked offline, with no rate limiting or lockout to slow an attacker down.
- **Dictionary attacks beat "dictionary word + a digit".** Patterns like `word1`, keyboard-pattern passwords and phrases with a hyphen all sit near the top of common wordlists.
- **Strong passwords resist this approach.** A long, random, non-dictionary passphrase would never appear in these wordlists, which forces a far slower brute-force search.

## 🛡️ Defensive Takeaways

1. Use long passphrases (a random multi-word phrase beats a short complex one).
2. Never reuse a password across files or accounts.
3. Use a password manager to generate and store unique passwords.
4. Don't rely on a PDF password alone for sensitive documents. Use proper file or disk encryption with a strong passphrase.
5. Check passwords against known-breached or common lists before using them.

---

## 🛠️ Troubleshooting Notes

| Problem | Fix |
|---|---|
| `pdf2john` fails on file names with spaces or brackets | Wrap the file name in quotes, or escape each space with a backslash (`My\ Locked\ PDF2.pdf`) |
| `john` finishes but shows nothing useful | Run `john --show --format=PDF hashfile.txt` to list cracked passwords reliably |

---

## 👤 Author

**Jahmir Vargas**, Cybersecurity Intern, NetworkWalks (Batch B0XX)
Instructor: Waqas Karim (CCIE)

`#Networkwalks` `#Cybersecurity` `#EthicalHacking` `#KaliLinux` `#PasswordSecurity`
