# Week 4 — Mediroza General Hospital Penetration Test

**Target:** `https://medirozahospital.com`
**Type:** Black-box Penetration Test
**Duration:** 5 Days
**Batch:** B083 | Week 4
**Author:** Idowu Oluwapelumi

---

## 1. Executive Summary

Mediroza General Hospital commissioned a black-box penetration test of their public web infrastructure. The assessment identified **seven vulnerabilities**, three of which are rated **Critical**. An unauthenticated attacker was able to bypass authentication, access confidential patient records, crack their encryption, and ultimately download a full database backup containing staff salaries and shareholder details.

---

## 2. Scope and Methodology

**Target:** `https://medirozahospital.com`

**Approach:** Passive reconnaissance → Authentication testing → File encryption analysis → Metadata analysis → Server directory enumeration → Data extraction.

**Tools Used:**

| Tool | Purpose |
|---|---|
| Browser | Recon, manual testing, SQL injection |
| curl / PowerShell | Fetching robots.txt |
| Networkwalks Hash Calculator | Extract PDF hashes |
| Networkwalks Password Cracker | Crack PDF passwords |
| John the Ripper | Crack report 3 password |
| qpdf | Decrypt PDF files |
| exiftool | Read PDF metadata |
| Notepad++ | Analyze SQL backup file |
| ChatGPT | Format SQL data into readable tables |

---

## 3. Findings and Proof of Exploitation

### Finding 1 — Username Enumeration

**Location:** `https://medirozahospital.com/patient/login.php`
**Risk:** Medium

The login page returns different error messages for invalid usernames vs. invalid passwords. This allows an attacker to enumerate valid accounts.

**Evidence:**

<img width="420" height="400" alt="Fake User" src="https://github.com/user-attachments/assets/45bc872e-3463-4d35-8a05-77bb4db107c1" />


<img width="403" height="299" alt="admin_login" src="https://github.com/user-attachments/assets/479f3794-e6a9-467b-a615-67c30fe8bde7" />


When `bob` is entered, the response is `Username not found`. When `admin` is entered, the response is `Incorrect password`. This confirms `admin` is a valid account.

---

### Finding 2 — SQL Injection Login Bypass

**Location:** `https://medirozahospital.com/patient/login.php`
**Risk:** Critical

The username field is vulnerable to SQL injection. A single quote triggers a database error, and a crafted payload bypasses authentication entirely.

**Evidence:**

<img width="374" height="231" alt="sql_error" src="https://github.com/user-attachments/assets/94523a14-c525-4158-8c23-01970e55ee6c" />

Typing `admin'` in the username field produces a MySQL syntax error, confirming the input is unsanitised.

Using `admin' --` as the username and any password bypasses the login and grants access to the patient portal.

---

### Finding 3 — Encrypted PDFs Accessible After Login Bypass

**Location:** `https://medirozahospital.com/patient/reports/`
**Risk:** High

After bypassing the login, three confidential patient pathology reports were accessible and downloadable.

**Evidence:**

<img width="453" height="422" alt="pdfs evidence" src="https://github.com/user-attachments/assets/8ca54647-4cc8-486b-bbfb-88b7f16ec7b3" />

---

### Finding 4 — Weak PDF Passwords Crackable with a Wordlist

**Location:** `patient_report_1.pdf`, `patient_report_2.pdf`, `patient_report_3.pdf`
**Risk:** High

All three PDF files used weak password-based encryption. All passwords were recovered using common wordlists.

**Evidence:**

<img width="431" height="338" alt="pdf1_opened" src="https://github.com/user-attachments/assets/ca75c871-d98f-4039-a7f6-6679ab9a6f4a" />


<img width="425" height="313" alt="pdf2_open" src="https://github.com/user-attachments/assets/4be5d73c-ed2d-4c93-8be7-c6d7a6306e0e" />

<img width="437" height="329" alt="pdf3_opened" src="https://github.com/user-attachments/assets/19d819bb-0438-4686-84da-2c09ab64d3a7" />


| Report | Password | Method |
|---|---|---|
| patient_report_1.pdf | `123456` | Built-in wordlist |
| patient_report_2.pdf | `password` | Built-in wordlist |
| patient_report_3.pdf | `!@#$^&` | JTR wordlist |

