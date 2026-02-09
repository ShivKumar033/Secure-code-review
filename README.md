# Automated Source Code Review Using Tools (SAST)

- SAST tools often show many false alerts, so do not trust them fully.
- Run SAST tools first to get hints about risky code.
- Use the results to know **where to look**, not **what to report**.
- Manually review the code to confirm real vulnerabilities.
- Using both SAST and manual review saves time and avoids false reports.

###  Common SAST Tools (Short List)

- **Semgrep** – pattern-based, fast, customizable
- **CodeQL** – deep data-flow analysis (GitHub)
- **SonarQube** – code quality + security
- **Snyk Code** – developer-friendly SAST
- **Checkmarx** – enterprise SAST
- **Fortify** – enterprise static analysis
- **Bandit** – Python-focused SAST
- **ESLint Security Plugins** – JS security rules

# Manual Source Code Review

## 1️⃣ Recon & Scope Understanding

**Goal:** Don’t read everything. Find where attacks start.

### ✅  Checklist

☐ Identify application type (Web / API / Microservice)
☐ Identify tech stack (Java / Node / Python / PHP) 
☐ Locate entry folders (controllers, routes, handlers)
☐ Identify auth mechanism (session / JWT / OAuth)
☐ Identify DB type & ORM
☐ Identify external integrations (payments, emails, webhooks)

---

## 2️⃣ Attack Surface Mapping (MOST IMPORTANT)

**Goal:** Find what an attacker can reach.

### ✅ Checklist

☐ List all HTTP endpoints    
☐ Identify HTTP methods (GET/POST/PUT/DELETE)
☐ Identify unauthenticated endpoints
☐ Identify admin-only endpoints
☐ Identify file upload / download endpoints
☐ Identify background jobs / webhooks
☐ Identify GraphQL resolvers (if any)

---

## 3️⃣ User Input Identification (Sources)

**Goal:** Mark all attacker-controlled data.

### ✅ Checklist

☐ URL parameters    
☐ Query parameters
☐ Request body (JSON / form)
☐ Headers (Authorization, X-* headers)
☐ Check Cookies flag like `SameSite`, `Secure`, `HttpOnly` 
☐ JWT claims 
☐ File uploads
☐ WebSocket messages
☐ Mobile app parameters

---

## 4️⃣ Data Flow Tracking (Source → Sink)

**Goal:** Understand how input travels.

### ✅ Checklist

☐ Track input from controller to service    
☐ Track service to DAO / repository
☐ Track input passed to templates
☐ Track async jobs / queues
☐ Track cache usage
☐ Track cross-service calls
☐ Track reuse of same variable elsewhere

---

## 5️⃣ Dangerous Sink Identification

**Goal:** Find places where input causes impact.

### ✅ Checklist

**Injection Sinks**

☐ SQL queries (raw / string concat)    
☐ NoSQL queries
☐ LDAP queries
☐ OS command execution
☐ Deserialization

**Web Sinks**

☐ HTML rendering (XSS)
☐ JS execution
☐ File read/write
☐ URL fetching (SSRF)
☐ Redirects

---

## 6️⃣ Input Validation Review

**Goal:** Ensure validation is real and enforced.

### ✅ Checklist

☐ Server-side validation exists
☐ Type validation (int, string, enum)
☐ Length limits enforced
☐ Whitelist validation (not blacklist)
☐ Validation happens BEFORE sink
☐ Validation applied on ALL code paths
☐ Validation reusable (not duplicated)

---

## 7️⃣ Sanitization Review (Context-Aware)

**Goal:** Check output safety.

### ✅ Checklist

- ☐ SQL uses parameterized queries
- ☐ HTML output encoded
- ☐ JS context properly escaped
- ☐ File paths normalized
- ☐ URL encoding where needed    
- ☐ No custom “sanitize everything” functions

---

## 8️⃣ Authentication Review

**Goal:** Prevent account takeover.

### ✅ Checklist

☐ Password hashing (bcrypt / argon2)
☐ No hardcoded credentials
☐ Secure password reset flow
☐ Token randomness & expiry
☐ MFA logic enforcement
☐ Session regeneration on login
☐ Logout invalidates session/token    

---

## 9️⃣ Authorization Review (IDOR ZONE 🔥)

**Goal:** Ensure users only access their data.

### ✅ Checklist

☐ Object ownership verified
☐ User ID not trusted from request
☐ Role checks server-side
☐ Admin routes protected
☐ Nested object access validated
☐ No client-side auth decisions
☐ AuthZ enforced on every access

---

## 🔟 Business Logic Review

**Goal:** Break assumptions.

### ✅ Checklist

☐ Price / quantity not client-controlled
☐ Workflow steps enforced
☐ State transitions validated 
☐ Replay attacks prevented
☐ Coupon / token single-use
☐ Race conditions handled
☐ Order/payment consistency checks
 
---

