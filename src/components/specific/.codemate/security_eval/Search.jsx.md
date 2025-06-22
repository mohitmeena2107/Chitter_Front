# Security Vulnerability Report

**Component:** `Search`
**Language:** JavaScript (React)
**Date:** 2024-06

---

## 1. Input Handling and Injection Risks

### a. Lack of Input Sanitization for Search
- **Problem:** The `search.value` taken from user input (via `useInputValidation`) is passed directly to the `searchUser` function, likely making its way to the backend unescaped.
- **Risk:** If `searchUser` makes requests to a backend (especially a database or search engine) that does not properly sanitize input, this could allow for:
  - **Injection attacks**, e.g. NoSQL injection or SQL injection (depending on the backend).
  - **Search endpoint abuse** (e.g. broken search queries, DoS).
- **Mitigation:** Ensure both client-side and server-side input validation and sanitization. Never trust client input.

### b. Logging Raw Errors
- **Code:** `.catch((e) => console.log(e));`
- **Risk:** This could result in sensitive error output to the browser console, potentially leaking back-end implementation or user information leading to **information disclosure**.
- **Mitigation:** Log only non-sensitive diagnostic data to the client. Prefer redacting message content or logging errors server-side only.

---

## 2. API Usage

### a. Passing Unvalidated IDs to sendFriendRequest
- **Code:**  
  ```js
  const addFriendHandler = async (id) => {
    await sendFriendRequest("Sending friend request...", { userId: id });
  };
  ```
- **Risk:** If `id` (from `users.map(i)` and `UserItem`) is not validated as a safe string or identifier, a malicious actor could manipulate user objects to inject arbitrary values as a `userId`.
- **Mitigation:** Ensure user IDs received from the server are strictly validated both client- and server-side before use.

---

## 3. Rate Limiting & Enumeration

### a. Potential User Enumeration
- **Implementation:** The code ties live search via a 1-second debounce to a backend search endpoint.
- **Risk:** As written, an attacker could script requests to the search endpoint to enumerate users.
- **Mitigation:** 
  - Implement rate-limiting and throttling on the server.
  - Avoid returning detailed or sensitive data in search results.
  - Obfuscate error messages/results for failed searches.

---

## 4. Cross-Site Scripting (XSS)

### a. Displaying User Data
- **Code**:  
  ```js
  {users.map((i) => (
    <UserItem user={i} ... />
  ))}
  ```
- **Risk:** If `UserItem` or any children render user-supplied fields directly (e.g., names, usernames) without output encoding, there’s a risk of reflected or stored XSS.
- **Mitigation:** Ensure **all** fields from users are output-encoded/escaped before rendering.

---

## 5. General Recommendations

- **Never trust client input; always validate and sanitize on the server**.
- **Strictly validate all IDs and ensure they conform to expected patterns (e.g., MongoDB ObjectIDs)**.
- **Implement server-side rate limiting and abuse protection** on all endpoints accessed via search.
- **Escape all output** used in React components to defend against XSS.
- **Remove or mask verbose error logs** from browser output in production.

---

## Summary Table

| Vulnerability                      | Risk Level | Description                                                        | Location             |
|-------------------------------------|------------|--------------------------------------------------------------------|----------------------|
| Input injection/search abuse        | High       | Unfiltered user input could enable injection/search abuse          | searchUser           |
| Information disclosure (logging)    | Medium     | Error logs in browser may leak implementation/user info            | console.log(e)       |
| Enumeration/brute-forcing           | Medium     | Search allows potential for user enumeration/brute force           | searchUser           |
| XSS in displaying user data         | High       | Rendering user data without encoding may lead to XSS               | UserItem             |
| Unvalidated userId passed to APIs   | Medium     | API may be exposed to malicious IDs if not strictly validated      | addFriendHandler     |

---

**Remediate these issues before deploying to production.**