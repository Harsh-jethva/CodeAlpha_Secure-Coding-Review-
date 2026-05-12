# Secure Coding Review Project
### Python Login Website - Vulnerability Analysis & Fixes
**College Mini Project | Beginner Friendly**

---

## Project Structure

```
secure_coding_review/
├── python/               # CLI Python demos
│   ├── login_vulnerable.py
│   └── login_secure.py
├── website/              # Flask website
│   ├── app.py
│   └── templates/
├── requirements.txt      # Python dependencies
├── .env.example          # Template for environment secrets
└── README.md             # This file
```

---

## Setup

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Create your .env file
cp .env.example .env
# Edit .env and set a real SECRET_KEY

# 3. Run the website from the project root
python website/app.py

# 4. Open the site in your browser
#    http://127.0.0.1:5000

# 5. Run the vulnerable CLI version (for comparison only)
python python/login_vulnerable.py

# 6. Run the secure CLI version
python python/login_secure.py

# 7. Run Bandit static analysis on the vulnerable file
bandit -r python/login_vulnerable.py
```

---

## Vulnerabilities Covered

| # | Vulnerability              | Severity | Fix Applied                     |
|---|----------------------------|----------|---------------------------------|
| 1 | SQL Injection               | HIGH     | Parameterized queries (`?`)     |
| 2 | Plain-text Password Storage | HIGH     | bcrypt hashing                  |
| 3 | Weak MD5 Hashing            | HIGH     | bcrypt.checkpw()                |
| 4 | Hardcoded Secrets           | MEDIUM   | os.getenv() + .env file         |
| 5 | Verbose Error Leakage       | LOW      | logging module + generic message |
| 8 | Sensitive Data Logging      | MEDIUM   | Avoid logging passwords/tokens   |

---

## Web Demo

The website has two login routes:

- `/vulnerable` shows the insecure version with SQL injection, plain-text storage, MD5 hashing, verbose errors, and sensitive logging.
- `/secure` shows the fixed version with parameterized queries, bcrypt, validation, and safer error handling.

The homepage at `/` links to both demos.

## How the website runs

The Flask app lives in `website/app.py`. Run it from the project root with `python website/app.py`, and Flask starts a local development server on `http://127.0.0.1:5000`.

Use the browser to open:

- `/` for the landing page
- `/vulnerable` for the insecure demo
- `/secure` for the fixed demo

Stop the server with `Ctrl+C` in the terminal.

## SQL Injection Demo (on vulnerable version only)

When running `login_vulnerable.py`, try this input:
```
Username: admin' OR '1'='1' --
Password: anything
```
This bypasses the login entirely - proving the vulnerability is real.
The secure version is immune to this attack.

---

## Important Notes

- `login_vulnerable.py` is for **educational purposes only**
- Never use MD5 for password hashing in real applications
- Always add `.env` to your `.gitignore`
- Run `bandit -r .` regularly in your projects
