# Module 14: OWASP Top 10 (2025)

# 1. OWASP Top 10 2025: IAAA Failures



Notes from my TryHackMe room covering three OWASP Top 10:2025 categories related to **Identity, Authentication, Authorisation and Accountability (IAAA)**.

I’m keeping these notes so I can come back later and remember what I learned.

## IAAA

I learned that IAAA is a simple way to understand how applications identify users and control their actions.

```text
Identity
   ↓
Authentication
   ↓
Authorisation
   ↓
Accountability
```

* **Identity** – the account representing a person or service.
* **Authentication** – proving that identity using things like passwords, OTPs or passkeys.
* **Authorisation** – deciding what that identity is allowed to do.
* **Accountability** – recording and alerting on who did what, when and from where.

Each stage depends on the previous one.

## A01 – Broken Access Control

I learned that **Broken Access Control** happens when the server does not properly enforce what a user is allowed to access.

A common example is **IDOR (Insecure Direct Object Reference)**.

For example:

```text
?id=7
↓
?id=6
```

If changing the ID allows me to access another user's data, the application has an access control problem.

I also learned about:

* **Horizontal privilege escalation** – accessing another user's resources at the same privilege level.
* **Vertical privilege escalation** – gaining access to functionality intended for a higher-privileged user, such as an administrator.

Important lesson:

**Access control must be enforced server-side on every request.**

## A07 – Authentication Failures

I learned that authentication failures happen when an application cannot reliably verify or correctly bind a user's identity.

Common problems include:

* Username enumeration
* Weak or guessable passwords
* Missing rate limits / account lockouts
* Login or registration logic flaws
* Insecure sessions or cookies

One of the practical challenges involved registering a username using a different capitalization:

```text
admin
↓
aDmiN
```

This showed me how problems with handling usernames and canonicalisation can lead to authentication issues.

Important things to remember:

* Use a canonical form for identities.
* Prevent duplicate accounts caused by case differences.
* Rate-limit / lock out brute-force attempts.
* Rotate sessions after password or privilege changes.

## A09 – Logging & Alerting Failures

I learned that logging is important for **accountability** and incident investigation.

Without good logs, defenders may not be able to determine:

* Who performed an action
* What they did
* When it happened
* Where it came from

Examples of logging failures include:

* Missing authentication events
* Vague error messages
* No alerts for brute-force activity
* No alerts for privilege changes
* Short log retention
* Logs stored somewhere attackers can modify

Important lesson:

**Logging should cover the authentication lifecycle and important security events.**

Useful events include:

* Failed/successful authentication
* Password changes
* 2FA changes
* Role/privilege changes
* Administrative actions

Logs should also be centralised, stored off-host and retained appropriately.

Alerts can help detect things such as:

```text
Brute-force bursts
        ↓
Unusual authentication activity
        ↓
Privilege elevation
```

## What I Learned

The three OWASP categories connect closely to IAAA:

```text
A01 → Access control
A07 → Authentication
A09 → Accountability / logging
```

The main things I want to remember are:

* **A01:** Always enforce authorisation server-side.
* **A07:** Properly handle identities, authentication, sessions and brute-force protection.
* **A09:** Log important security events and alert on suspicious activity.

This room helped me understand how mistakes in **identity, authentication, authorisation and accountability** can turn into real web application vulnerabilities.


# 2. OWASP Top 10 2025: Application Design Flaws



Notes from my TryHackMe room covering four OWASP Top 10:2025 categories related to **architecture, configuration, dependencies, cryptography and design**.

I’m keeping these notes so I can come back later and remember what I learned.

## Categories

* **AS02 – Security Misconfigurations**
* **AS03 – Software Supply Chain Failures**
* **AS04 – Cryptographic Failures**
* **AS06 – Insecure Design**

## AS02 – Security Misconfigurations

I learned that security misconfigurations happen when systems are deployed with unsafe defaults, exposed services, weak permissions or incomplete security settings.

Common examples:

