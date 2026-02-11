## What Are Secure Coding Practices?
- Secure coding practices are rules + patterns developers follow to prevent vulnerabilities at code level.

**Goal:** Stop bugs before deployment, not after a pentest report.

# Secure Coding Practice
## 1. Input Validation & Sanitization
### Learn
- Trust **nothing** from client (headers, cookies, JSON, params)
- Allowlist > blocklist
- Type, length, format validation
- Server-side validation only    
### Checklist
-  ☐ Validate data type (int, string, email, UUID)
-  ☐ Enforce max length
-  ☐ Reject unexpected fields
-  ☐ Normalize input before validation 
-  ☐ Validate **every** input (query, body, headers, cookies)
- ☐ Use allowlists, not regex-only filters
- ☐ Enforce min/max length
- ☐ Validate numeric ranges
- ☐ Validate UUID format strictly
- ☐ Reject unknown JSON fields
- ☐ Normalize Unicode before validation
- ☐ Reject null bytes (`\x00`)
- ☐ Validate file paths (no `../`)
- ☐ Never trust hidden fields

---
## 2. Output Handling & Encoding 
### Learn
- HTML encoding
- JavaScript encoding
- URL encoding
- Attribute encoding
### Checklist
- ☐ Encode output based on context
- ☐ Never build HTML with string concatenation
- ☐ Use framework auto-escaping correctly    
- ☐ HTML encode user content
- ☐ JS encode inside scripts
- ☐ URL encode inside URLs
- ☐ Attribute encode HTML attributes
- ☐ No inline JS with user input
- ☐ CSP headers present
- ☐ Disable dangerous templating features
- ☐ Auto-escape enabled in templates

---
## 3. Authentication Security
### Learn
- Password hashing (bcrypt, argon2)
- MFA basics
- Secure session handling
- Rate limiting    
### Checklist
- ☐ Hash passwords with Argon2 / bcrypt (never encrypt)
- ☐ Use secure password policies
- ☐ Implement login rate limits
- ☐ Generic login error messages
- ☐ No plaintext or reversible encryption
- ☐ Enforce password complexity
- ☐ Prevent username enumeration    
- ☐ MFA supported
- ☐ Password reset tokens random & expiring
- ☐ One-time use reset tokens
- ☐ Logout invalidates session server-side
- ☐ Re-auth for sensitive actions

---
## 4. Session Management
### Learn
- Secure cookies
- Session rotation
- Token expiration
- Logout invalidation    
### Checklist
- ☐ HttpOnly cookies
- ☐ Secure flag enabled
- ☐ SameSite set correctly
- ☐ Rotate session after login    
- ☐ Session ID unpredictable
- ☐ Rotate session on login
- ☐ Rotate session on privilege change
- ☐ Session timeout enforced
- ☐ Invalidate session on logout
- ☐ No session ID in URL
- ☐ Separate auth & CSRF tokens

---
## 5. Authorization & Access Control
### Learn
- Server-side authorization
- Object-level checks
- Role-based vs attribute-based access    
### Checklist
- ☐ Check authorization on **every** request
- ☐ Never trust user-supplied IDs and Roles
- ☐ Enforce access at API layer
- ☐ Default deny 
- ☐ Server-side authorization only
- ☐ Role checks enforced centrally
- ☐ Ownership checks per object
- ☐ Prevent privilege escalation
- ☐ No “admin=true” style flags
- ☐ Validate resource ownership every time

---
## 6. Database Security
### Learn
- Prepared statements
- ORM pitfalls
- Least privilege DB users    
### Checklist
- ☐ No string-built SQL queries
- ☐ Use parameterized queries
- ☐ DB user has minimal permissions
- ☐ Handle DB errors safely  and Error message sanitized
- ☐ Parameterized queries only
- ☐ No dynamic SQL
- ☐ ORM safe usage (no raw queries)
- ☐ Escaping is NOT used instead of parameters
- ☐ No stacked queries allowed
- ☐ Query timeouts enforced
- ☐ Sensitive fields encrypted at rest

---
## 7. Error Handling & Logging
### Learn
- Safe error messages
- Secure logging
- Sensitive data masking    
### Checklist
- ☐ No stack traces to users
- ☐ Log errors server-side only
- ☐ Mask secrets in logs
- ☐ Centralized logging     
- ☐ Generic error messages
- ☐ Detailed logs server-side not fronted-side
- ☐ No secrets in logs
- ☐ Mask tokens & passwords    
- ☐ Log auth & privilege events
- ☐ Protect log integrity

---
## 8. File Upload & Download Security
### Learn
- Extension handling
- Storage outside web root  
-  MIME & extension validation
- Safe storage locations
- Filename randomization
- Execution prevention
- Secure downloads
### Checklist
- ☐ Validate MIME + extension and Content type server-side
- ☐ Randomize filenames
- ☐ No / Disable execution permissions
- ☐ Virus scanning if needed 
- ☐ Validate file extension (allowlist)    
- ☐ Store files outside web root    
- ☐ Enforce file size limits
- ☐ Strip metadata if possible
- ☐ Prevent double extensions
- ☐ No user-controlled paths
- ☐ Scan uploads if high risk