## 1️⃣1️⃣ CSRF & State Change Review

**Goal:** Prevent cross-site abuse.

### ✅ Checklist

☐ CSRF tokens present
☐ Token validated server-side
☐ Tokens tied to session
☐ No state-changing GET requests
☐ SameSite cookie protection
☐ APIs protected where required

---

## 1️⃣2️⃣ File Handling Review

**Goal:** Prevent RCE & data leaks.

### ✅ Checklist

☐ File type validation (MIME + extension)
☐ File size limits
☐ Upload directory non-executable
☐ Path traversal protection
☐ SVG / HTML uploads blocked or sanitized
☐ Download authorization checks    

---

## 1️⃣3️⃣ Error Handling & Logging

**Goal:** Avoid information leakage.

### ✅ Checklist

☐ No stack traces to users
☐ No sensitive data in logs
☐ Debug mode disabled in prod
☐ Proper error messages
☐ No silent failures (fail-open)
☐ Log injection prevented    

---

## 1️⃣4️⃣ Configuration & Secrets

**Goal:** Avoid easy wins for attackers.

### ✅ Checklist

☐ No hardcoded secrets
☐ Secure environment variable usage
☐ Secure CORS configuration
☐ Security headers enabled
☐ Test/debug endpoints removed
☐ Feature flags secured    

---

## 1️⃣5️⃣ Dependency & Supply Chain Review

**Goal:** Prevent inherited vulnerabilities.

### ✅ Checklist

☐ Outdated dependencies
☐ Known CVEs
☐ Insecure deserialization libs
☐ Dependency confusion risk
☐ Excessive permissions    

---

## 1️⃣6️⃣ Error Paths & Edge Cases (PRO LEVEL)

**Goal:** Find hidden bugs.

### ✅ Checklist

☐ Catch blocks reviewed
☐ Fallback logic reviewed
☐ Null handling checked
☐ Default values safe
☐ Fail-closed behavior

---

## 1️⃣7️⃣ Vulnerability Chaining

**Goal:** Increase impact.

### ✅ Checklist

☐ Can this bug expose sensitive data?
☐ Can exposed data be reused?
☐ Can bug lead to auth bypass?
☐ Can bug escalate privilege?    
☐ Can multiple low bugs chain?# Automated Source Code Review Using Tools (SAST)

- SAST tools often show many false alerts, so do not trust them fully.
- Run SAST tools first to get hints about risky code.
- Use the results to know **where to look**, not **what to report**.
- Manually review the code to confirm real vulnerabilities.
- Using both SAST and manual review saves time and avoids false reports.

###  Common SAST Tools (Short List)

- **Semgrep** – pattern-based, fast, customizable
- **CodeQL** – deep data-flow analysis (GitHub)
- **SonarQube** – code quality + security
- **Snyk Code** – developer-friendly SAST
- **Checkmarx** – enterprise SAST
- **Fortify** – enterprise static analysis
- **Bandit** – Python-focused SAST
- **ESLint Security Plugins** – JS security rules

# Manual Source Code Review

## 1️⃣ Recon & Scope Understanding

**Goal:** Don’t read everything. Find where attacks start.

### ✅  Checklist

- ☐ Identify application type (Web / API / Microservice)
- ☐ Identify tech stack (Java / Node / Python / PHP) 
- ☐ Locate entry folders (controllers, routes, handlers)
- ☐ Identify auth mechanism (session / JWT / OAuth)
- ☐ Identify DB type & ORM
- ☐ Identify external integrations (payments, emails, webhooks)

---

## 2️⃣ Attack Surface Mapping (MOST IMPORTANT)

**Goal:** Find what an attacker can reach.

### ✅ Checklist

- ☐ List all HTTP endpoints    
- ☐ Identify HTTP methods (GET/POST/PUT/DELETE)
- ☐ Identify unauthenticated endpoints
- ☐ Identify admin-only endpoints
- ☐ Identify file upload / download endpoints
- ☐ Identify background jobs / webhooks
- ☐ Identify GraphQL resolvers (if any)

---

## 3️⃣ User Input Identification (Sources)

**Goal:** Mark all attacker-controlled data.

### ✅ Checklist

- ☐ URL parameters    
- ☐ Query parameters
- ☐ Request body (JSON / form)
- ☐ Headers (Authorization, X-* headers)
- ☐ Check Cookies flag like `SameSite`, `Secure`, `HttpOnly` 
- ☐ JWT claims 
- ☐ File uploads
- ☐ WebSocket messages
- ☐ Mobile app parameters

---

## 4️⃣ Data Flow Tracking (Source → Sink)

**Goal:** Understand how input travels.

### ✅ Checklist

- ☐ Track input from controller to service    
- ☐ Track service to DAO / repository
- ☐ Track input passed to templates
- ☐ Track async jobs / queues
- ☐ Track cache usage
- ☐ Track cross-service calls
- ☐ Track reuse of same variable elsewhere

