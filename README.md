# 🧬 JML Dynamic Group Automation Lab (Joiner • Mover • Leaver)

**Platform:** Microsoft Entra ID  
**Focus:** Identity Lifecycle Automation, Dynamic Groups, Enterprise App Access, Conditional Access

<img width="1536" height="1024" alt="ChatGPT Image Feb 4, 2026 at 10_46_48 AM" src="https://github.com/user-attachments/assets/80b4692f-fe9f-43bb-8af8-5bcada3585d0" />

---

## 📘 Project Overview

In this project, I implemented a Joiner–Mover–Leaver (JML) model where **user attributes** automatically controlled:

- Group membership
- Application access
- Conditional Access enforcement
- Access revocation

All behavior was validated using group membership, jwt.ms token inspection, and sign-in logs.

---

## 🧱 Identity Foundation — Users as the HR System

I created three users representing different departments and configured their attributes.

| User | Department | Job Title |
|-----|------------|-----------|
| JML-HR-User | HR | HR Analyst |
| JML-Finance-User | Finance | Finance Analyst |
| JML-IT-User | IT | IT Admin |

These attributes acted as the simulated HR source of truth for access decisions.

![Image 2-3-26 at 22 21](https://github.com/user-attachments/assets/29157292-1db3-4318-90e8-9a551b4511b5)

![Image 2-3-26 at 22 23](https://github.com/user-attachments/assets/76b4b056-c72d-48d3-867b-c8a16bd2e2a2)

![Image 2-3-26 at 22 25](https://github.com/user-attachments/assets/a58a07e4-cb45-40e3-ae97-7f8a2a396e39)

![Image 2-3-26 at 22 27](https://github.com/user-attachments/assets/76922eec-6cdd-4af3-801f-55905229fe94)

---



## ⚙️ Dynamic Security Groups — Automation Layer

I created dynamic groups that evaluate user attributes in real time.

- DG-HR-Users → `(user.department -eq "HR")`
- DG-Finance-Users → `(user.department -eq "Finance")`
- DG-IT-Admins → `(user.department -eq "IT") and (user.jobTitle -contains "Admin")`

After a short time, Entra automatically populated users into the correct groups without any manual assignment.

![Image 2-3-26 at 22 29](https://github.com/user-attachments/assets/b56f55fc-12d4-4c0f-8c33-657f8fcaf46d)

![Image 2-3-26 at 22 30](https://github.com/user-attachments/assets/778537a1-a4f6-4c30-b323-7ed575af9f25)

![Image 2-3-26 at 22 32](https://github.com/user-attachments/assets/1c26aae4-21aa-4672-a791-6ae28fc2229e)

![Image 2-3-26 at 22 31](https://github.com/user-attachments/assets/c64fcfa1-098f-42bc-8788-ef80c6b0f560)

![Image 2-3-26 at 22 34](https://github.com/user-attachments/assets/77769860-e37a-4f76-b75b-440aa2c0101a)

![Image 2-3-26 at 22 39](https://github.com/user-attachments/assets/332b08de-c9c1-4f48-b5b7-c23b4b84088a)

![Image 2-3-26 at 22 38](https://github.com/user-attachments/assets/86b8dace-8182-4c2d-a54e-d11389e2cb3d)

![Image 2-3-26 at 22 39](https://github.com/user-attachments/assets/ca995f79-9a2b-47a6-90a7-22b5c164d07c)


---

## 🧩 Enterprise Application Entitlement (ENT-RBAC-Portal)

I configured the enterprise application so only dynamic groups could access it.

- Assignment required = Yes
- DG-HR-Users, DG-Finance-Users, DG-IT-Admins assigned

This ensured application access was fully attribute-driven.

![Image 2-3-26 at 22 42](https://github.com/user-attachments/assets/ce0bb5a5-6858-4f6b-96b4-f74785f937c0)

![Image 2-3-26 at 22 43](https://github.com/user-attachments/assets/42139dc2-1220-445d-8dfa-f599c5365232)

---

## 🔐 Conditional Access by Department

I created Conditional Access policies targeting each dynamic group.

| Group | Controls |
|------|----------|
| HR | Require MFA |
| Finance | MFA + session controls |
| IT | Strong authentication + short session |

Because policies target groups instead of users, security enforcement adapts automatically as users move between departments.

![Image 2-3-26 at 22 45](https://github.com/user-attachments/assets/8137ed1f-07ae-4210-810b-67fb6621fae8)

![Image 2-3-26 at 22 46](https://github.com/user-attachments/assets/9d4d338b-3da4-4666-af69-17110ad7bd5e)

![Image 2-3-26 at 22 47](https://github.com/user-attachments/assets/20564b22-d4eb-4cca-b071-ee0a0f637596)

![Image 2-3-26 at 22 48](https://github.com/user-attachments/assets/fba5658a-2908-4b33-a0f1-2507093d188e)

![Image 2-3-26 at 22 49](https://github.com/user-attachments/assets/973ce2dd-0139-41bb-a64f-6e49a0f5eb6a)

![Image 2-3-26 at 22 50](https://github.com/user-attachments/assets/5fab0399-85b7-49a2-b186-5c501b4d1adc)

![Image 2-3-26 at 22 51](https://github.com/user-attachments/assets/6ce9755e-dccc-4496-a28d-ff624974732f)

![Image 2-3-26 at 22 52](https://github.com/user-attachments/assets/d4f83e3d-5df3-47c7-9277-9704d2f0e4de)

![Image 2-3-26 at 22 54](https://github.com/user-attachments/assets/853fc5b3-7fc6-4899-b90c-ebf7da367def)

![Image 2-3-26 at 22 55](https://github.com/user-attachments/assets/6ce26384-c564-45ca-b5b2-d5a07203ab84)

![Image 2-3-26 at 22 56](https://github.com/user-attachments/assets/ba15da73-13df-494d-9a72-d25b5b8c1990)

---

## 🟢 Joiner Scenario — New Hire Access

When signing in as JML-HR-User:

- App access worked automatically
- MFA was required due to the HR policy
- Token claims were validated using jwt.ms
- Sign-in logs confirmed the correct Conditional Access policy applied

No manual provisioning was required.

![Image 2-3-26 at 23 01](https://github.com/user-attachments/assets/34e4a7a8-c32c-4f2d-96b1-43098b021c3e)

![Image 2-3-26 at 23 01](https://github.com/user-attachments/assets/2cdbcd1e-de07-4c58-a94c-9de5ea54ac50)

![Image 2-3-26 at 23 02](https://github.com/user-attachments/assets/1f93bbc6-654a-4410-8463-abc2fe17a6da)

![Image 2-3-26 at 23 03](https://github.com/user-attachments/assets/d34b823d-939f-469e-9113-c1e3c406b652)

![Image 2-3-26 at 23 10](https://github.com/user-attachments/assets/f6e54085-1f59-4484-a214-e3e88b066c1b)

![Image 2-3-26 at 23 10 (1)](https://github.com/user-attachments/assets/b1b67cca-e4f2-4629-9420-3552c93381af)

---

## 🔁 Mover Scenario — HR → Finance

I changed only the Department attribute from HR to Finance.

Within minutes:

- The user was removed from DG-HR-Users
- The user appeared in DG-Finance-Users
- Application access remained intact
- A different Conditional Access policy was enforced

This demonstrated fully automated access changes driven by identity attributes.

![Image 2-3-26 at 23 12](https://github.com/user-attachments/assets/d11b06a2-b6af-4207-99e9-5722902fa3d5)

![Image 2-3-26 at 23 16](https://github.com/user-attachments/assets/efbcf3ea-29cc-42b2-b260-6de4677ed1ef)

![Image 2-3-26 at 23 19](https://github.com/user-attachments/assets/5f7cede0-1d24-41c1-8fd6-ad97bfd73970)

![Image 2-3-26 at 23 19 (1)](https://github.com/user-attachments/assets/78cdfa53-152a-4435-989e-8bc1429c6471)

![Image 2-3-26 at 23 19 (2)](https://github.com/user-attachments/assets/2b72bcd3-73ab-4e91-a689-e31be3b5e9dd)

![Image 2-3-26 at 23 26](https://github.com/user-attachments/assets/6c7e47f0-c04d-4872-be8c-4674a2723f5c)

![Image 2-3-26 at 23 26](https://github.com/user-attachments/assets/25521f22-e978-4017-a2ba-49870550c711)

---

## 🔴 Leaver Scenario — Access Revoked

I disabled the user account by setting **Account enabled = No**.

Results:

- Login attempts failed
- Sign-in logs showed: *User account is disabled*
- Application access was immediately revoked

![Image 2-3-26 at 23 38](https://github.com/user-attachments/assets/1cea242b-2fe7-4284-99f7-1405f3e48ccf)

![Image 2-3-26 at 23 40](https://github.com/user-attachments/assets/61bc1ac9-7c2c-4e7a-8f52-81a3f4fc6a47)

![Image 2-3-26 at 23 40 (1)](https://github.com/user-attachments/assets/c6b0961b-5922-406f-9888-4508cbe9fbcb)

![Image 2-3-26 at 23 48](https://github.com/user-attachments/assets/f25ee79f-3e5a-48fb-80ac-7ec7bfbad04c)
