# 🚪 Flagdoor – BlitzHack 2025 (Web, 474 pts)

**Challenge Name:** Flagdoor  
**Category:** Web  
**Author:** Shadowh  
**Points:** 474  
**URL:** [http://flagdoor-cdsdwf9w.blitzhack.xyz/](http://flagdoor-cdsdwf9w.blitzhack.xyz/)

---

### 📝 Description

> "Finding doors can be exhausting too. Sometimes you need to tackle the rate limits."

This was a web challenge that involved user authentication and improper access control. It hinted at rate limiting, but the actual vulnerability was an IDOR combined with negative ID manipulation.

---

### 🧠 Recon & Observations

- A typical **login + registration** flow was implemented.
- After registration and login, the dashboard displayed a message like:

Welcome, shadow
User ID: 58452
Logout

- I assumed this `User ID` might be used to retrieve user-specific data.
- When changing the ID manually, it sometimes showed another user's data or "user not found".
- I initially tried brute-forcing IDs, but it didn’t work due to a **rate-limiting system** — I could only send 30 requests at a time.
- There was a **rate-limit cookie** present. After removing it, I was able to send unlimited requests.

---

### 🛠️ Exploitation

1. **Tested for IDOR:**
   - Suspected that the app used direct `user_id` references in backend.
   - Tried modifying the ID manually — no success with regular positive integers.

2. **Tried Negative Values:**
   - On a hunch, entered `-3` as a user ID (possibly in cookie, request parameter, or crafted URL).
   - It worked! The app responded:

Welcome, admin
User ID: -3
Logout

- That’s an **IDOR vulnerability with negative IDs**, revealing the admin account and access.

3. **Flag Found:**
   - The admin dashboard showed the flag.

---

### 🏁 Flag

Blitz{ID0R_1s_FUN_w1TH_N3g4t1v3_IDs!?}

---

### 🔑 Key Takeaways

- **IDOR (Insecure Direct Object Reference)** is still a common and exploitable flaw.
- Always test edge cases: negative numbers, zeros, large numbers, etc.
- Access control should be based on session ownership and roles — not just on numeric ID values.
- Never trust user-controlled identifiers for sensitive lookups.

---

### 👥 Team: 0bscuri7y

- Dishant Chaudhary  

---

*"It was really fun and a bit tricky as well!"*