# Introduction to Threat Modeling
* _(with real-world metaphors)_
* presenters: David Lawrence, Ying Li @ Docker (security team)
* Making sure they aren't introducing new vulnerabilities in customers' Docker systems.

**Code churn introduces ambiguity for your security footprint.**

Example: WSGI Server --> Django --> (Thumbnailer --> (File Storage), File Storage, SQL)

Vendors should be using appropriate security measures to beat vulnerabilities and provide "Approved methods of using it."

Platform should also be threat modeled: the end-to-end system.
(The related and interconnected systems, such as infrastructure, CI/CD, build registries.)

# The Threat Modeling Process
- A process so you can apply it reliably and consistently across every level.
- Prioritize the worst problems first.
1. Collect Data - What types of information do we need to collect about our application?
2. Analyze Data - A structured approach + our intuitive knowledge.
3. Remediation  - A set security control can be applied to each vulnerability to mitigate it.


# STEP 1. COLLECT DATA:
* Collect list of external dependencies: infrastructure, components. We'll decide which one is "best". (Don't threat model the internals of these.)
* Entrypoints. How data comes into our system and goes out of it. (A closed system is generally secure, but we need interaction with the system.)
* Assets. Things we care about protecting. Physical assets or consumable assets.
* Trust Levels- Tiers (roles) of access within our system, and what are the controls in place for each?
* Data Flows- How information moves around our system.

## Dependencies
- pypi, CAs, caching services
- The Platform- AWS, k8s, Docker, Heroku
-
- Manufacturers- e.g. browsers

## Entrypoints - Two types
- Intended entry points
  - e.g. WSGI server
- Unintended entry points
  - e.g. Thumbnailer, file storage, SQL

## Assets
- Exfiltration
  - TLS, User Data, Photos, Keys & Secrets
- Consumption
  - consumable CPU/memory/storage/bandwidth (e.g. bitcoin mining)
- Nonessential
  - .pyc files (their disappearance/modification acts like a tripwire)

## Trust Levels (e.g.)
- Admin
- Moderator
- Registered User
- Anonymous User- the base trust level, and the one used by untrusted attackers (must be considered)

## Data Flows
- Symbology to diagram them
- Entrypoints must cross a trust boundary, since that's where untrusted data may be introduced.


# STEP 2. ANALYZE DATA:

- Spoofing- e.g. brute force, CSRF
- Tampering- e.g. SQL injection, MITM, DNS Poisoning
- Repudiation- e.g. rm -rf /var/log (we want a way proof/trace that it happened)
- Information disclosure- e.g. Bad Messaging, SQL Injection, MITM ("Your password was incorrect." vs "Username or password invalid.")
- Denial of service- SQL Injection, DDOS (how does the system behave if it is flooded/destroyed?)
- Elevation of privilege- e.g. SQL Injection, CSRF (what happens if the onion is breached?)

## Prioritization
1. Impact- How much damage can they do?
2. Probability- How likely is it that someone can find and make use of the vulnerability?
* ^ Balance these two (e.g. average) to get a score. Consider weighting impact higher than probability (e.g. double impact + probability). Fairly abstract, and easy to disagree about.

## Scoring- DREAD, e.g. on 0-10 or 1-3
- Damage Potential-
- Reproducibility-
- Exploitability- How much resource is required to make use of the vulnerability?
- Affected Users- How broad is the scope of the vulnerability?
- Discoverability- How easy is it to find?

Sum or average to determine scope.

# STEP 3. REMEDIATE VULNERABILITIES:

For each letter of STRIDE, there's a generally-accepted security control for each.
- Spoofing -> Authentication, e.g. User auth (2FA), Request auth (logins, not XSRF)
- Tampering -> Integrity, e.g. Input Validation, Checksums
- Repudiation -> Non-Repudiation, e.g. Append-only logs, Digital signatures
- Information disclosure -> Confidentiality, e.g. Encryption (at rest and transit), Redaction (sensitive data, when displayed), Configuration (so logs don't print passwords)
- Denial of service -> (Better) Availability, e.g. More replicas, Rate Limiting, Recovery Protocol
    - TODO: rewatch
- Elevation of privilege -> Authorization, e.g. Defined auth model, Least privilege (for each operation), Re-authentication (at the time of privileged operations)


# STEP 3+1: REDRAW THREAT MODEL:
Consider changes based on the controls we've introduced.


# System-Level Threat Models
* As you scale, ensure new features/systems are re-considered.
* Consider infrastructure changes and how they impact your threat model.
* Threat model the individual components of your system if you're the ones responsible for development. If you aren't responsible for their development, make sure our usage continues to not break the threat models of those components. (e.g. private fields, methods, security-guaranteed operations only; no raw SQL queries, use model queries)


# SUMMARY
* 3-Step Process: Collect Data, Analyze, Remediate
* During Data Collection, Consider 5 points
* Use that to think about (using STRIDE) what vulnerabilities exist in your system
* Having come up with that list of vulnerabilities, look at the remediations you can use to close off those holes
* Prioritize your vulnerabilities to figure out which ones you want to fix first, using some form of scoring system (like DREAD)
* Iterate over your stack and over time (as things change)


# Learn More
OWASP "Threat Risk Modelling" and "Application Threat Modelling"
Talk tomorrow- state machines
