# Week 3 — Password Cracking with JTR & Networkwalks Tools

**Batch:** B083 | Week 3
**Author:** Idowu Oluwapelumi

---

## 1. Executive Summary

This week focused on password cracking — recovering passwords from protected files to demonstrate why weak passwords are a critical security risk.

Two labs were completed:
1. **Lab 1:** Crack a password-protected PDF using **John the Ripper (JTR)** and **Johnny GUI**
2. **Lab 2:** Crack the same PDF using **Networkwalks Hash Calculator** and **Networkwalks Password Cracker**

**Files cracked:**
- `My Locked PDF1.pdf` → password `password1`
- `My Locked PDF2.pdf` → password `good-luck`
- `My Locked PDF3.pdf` → password `1qaz2wsx`

**Flag captured:** `nw{cybersecurity_flag_captured_2608}`

---

## 2. Scope and Methodology

### 2.1 Objective
Demonstrate how password-protected files are cracked and why strong passwords are essential.

### 2.2 Tools Used

| Tool | Purpose |
|---|---|
| John the Ripper (JTR) CLI | Crack PDF hashes using wordlists |
| Johnny GUI | Graphical interface for JTR |
| Networkwalks Hash Calculator | Extract `$pdf$` hashes from PDFs |
| Networkwalks Password Cracker | Crack hashes via dictionary attack |
| OnlineHashCrack PDF Extractor | Alternative hash extraction |

### 2.3 Methodology
1. Download and install JTR + Johnny on Windows
2. Extract the PDF hash using OnlineHashCrack or NW Hash Calculator
3. Load the hash into Johnny or NW Password Cracker
4. Run the attack → recover the password
5. Open the PDF with the cracked password

---

## 3. Findings and Proof of Exploitation

### Lab 1 — Password Cracking with JTR & Johnny

#### Step 1: Configure Johnny — Point to JTR Executable

In Johnny: **Settings** → **Browse** → selected `john.exe` from the JTR folder.

<img width="440" height="342" alt="Capture" src="https://github.com/user-attachments/assets/026b4dc2-0f42-4ca1-8a5a-9b6d07611d0b" />


Johnny detected `John the Ripper 1.9.0-jumbo-1` and was ready to run attacks.

#### Step 2: Extract the PDF Hash

Opened OnlineHashCrack → uploaded `My Locked PDF3.pdf` → extracted the `$pdf$` hash.

![OnlineHashCrack Extract]
<img width="476" height="453" alt="hash_file" src="https://github.com/user-attachments/assets/91c17972-e652-4b95-b962-5f7364dc95ff" />


The extracted hash started with `$pdf$4*4*128*-1028*1*16*...`

#### Step 3: Crack the PDF with NW Password Cracker

Pasted the hash into Networkwalks Password Cracker → ran dictionary attack.

<img width="516" height="500" alt="Capture2" src="https://github.com/user-attachments/assets/277c80e0-74db-492a-b81c-f60d9025273e" />


**Cracked password for PDF3:** `1qaz2wsx`

#### Step 4: Crack the PDF with Johnny

Loaded `My Locked PDF1.pdf` hash into Johnny → ran attack.

<img width="438" height="351" alt="Cracked_Password" src="https://github.com/user-attachments/assets/c0d9e8e7-66e9-40b8-8015-63baf75ef421" />


**Cracked password for PDF1:** `good-luck`

#### Step 5: Open the PDF — Flag Captured

Entered the cracked password → PDF unlocked → flag revealed.



**Flag:** `nw{cybersecurity_flag_captured_2608}`

---

### Lab 2 — Password Cracking with Networkwalks Tools

#### Step 1: Extract PDF Hash (PDF2)

Uploaded `My Locked PDF2.pdf` to Networkwalks Hash Calculator → extracted the `$pdf$` hash.

![Hash Calculator PDF2]
<img width="453" height="425" alt="networks_cracking_tool" src="https://github.com/user-attachments/assets/76c15593-7e07-4e1d-97f5-576f049989f5" />


#### Step 2: Crack PDF1 with NW Password Cracker

Pasted the PDF1 hash into NW Password Cracker → ran dictionary attack.

![NW Cracker — password1]
<img width="452" height="451" alt="password_cracked" src="https://github.com/user-attachments/assets/45dc3423-324a-4805-9033-4e418ac98ef5" />

<img width="525" height="734" alt="image" src="https://github.com/user-attachments/assets/9e141ebe-dbe5-4266-bf5f-7538a23663e2" />



**Cracked password:** `password1`

#### Step 4: Open the Locked PDF

Opened `My Locked PDF1.pdf` → entered `password1`.

![PDF Password Prompt](02_NW_Tools/09_pdf_password_prompt_screenshot.png)

#### Step 5: PDF Unlocked

PDF opened successfully → lab complete.





---

## 4. Risk Rating

| # | Risk | Impact | Level |
|---|---|---|---|
| 1 | Weak PDF passwords (`password1`, `good-luck`, `1qaz2wsx`) | All 3 PDFs cracked in seconds using common wordlists | High |
| 2 | Reliance on weak password-based encryption | Encryption offers no real protection against dictionary attacks | High |
| 3 | Sensitive files distributed with weak passwords | If file leaks, contents are trivially exposed | High |

---

## 5. Recommendations and Remediation

- **Use strong, unique passwords** — minimum 12 characters with mixed case, numbers, and symbols.
- **Never use common words** like `password`, `123456`, or keyboard patterns like `1qaz2wsx`.
- **Use AES-256 encryption** for sensitive files, not just weak PDF encryption.
- **Store sensitive files outside the web root** and behind proper access controls.
- **Implement multi-factor authentication** for accessing sensitive documents.
- **Educate staff** on password hygiene.
- **Regularly audit** for weak passwords across the organization.

---

## 6. Key Takeaway

**Encryption is only as strong as the password protecting it.** All 3 PDFs were cracked in seconds using free, publicly available tools because they used weak, dictionary-crackable passwords. Strong passwords and layered security are essential.

---

## Liability Disclaimer

All activities were performed only on systems where written permission was granted. This is for educational purposes only.

---

## Author

**Idowu Oluwapelumi**
Week 3 Project — Networkwalks Cybersecurity Program

---

## Contact

- Email: idowuoluwapelumi021@gmail.com
- GitHub: [Idowu-Oluwapelumi-Emmanuel](https://github.com/Idowu-Oluwapelumi-Emmanuel)
