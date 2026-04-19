# Assessment — Enterprise Portal: SQLi + Insecure Cookie + SSTI (3-Stage Chain)

---

## Question 1 — MCQ

**What SQL injection payload bypasses the login page in Stage 1?**

- A) `' OR 1=1`
- B) `admin"; --`
- C) **`admin' --`** ✅
- D) `UNION SELECT null,null --`

> **Answer:** C — `admin' --` closes the username string and comments out the password check via unsafe string concatenation in the backend SQL query.

---

## Question 2 — MCQ

**What encoding is used for the manipulable `enterprise_session` cookie?**

- A) URL encoding
- B) Hex encoding
- C) AES-256 encryption
- D) **Base64 encoding** ✅

> **Answer:** D — The cookie is a Base64-encoded JSON object with no cryptographic signature, allowing the `role` field to be freely modified and re-encoded.

---

## Question 3 — MCQ

**Which template engine is exploited in Stage 3 to achieve Remote Code Execution?**

- A) Twig
- B) Mustache
- C) Handlebars
- D) **Jinja2** ✅

> **Answer:** D — The admin email preview renders user input directly through Jinja2. The `{{ request.application.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read() }}` payload traverses Python internals to execute OS commands.

---

## Question 4 — Fill in the Blank

**What is the name of the session cookie that is Base64-decoded and modified to escalate privileges in Stage 2?**

**Answer:** `enterprise_session`

> This cookie holds a Base64-encoded JSON object containing `{"username": "admin", "role": "employee"}`. Because it has no cryptographic signature, the `role` value can be changed to `"admin"`, re-encoded, and used to access the admin panel.

---

## Question 5 — Fill in the Blank

**What server-side template engine renders user input in the admin Email Hub, enabling Remote Code Execution in Stage 3?**

**Answer:** `Jinja2`

> The admin panel's Custom Email Context field passes user input directly to the Jinja2 rendering engine without sandboxing. Jinja2 expressions have access to Python object internals, allowing an attacker to traverse to `os.popen()` and execute arbitrary OS commands.

---

*Lab target:* `http://localhost:5000`
