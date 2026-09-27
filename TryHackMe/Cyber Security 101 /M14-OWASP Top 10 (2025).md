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
