# Old Sessions
**Category**: Web exploitation

**Vulnerability**: Insecure session management

## Overview
The objective of this challenge was to investigate a web application's session management mechanism and obtain access to the flag.

The application allowed users to register and log in. During the investigation, I discovered an endpoint that exposed information about active sessions.

---

### Reconnaissance
I started by opening the challenge instance and creating an account with test credentials.

While inspecting the application, I noticed a reference to the */sessions* endpoint in the page's comments. If the endpoint had not been explicitly referenced, directory and content discovery tools such as Gobuster or ffuf could have helped identify it by testing possible paths against the application. I navigated to this endpoint to investigate its behavior.

![The endpoint is referenced in this comment](https://github.com/user-attachments/assets/edc74dd0-6354-4cc6-ab80-5d57197fac41)


The page displayed information about active sessions, including the session associated with my test account and another associated with the admin user.





### Exploitation
Afterwards, I opened the developer tools and located the session cookie in the browser's storage. I replaced its value with the session identifier associated with the administrator account. After reloading the main page, the application recognized me as the administrator and showed the flag.

---

### Security Impact
An attacker able to access the exposed session information could potentially impersonate another user without knowing their credentials.

Depending on the application's privileges and the data accessible to the impersonated account, this could result in unauthorized access to sensitive information or administrative functionality.

### Root Cause

The application exposed sensitive session information through an accessible endpoint. This information included a session identifier associated with the administrator account.

The server accepted the administrator's session identifier when it was supplied in the session cookie, allowing me to access the application as that user without providing the administrator's credentials.

This behavior demonstrates insecure session management and enables session impersonation.

### Remediation
- Restrict access to session-management endpoints and make sure that session identifiers are never exposed to unauthorized users.

---

### Key Takeaways

This challenge demonstrated the importance of inspecting application endpoints, understanding how browser cookies maintain authenticated sessions, and investigating the consequences of exposing session information.

