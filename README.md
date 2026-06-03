# CyberVault — Password Manager

A local, offline password manager encrypted with AES-256-GCM. No cloud, no account, no internet required.

---

## Installation

1. Download Python from [python.org/downloads](https://python.org/downloads)
2. During installation, check **"Add python.exe to PATH"**
3. Open CMD or PowerShell and run:

```
pip install cryptography
```

For 2FA support (optional):

```
pip install pyotp qrcode
```

4. Done.

---

## Starting CyberVault

You have two options to run the file:

Option 1 — Navigate to the folder first (easier):
Open CMD or PowerShell, go to the folder where `cybervault.py` is saved, then run:

```
cd C:\Users\YourName\Downloads
python cybervault.py
```

Option 2 — Enter the full path directly:

```
python C:\Users\YourName\Downloads\cybervault.py
```

CyberVault opens a menu and guides you through everything:

```
  What do you want to do?

  [1] Add a new entry
  [2] Get / show an entry
  [3] List all entries
  [4] Search entries
  [5] Delete an entry
  [6] Generate a password (without saving)
  [7] Change master password
  [0] Exit
```

Just type a number and press Enter.

---

## Master Password

The first time you start CyberVault, it will ask you to create a master password. This is the one password that protects everything inside the vault. Pick something you will remember — there is no way to recover it if you forget it.

**When typing a password, nothing appears on screen — no letters, no stars, nothing.** This is intentional for security. Just type normally and press Enter. It is being registered even though you see nothing.

---

## 2FA (Two-Factor Authentication) — New in V1.1

After creating your vault, CyberVault will ask if you want to enable 2FA. This is optional but strongly recommended.

If you choose yes, a QR code appears in the terminal. Scan it with an authenticator app like Google Authenticator or Authy. From that point on, every time you open CyberVault you will need to enter a 6-digit code from your app in addition to your master password.

If you skip 2FA during setup, you can enable it anytime by running init again.

**2FA requires the extra libraries:**

```
pip install pyotp qrcode
```

---

## Adding an Entry

Choose option 1 from the menu. CyberVault will ask you everything step by step:

```
  Name (e.g. GitHub, Netflix): GitHub
  Username / Email  (Enter to skip): mika@example.com
  URL / Website     (Enter to skip): github.com
  Notes             (Enter to skip):

  How do you want to set the password?
  [1] Generate a secure password automatically  (recommended)
  [2] Enter my own password
```

Fields marked with "Enter to skip" are optional.

---

## Getting an Entry

Choose option 2 from the menu. CyberVault shows you all saved entries and asks which one you want. Then you can choose to copy the password to your clipboard or show it on screen.

---

## Where Is My Data Stored?

The vault is a single encrypted file at:

- Windows: `C:\Users\YourName\.cybervault\vault.enc`
- Linux / Mac: `~/.cybervault/vault.enc`

The 2FA secret (if enabled) is stored at:

- Windows: `C:\Users\YourName\.cybervault\totp.key`
- Linux / Mac: `~/.cybervault/totp.key`

Both files are unreadable without your master password and 2FA code. Back them up by copying the entire `.cybervault` folder.

---

## How Secure Is It?

CyberVault uses the same encryption standard as governments and banks.

**AES-256-GCM** — Your passwords are encrypted with AES-256. GCM also verifies integrity, meaning if anyone tampers with the file, the vault will refuse to open.

**PBKDF2-SHA512 with 600,000 iterations** — Your master password is never stored. It is used to derive the encryption key through 600,000 rounds of hashing, which makes brute-force attacks extremely slow. An attacker with a powerful GPU would need centuries to guess a decent master password.

**2FA (TOTP)** — With 2FA enabled, even if someone steals your master password, they still cannot open the vault without the 6-digit code from your phone.

**Random salt** — Every time the vault is saved, a new random 32-byte salt is generated. Two vaults with the same master password produce completely different encrypted files.

**Cryptographically secure password generator** — Generated passwords use Python's `secrets` module, which pulls from the operating system's secure random source.

**No network** — CyberVault never connects to the internet. Nothing leaves your machine.

---

## Tips

- Use a master password that is long but memorable, for example a passphrase like `coffee-table-mountain-7`
- Back up your `.cybervault` folder to a USB stick regularly
- If you forget the master password, there is no recovery — the encryption is real
- Install `pyperclip` for clipboard support: `pip install pyperclip`

---

## Requirements

- Python 3.10 or newer
- `cryptography` library: `pip install cryptography`
- Optional 2FA: `pip install pyotp qrcode`
- Optional clipboard: `pip install pyperclip`

---

## Changelog

V1.0 — Initial release. AES-256-GCM vault, interactive menu, password generator, strength checker.

V1.1 — Added optional 2FA (TOTP) during vault setup. Scan a QR code with any authenticator app for a second layer of protection.
