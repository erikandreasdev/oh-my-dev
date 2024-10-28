#### 🔹 **What is OAuth 2.0?**
OAuth 2.0 is an **Open Authorization protocol** that allows secure access to user data without sharing passwords. Instead, it uses **tokens** to grant limited access permissions to third-party applications on behalf of users.

---

#### 🔹 **Key OAuth Roles**

1. **Resource Owner (User)**: The individual whose data or resources are being accessed.
2. **Resource Server (API)**: Holds and protects the user’s data. It verifies access tokens and provides requested data if authorized.
3. **Authorization Server**: Authenticates the user and issues access tokens upon authorization. It may be the same as the Resource Server.
4. **Client (Application)**: The third-party app that wants to access the user’s data, acting on the user’s behalf.

---

#### 🔹 **OAuth Client Types**

- **Confidential Client**:
  - Typically server-side applications where the client secret can be stored securely.
  - Example: Web applications with access restricted to administrators.

- **Public Client**:
  - Front-end or mobile applications that cannot securely store a client secret.
  - Example: JavaScript applications in a browser or mobile apps.

---

#### 🔹 **OAuth Implementation Schemes (Flows)**

| **Flow Type**       | **Description** | **Client Type** | **Use Case**                          |
|---------------------|-----------------|-----------------|---------------------------------------|
| **Application Flow**| Client uses its client secret to get an access token directly, no user consent needed. | Confidential only | Server-to-server requests (e.g., API automation). |
| **Implicit Flow**   | Client receives an access token directly after user authorization without needing a code exchange. | Public only      | Browser-based or mobile applications. |
| **Password Flow**   | User directly provides username/password to client, which then requests an access token. | Public/Confidential | Trusted applications where user credentials are shared. |
| **Access Code Flow**| User authorizes app, receives a code, and the client exchanges this code for an access token. | Public/Confidential | Web and mobile applications needing enhanced security. |

---

#### 🔹 **How OAuth Works - Step-by-Step Process**

1. **Application Registration**:
   - Developer registers their application on the provider’s platform (e.g., Google, Facebook).
   - Developer receives a **client_id** and **client_secret**.

2. **Authorization Request**:
   - Developer creates an authorization URL containing:
     - `client_id`
     - `redirect_uri`: The URL the user will be redirected to post-authorization.
     - `response_type`: Specifies the type of response (usually an authorization code).
     - `scopes` (optional): Defines the specific permissions the application is requesting.

3. **User Consent**:
   - User visits the authorization URL and is prompted to allow or deny access to their data.
   - If the user consents, they are redirected to the `redirect_uri` with a response.

4. **Token Exchange**:
   - The client uses the response (e.g., an authorization code) to request an **access token** from the authorization server.

5. **Access Token Usage**:
   - The access token is used in HTTP requests to the API (Resource Server) to access user data.
   - Tokens have an expiry period and may require a refresh after expiration.

---

#### 🔹 **Summary of OAuth Concepts**

- **OAuth Roles**: Resource Owner, Resource Server, Authorization Server, Client.
- **Client Types**: Confidential (server apps) and Public (mobile or front-end apps).
- **Flows**: Application, Implicit, Password, Access Code.

OAuth provides a **secure, token-based method** for applications to access user data without exposing passwords. By understanding the roles, flows, and setup steps, developers can integrate OAuth effectively to enhance user security and simplify access control in their applications.

#### Additional resources
https://www.youtube.com/watch?v=CPbvxxslDTU