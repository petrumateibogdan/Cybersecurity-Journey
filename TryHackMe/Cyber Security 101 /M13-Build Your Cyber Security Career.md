# Module 13: Build Your Cyber Security Career


# Security Principles


## CIA Triad

I learned that security is mainly concerned with:

* **Confidentiality** – only authorized people should access data.
* **Integrity** – data should not be changed without authorization, and changes should be detectable.
* **Availability** – systems and services should be available when needed.

The importance of each one depends on the system. For example, confidentiality might not matter much for a public university announcement, but integrity is very important.

## Beyond CIA

I also learned about:

* **Authenticity** – being able to verify that data came from the claimed source.
* **Nonrepudiation** – ensuring the source cannot deny creating or sending something.

### Parkerian Hexad

The Parkerian Hexad expands the security model to six elements:

* Availability
* Utility
* Integrity
* Authenticity
* Confidentiality
* Possession

**Utility** means that information needs to remain useful.
**Possession** means protecting information from unauthorized taking, copying, or control.

## DAD Triad

The opposite of CIA can be viewed as **DAD**:

* **Disclosure** → attacks Confidentiality
* **Alteration** → attacks Integrity
* **Destruction / Denial** → attacks Availability

I also learned that security requires balance. Maximizing one part of CIA can sometimes negatively affect another.

## Security Models

### Bell-LaPadula

Focused on **confidentiality**.

* Simple Security Property → **No Read Up**
* Star Security Property → **No Write Down**
* Discretionary Security Property → access matrix

A useful way I remember the first two:

**Write Up, Read Down**

### Biba

Focused on **integrity**.

* Simple Integrity Property → **No Read Down**
* Star Integrity Property → **No Write Up**

Easy way to remember it:

**Read Up, Write Down**

### Clark-Wilson

Focused on maintaining **integrity** using:

* **CDI** – Constrained Data Item
* **UDI** – Unconstrained Data Item
* **TP** – Transformation Procedures
* **IVP** – Integrity Verification Procedures

Other security models mentioned:

* Brewer-Nash
* Goguen-Meseguer
* Sutherland
* Graham-Denning
* Harrison-Ruzzo-Ullman

## Defence in Depth

I learned that **Defence in Depth** means using multiple layers of security instead of relying on one control.

For example:

```text
Physical security
      ↓
Building security
      ↓
Room security
      ↓
System security
      ↓
Application / data security
```

If one layer fails, other layers can still provide protection.

## ISO/IEC 19249

I learned about the five architectural principles:

1. **Domain Separation**
2. **Layering**
3. **Encapsulation**
4. **Redundancy**
5. **Virtualization**

And the five design principles:

1. **Least Privilege**
2. **Attack Surface Minimisation**
3. **Centralized Parameter Validation**
4. **Centralized General Security Services**
5. **Preparing for Error and Exception Handling**

Important ideas I took from these:

* Give users only the permissions they need.
* Reduce unnecessary services and attack surface.
* Validate input properly.
* Centralize security functions where appropriate.
* Design systems to fail safely.

## Trust

### Trust but Verify

I learned that even when an entity is trusted, its actions should still be verified.

Logging and automated security controls can help with this.

### Zero Trust

The idea is:

**Never trust, always verify.**

Trust should not automatically be given because a device is inside a network or owned by the organization.

Authentication and authorization should be required before accessing resources.

I also learned about **microsegmentation**, where network segments can be very small and communication between them requires additional security controls.

## Vulnerability, Threat and Risk

These three terms are important to keep separate:

**Vulnerability**
A weakness that can be exploited.

**Threat**
A potential danger associated with that weakness.

**Risk**
The likelihood of the threat exploiting the vulnerability and the resulting impact.

Simple example:

```text
Vulnerability → Weak security control
Threat        → Attacker could exploit it
Risk          → Likelihood + impact of exploitation
```

## Shared Responsibility Model

I learned that cloud security is shared between the **cloud provider and the customer**.

The responsibility depends on the service model.

For example:

* **IaaS ( Infrastructure as a Service)** → customer has significant responsibility, including the operating system.
* **SaaS ( Software as a Service )** → provider manages much more of the underlying infrastructure, while the customer still has responsibilities at their level.

## What I Learned

This room gave me a foundation in:

* CIA and DAD
* Authenticity
* Nonrepudiation
* Parkerian Hexad
* Bell-LaPadula
* Biba
* Clark-Wilson
* Defence in Depth
* ISO/IEC 19249
* Least Privilege
* Attack Surface Minimisation
* Trust but Verify
* Zero Trust
* Microsegmentation
* Vulnerabilities, threats and risks
* Shared Responsibility in cloud security

Things I want to revisit later: **security models, ISO/IEC 19249, Zero Trust, risk analysis, and cloud security.**

# 2. Training Impact on Teams


## Why Training Matters

I learned that cyber security skills need to be built through **practice and continuous training**.

Nobody becomes a cyber security expert without practice. Training environments are important because they allow people to learn and experiment without risking production systems.

It is better to learn how to respond to attacks **before** dealing with a real incident.

Training can help organizations:

* Improve the skills of their security teams
* Prepare employees for real incidents
* Increase productivity
* Help junior employees develop faster
* Reduce the amount of repeated training required from senior staff
* Create a common baseline for measuring skills
* Make it easier to assign people to tasks based on their abilities
* Encourage teamwork through activities such as CTFs

## Training Platforms

I learned that smaller teams can use **off-the-shelf training**, while larger organizations or teams with specific requirements may benefit from customized training.

TryHackMe's **Content Studio** can be used by organizations to modify or create training content around their own requirements.

For larger companies, training platforms may also need integrations such as:

* **SSO (Single Sign-On)**
* **APIs**
* Integration with existing systems

## Training ROI

I learned how training can be measured financially using **Return on Investment (ROI)**.

Example:

```text
10 employees
$80,000 cost per employee
4% productivity increase
```

Productivity gain:

```text
10 × $80,000 × 4% = $32,000
```

Training cost:

```text
10 × $500 = $5,000
```

ROI:

```text
$32,000 / $5,000 = 640%
```

So the example showed a **640% ROI**.

## Training Scenario

I also learned how to calculate the productivity savings for a different team:

```text
20 employees
$50,000 per employee
4% productivity increase
```

Calculation:

```text
20 × $50,000 × 4% = $40,000
```

**Savings = $40,000**

## Choosing a Training Vendor

I learned that organizations should consider:

* Who the training is for
* Employees' experience and background
* Their roles and relevant topics
* Whether the vendor has experience with similar organizations
* Content breadth, depth and quality
* Whether users can learn and practice on the same platform
* Cost compared with the expected benefits

## What I Learned

The main thing I took from this room is that **cyber security requires continuous learning**.

Training is useful not only for learning technical skills, but also for preparing teams to respond to real incidents, measuring skill development, improving productivity and building a stronger security team.

I also learned how to think about training from a **business perspective**, including costs, productivity gains, ROI and choosing a training platform.