* Default credentials
* Unnecessary exposed services
* Misconfigured cloud storage
* Missing authentication / authorisation
* Verbose error messages
* Outdated software
* Exposed AI/ML endpoints

Important things to remember:

* Harden default configurations.
* Remove unnecessary services.
* Use strong authentication and least privilege.
* Limit network exposure.
* Keep software and containers updated.
* Don't expose stack traces or sensitive system information.
* Regularly review cloud permissions.
* Include configuration checks in the deployment process.

**Challenge:** I investigated a User Management API with too many exposed traces.

## AS03 – Software Supply Chain Failures

I learned that a vulnerability doesn't always come from my own code. Applications depend on libraries, packages, APIs, services and other components that can also be compromised.

Common problems:

* Unverified dependencies
* Outdated libraries
* Automatic updates without verification
* Vulnerable third-party components
* Insecure CI/CD pipelines
* Poor dependency provenance tracking
* Unverified AI models or datasets

Important things to remember:

* Verify third-party components.
* Keep dependencies updated.
* Sign and verify software updates.
* Secure CI/CD pipelines.
* Track dependency provenance.
* Monitor dependencies after deployment.
* Treat third-party AI components as part of the supply chain.

**Challenge:** I investigated an application using an outdated `vulnerable_utils.py` component.

## AS04 – Cryptographic Failures

I learned that cryptography can fail through incorrect implementation, weak algorithms, exposed keys or poor secret management.

Common examples:

* Weak/deprecated algorithms such as MD5 or SHA-1
* ECB mode
* Hard-coded secrets
* Poor key management
* Poor key rotation
* Unencrypted sensitive data
* Invalid TLS certificates
* Exposed secrets in AI systems

I learned that modern applications should use strong cryptographic algorithms and proper key management.

Examples mentioned:

```text
AES-GCM
ChaCha20-Poly1305
TLS 1.3
```

For key management, examples include:

```text
AWS KMS
Azure Key Vault
HashiCorp Vault
```

Important things to remember:

* Don't hard-code secrets.
* Protect data both at rest and in transit.
* Rotate keys and secrets.
* Keep track of certificates and keys.
* Never expose sensitive secrets through AI models or automation.

**Challenge:** I investigated a web application where I had to find the key needed to decrypt a file.

## AS06 – Insecure Design

I learned that **insecure design** is different from simply having a coding bug.

The problem can be built into the architecture or business logic from the beginning.

Examples include:

* Weak recovery or approval flows
* Bad assumptions about user behaviour
* Missing security requirements
* Missing abuse-case analysis
* Test/debug functionality left in production
* AI systems with too much authority
* Missing guardrails around AI agents

An important lesson:

**You can't simply patch an insecure design. Sometimes the design or workflow itself needs to change.**

## Insecure Design + AI

I learned that AI introduces additional design risks.

Examples include:

* Prompt injection
* Blindly trusting model output
* AI agents having excessive permissions
* Unverified models or datasets
* Missing human review
* Sensitive information being placed into prompts

Important principles:

* Treat AI models as untrusted.
* Validate model inputs and outputs.
* Separate system instructions from user input.
* Keep sensitive information out of prompts when possible.
* Require human review for high-risk actions.
* Monitor model behaviour and provenance.
* Include AI threats in threat modelling.

## Secure Design

Things I want to remember:

```text
Threat modelling
      ↓
Security requirements
      ↓
Least privilege
      ↓
Authentication / Authorisation
      ↓
Secure dependencies
      ↓
Testing
      ↓
Monitoring
```

I learned that security needs to be considered throughout development rather than added at the very end.

## What I Learned

This room helped me understand that security problems can come from much more than vulnerable code.

I learned about:

* **Security misconfigurations**
* **Software supply chains**
* **Cryptographic failures**
* **Insecure design**
* Dependency security
* Key and secret management
* Least privilege
* Threat modelling
* Secure architecture
* AI-specific security risks

The main thing I want to remember is:

**Security needs to be built into the architecture, configuration, dependencies and design from the beginning.**

# 3. OWASP Top 10 2025: Insecure Data Handling