---
## 9. Serialization & Deserialization Safety
### Learn
- Unsafe deserialization risks
- Signed objects
- Avoid native serialization formats    
### Checklist
- ☐ Avoid deserializing user input
- ☐ Use safe formats (JSON)
- ☐ Enforce type restrictions
- ☐ Sign serialized data  
- ☐ Avoid native serialization formats
- ☐ No user-supplied serialized objects
- ☐ Use JSON with schema validation
- ☐ Enforce strict types
- ☐ Sign serialized data
- ☐ Disable dangerous magic methods
- ☐ Validate object structure
- ☐ Reject unexpected fields

---
## 10. Command Execution & OS Interaction
### Learn
- Avoid shell execution
- Safe APIs
- Argument escaping    
### Checklist
- ☐ Avoid system() / exec()
- ☐ No user input in system commands 
- ☐ Use native APIs & libraries instead of shell
- ☐ Restrict OS permissions
- ☐ Avoid shell execution 
- ☐ Escape arguments properly
- ☐ Drop OS privileges
- ☐ Use allowlisted commands only
- ☐ Enforce execution timeouts
- ☐ No backticks or eval-like behavior

---
## 11. CSRF Protection
### Learn
- CSRF tokens
- SameSite cookies
- Origin/Referer checks    
### Checklist
- ☐ CSRF tokens on state-changing requests
- ☐ SameSite=Lax or Strict
- ☐ Reject missing/invalid tokens 
- ☐ Token validated server-side
- ☐ Token bound to session    
- ☐ Origin/Referer checked
- ☐ No GET requests modify state

---
## 12. API Security
### Learn
- Auth per endpoint
- Rate limiting
- Schema validation    
### Checklist
- ☐ Validate JSON schema
- ☐ Enforce auth per endpoint
- ☐ Rate limit per APIs & user/IP
- ☐ No hidden fields accepted
- ☐ Auth check on **every** endpoint
- ☐ Object-level authorization (BOLA)
- ☐ Prevent mass assignment      
- ☐ Disable introspection (GraphQL)
- ☐ Enforce query depth limits
- ☐ No sensitive data in responses
- ☐ Version APIs properly

---
## Business Logic Security

### Learn:
- Workflow enforcement
- Server-side validation of actions
- State & step integrity
- Race condition prevention
- Price & quantity validation
- Replay attack prevention
### Checklist
- ☐ Enforce workflow steps
- ☐ Prevent skipping steps
- ☐ Validate pricing server-side
- ☐ Prevent race conditions
- ☐ Lock critical operations
- ☐ Recalculate totals server-side
- ☐ No client-side trust
- ☐ Prevent replay attacks

---
## Headers & Browser Security

### Learn
- Content Security Policy (CSP)
- HSTS (Strict Transport Security)
- Clickjacking protection
- MIME sniffing prevention
- CORS security rules
- Referrer leakage control

### Checklist
- ☐ Content-Security-Policy set
- ☐ X-Frame-Options / frame-ancestors
- ☐ X-Content-Type-Options
- ☐ Referrer-Policy
- ☐ HSTS enabled
- ☐ Disable dangerous CORS configs
- ☐ No `Access-Control-Allow-Origin: *` with creds

---
## 13. Secrets & Configuration Management
### Learn
- Environment variables
- Secret managers
- Secure defaults    
### Checklist
- ☐ No secrets in code
- ☐ Rotate keys regularly
- ☐ Secure config files
- ☐  Use environment variables
- ☐ Secure `.env` files
- ☐ Disable debug in production
- ☐ Secure backup files
- ☐ Remove default credentials
- ☐ Secure cloud metadata access

---
## 14. Dependency & Supply Chain Security
### Learn
- Dependency scanning
- Version pinning
- Patch management
### Checklist
- ☐ Regular dependency updates
- ☐ Use trusted sources
- ☐ Lock dependency versions
- ☐ Use trusted package sources
- ☐ Lock dependency versions
- ☐ Scan for known CVEs
- ☐ Remove unused dependencies
- ☐ Monitor CVEs & dependency updates
- ☐ Prevent dependency confusion
- ☐ Verify package integrity

## Source:
[[https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/stable-en/02-checklist/05-checklist]]
[[https://www.xenonstack.com/blog/secure-code-best-practices]]
[[https://www.sonarsource.com/resources/library/secure-coding/]]
[[https://safecomputing.umich.edu/protect-the-u/protect-your-unit/secure-coding/best-practices]]
[[https://www.securityjourney.com/post/best-practices-for-secure-coding]]