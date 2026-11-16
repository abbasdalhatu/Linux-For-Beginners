
# COMMON CYBERATTACKS AND ATTACK TYPES

## 1. Introduction

A cyberattack is an attempt to access, damage, disrupt, manipulate, or steal information from computer systems and networks without proper authorization.

Understanding attack types helps security professionals identify threats and develop defenses.

## 2. Malware Attacks

Malware is malicious software designed to harm systems or compromise information.

Common types:

- Virus: Attaches itself to files and spreads when infected files execute.
- Worm: Spreads between systems, often without user interaction.
- Trojan: Disguises itself as legitimate software.
- Ransomware: Encrypts or blocks access to data and demands payment.
- Spyware: Secretly monitors activity or collects information.
- Rootkit: Hides malicious activity and maintains privileged access.

Protection:
- Use reputable security software.
- Keep systems updated.
- Avoid suspicious downloads.
- Maintain offline or protected backups.

## 3. Phishing Attacks

Phishing is a social engineering attack that uses deceptive messages to steal information or trick users into taking unsafe actions.

Common types:

- Email phishing: Fraudulent emails.
- Spear phishing: Messages targeting specific individuals.
- Whaling: Attacks targeting senior executives.
- Smishing: Phishing through SMS.
- Vishing: Phishing through voice calls.

Protection:
- Verify the sender.
- Inspect suspicious links.
- Avoid sharing passwords or verification codes.
- Use MFA.
- Report suspicious messages.

## 4. Denial-of-Service Attacks

A Denial-of-Service (DoS) attack attempts to make a system or service unavailable.

A Distributed Denial-of-Service (DDoS) attack uses multiple systems or sources to overwhelm a target.

Protection:
- Use traffic filtering.
- Apply rate limiting.
- Use DDoS protection services.
- Monitor unusual traffic patterns.
- Maintain incident response procedures.

## 5. Man-in-the-Middle Attacks

A Man-in-the-Middle (MITM) attack occurs when an attacker intercepts communication between two parties.

Possible objectives:
- Stealing information.
- Monitoring communication.
- Modifying transmitted data.

Protection:
- Use HTTPS.
- Avoid untrusted Wi-Fi networks.
- Use properly configured encryption.
- Verify certificates.
- Use trusted VPN services when appropriate.

## 6. Password Attacks

Password attacks attempt to obtain or compromise user credentials.

Common types:

- Brute force: Trying many possible passwords.
- Dictionary attack: Trying words and common passwords.
- Credential stuffing: Reusing stolen username and password combinations.
- Password spraying: Trying a small set of common passwords against many accounts.
- Keylogging: Recording keyboard input.

Protection:
- Use long, unique passwords.
- Enable MFA.
- Use a password manager.
- Apply account lockout or rate-limiting controls.
- Monitor suspicious login attempts.

## 7. SQL Injection

SQL Injection occurs when an application handles untrusted input unsafely, allowing an attacker to interfere with database queries.

Potential consequences:
- Unauthorized data access.
- Data modification.
- Authentication bypass.
- Data deletion.

Protection:
- Use parameterized queries.
- Validate input appropriately.
- Apply least-privilege database permissions.
- Avoid building SQL queries through unsafe string concatenation.

## 8. Cross-Site Scripting (XSS)

XSS occurs when an application allows untrusted content to execute as script in another user's browser.

Common categories:

- Stored XSS.
- Reflected XSS.
- DOM-based XSS.

Protection:
- Use context-aware output encoding.
- Sanitize HTML when necessary.
- Apply Content Security Policy.
- Avoid unsafe DOM operations.

## 9. Session Hijacking

Session hijacking occurs when an attacker obtains or abuses another user's active session.

Protection:
- Use HTTPS.
- Secure session cookies.
- Regenerate session IDs after authentication.
- Set appropriate session expiration.
- Revoke sessions after logout or suspected compromise.

## 10. Insider Threats

An insider threat comes from someone with legitimate access who intentionally or accidentally causes harm.

Examples:
- Unauthorized data sharing.
- Misuse of administrative privileges.
- Accidental exposure of confidential information.

Protection:
- Apply least privilege.
- Monitor sensitive activities.
- Provide security awareness training.
- Review access permissions regularly.

## 11. Zero-Day Attacks

A zero-day attack exploits a vulnerability that is unknown to the vendor or has no available fix at the time of exploitation.

Protection:
- Apply security updates quickly.
- Use endpoint detection tools.
- Monitor unusual behavior.
- Restrict unnecessary access.
- Maintain incident response plans.

## 12. Cybersecurity Defense Strategy

Organizations should use multiple layers of protection:

1. Firewalls.
2. Endpoint protection.
3. Strong authentication.
4. Network monitoring.
5. Regular patching.
6. Data backups.
7. Security awareness training.
8. Incident response procedures.

## 13. Practice Exercises

1. What is malware?
2. Differentiate between a virus and a worm.
3. Explain phishing and its different forms.
4. What is the difference between DoS and DDoS?
5. Explain a Man-in-the-Middle attack.
6. What is SQL Injection?
7. Explain the three common types of XSS.
8. What is a zero-day vulnerability?
9. Mention five ways to defend against cyberattacks.

## Summary

Cyberattacks target systems, networks, applications, and people in different ways. Understanding their methods and defenses helps security professionals prevent incidents and protect digital assets.