---

### Finding 5 — Sensitive Metadata Left in Patient PDF Files

**Location:** `patient_report_3.pdf` metadata
**Risk:** Medium

The PDF metadata was not stripped before distribution. It reveals the author and a comment pointing to a hidden backup folder.

**Evidence:**

<img width="493" height="333" alt="Meta_data" src="https://github.com/user-attachments/assets/252a3b67-5f83-418c-a0cf-824a2a5d6f27" />


Key fields:

| Field | Value |
|---|---|
| Author | `j.malik` |
| Comments | `DB backup moved to /old before site migration, do not delete` |
| Creator | `Mediroza CMS 1.4.2` |

This metadata directly points to the exposed backup folder discovered next.

---

### Finding 6 — Forgotten Backup Folder with Directory Listing Enabled

**Location:** `https://medirozahospital.com/old/`
**Risk:** Critical

The `/old/` folder, referenced in both `robots.txt` and the PDF metadata, has directory listing enabled. A full database backup is publicly exposed.

**Evidence:**

<img width="383" height="201" alt="robot" src="https://github.com/user-attachments/assets/e6de8e87-b8f5-47a7-a789-be2475100963" />



The `robots.txt` file confirms `/old/` is a hidden folder. Visiting `/old/` shows the backup file `mediroza_db_backup_2019.sql`, which is downloadable without authentication.

---

### Finding 7 — Confidential Staff Salaries and Shareholder Data in Plain Text

**Location:** `https://medirozahospital.com/old/mediroza_db_backup_2019.sql`
**Risk:** Critical

The backup file contains the complete HR database in plain text, including staff salaries, contact details, national IDs, and shareholder ownership records.

**Evidence:**


Sample staff records:

| ID | Name | Job Title | Department | Monthly Salary (ZAR) |
|---|---|---|---|---|
| 1 | Dr. Rajesh Naidoo | Chief Pathologist | Diagnostics Lab | 138,000 |
| 3 | Dr. Johan van der Merwe | Medical Director | Management | 160,000 |
| 9 | Jameel Malik | IT Systems Administrator | IT | 58,000 |
| 30 | Andile Mbeki | Ward Clerk | Administration | 21,000 |


Sample shareholders records:

| ID | Shareholder Name | Share % | Share Class |
|---|---|---|---|
| 1 | Dr. Rajesh Naidoo | 18.00% | Ordinary |
| 2 | Cedar Health Holdings (Pty) Ltd | 15.00% | Ordinary |
| 4 | Reddy Family Trust | 11.00% | Ordinary |
| 10 | Dr. Vikram Chetty | 4.00% | Preferential |

**Trail Complete:**

<img width="855" height="95" alt="image" src="https://github.com/user-attachments/assets/a04a445f-768b-439f-94d6-0aab35353b22" />


Jameel Malik is listed in the staff table as the IT Systems Administrator. His email `j.malik` matches the Author field in the PDF metadata. He is the one who moved the backup to `/old` and left the note inside the report.

---

## 4. Risk Rating

| # | Vulnerability | Risk |
|---|---|---|
| 1 | Username enumeration | Medium |
| 2 | SQL injection login bypass | **Critical** |
| 3 | Encrypted PDFs accessible | High |
| 4 | Weak PDF passwords | High |
| 5 | Sensitive metadata in PDFs | Medium |
| 6 | Directory listing enabled | **Critical** |
| 7 | Database backup exposed | **Critical** |

---

## 5. Recommendations and Remediation

- **Username enumeration:** Show the same error message for wrong username and wrong password.
- **SQL injection:** Use parameterized queries. Never build SQL queries using raw user input.
- **PDF access control:** Store PDFs outside the web root or behind proper access controls. Use strong unique passwords per file.
- **PDF metadata:** Strip all metadata from patient files with `exiftool -all= filename.pdf`.
- **Directory listing and backup exposure:** Disable directory listing on all folders. Remove or relocate old backup files. Never store database backups in a public web folder.

---

## 6. Full Report

The complete penetration testing report is contained within this README. All evidence screenshots are embedded in Section 3 — Findings and Proof of Exploitation.

---

## Liability Disclaimer

All activities were performed only on systems where written permission was granted. This is for educational purposes only.