---

## 5️⃣ Dangerous Sink Identification

**Goal:** Find places where input causes impact.

### ✅ Checklist

**Injection Sinks**

- ☐ SQL queries (raw / string concat)    
- ☐ NoSQL queries
- ☐ LDAP queries
- ☐ OS command execution
- ☐ Deserialization

**Web Sinks**

- ☐ HTML rendering (XSS)
- ☐ JS execution
- ☐ File read/write
- ☐ URL fetching (SSRF)
- ☐ Redirects

---

## 6️⃣ Input Validation Review

**Goal:** Ensure validation is real and enforced.

### ✅ Checklist

- ☐ Server-side validation exists
- ☐ Type validation (int, string, enum)
- ☐ Length limits enforced
- ☐ Whitelist validation (not blacklist)
- ☐ Validation happens BEFORE sink
- ☐ Validation applied on ALL code paths
- ☐ Validation reusable (not duplicated)

---

## 7️⃣ Sanitization Review (Context-Aware)

**Goal:** Check output safety.

### ✅ Checklist

- ☐ SQL uses parameterized queries
- ☐ HTML output encoded
- ☐ JS context properly escaped
- ☐ File paths normalized
- ☐ URL encoding where needed    
- ☐ No custom “sanitize everything” functions

---

## 8️⃣ Authentication Review

**Goal:** Prevent account takeover.

### ✅ Checklist

- ☐ Password hashing (bcrypt / argon2)
- ☐ No hardcoded credentials
- ☐ Secure password reset flow
- ☐ Token randomness & expiry
- ☐ MFA logic enforcement
- ☐ Session regeneration on login
- ☐ Logout invalidates session/token    

---

## 9️⃣ Authorization Review (IDOR ZONE 🔥)

**Goal:** Ensure users only access their data.

### ✅ Checklist

- ☐ Object ownership verified
- ☐ User ID not trusted from request
- ☐ Role checks server-side
- ☐ Admin routes protected
- ☐ Nested object access validated
- ☐ No client-side auth decisions
- ☐ AuthZ enforced on every access

---

## 🔟 Business Logic Review

**Goal:** Break assumptions.

### ✅ Checklist

- ☐ Price / quantity not client-controlled
- ☐ Workflow steps enforced
- ☐ State transitions validated
- ☐ Replay attacks prevented
- ☐ Coupon / token single-use
- ☐ Race conditions handled
- ☐ Order/payment consistency checks
 
---

## 1️⃣1️⃣ CSRF & State Change Review

**Goal:** Prevent cross-site abuse.

### ✅ Checklist

- ☐ CSRF tokens present
- ☐ Token validated server-side
- ☐ Tokens tied to session
- ☐ No state-changing GET requests
- ☐ SameSite cookie protection
- ☐ APIs protected where required

---

## 1️⃣2️⃣ File Handling Review

**Goal:** Prevent RCE & data leaks.

### ✅ Checklist

- ☐ File type validation (MIME + extension)
- ☐ File size limits
- ☐ Upload directory non-executable
- ☐ Path traversal protection
- ☐ SVG / HTML uploads blocked or sanitized
- ☐ Download authorization checks    

---

## 1️⃣3️⃣ Error Handling & Logging

**Goal:** Avoid information leakage.

### ✅ Checklist

- ☐ No stack traces to users
- ☐ No sensitive data in logs
- ☐ Debug mode disabled in prod
- ☐ Proper error messages
- ☐ No silent failures (fail-open)
- ☐ Log injection prevented    

---

## 1️⃣4️⃣ Configuration & Secrets

**Goal:** Avoid easy wins for attackers.

### ✅ Checklist

- ☐ No hardcoded secrets
- ☐ Secure environment variable usage
- ☐ Secure CORS configuration
- ☐ Security headers enabled
- ☐ Test/debug endpoints removed
- ☐ Feature flags secured    

---

## 1️⃣5️⃣ Dependency & Supply Chain Review

**Goal:** Prevent inherited vulnerabilities.

### ✅ Checklist

- ☐ Outdated dependencies
- ☐ Known CVEs
- ☐ Insecure deserialization libs
- ☐ Dependency confusion risk
- ☐ Excessive permissions    

---

## 1️⃣6️⃣ Error Paths & Edge Cases (PRO LEVEL)

**Goal:** Find hidden bugs.

### ✅ Checklist

- ☐ Catch blocks reviewed
- ☐ Fallback logic reviewed
- ☐ Null handling checked
- ☐ Default values safe
- ☐ Fail-closed behavior

---

## 1️⃣7️⃣ Vulnerability Chaining

**Goal:** Increase impact.

### ✅ Checklist

- ☐ Can this bug expose sensitive data?
- ☐ Can exposed data be reused?
- ☐ Can bug lead to auth bypass?
- ☐ Can bug escalate privilege?    
- ☐ Can multiple low bugs chain?