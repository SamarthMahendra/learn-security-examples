# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The server relies solely on client-provided data for authentication and authorization 
   - Access control is based only on the user ID provided in the request body with no session validation
   - The application performs role verification based solely on user-controlled input data
   - The server doesn't track user sessions or authenticate requests properly

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can simply submit a POST request to `/update-role` with the ID of an admin user
   - By setting `userId` to 1 (admin's ID) in the request body, the attacker can bypass authentication
   - The server will think the admin is making the request and allow role changes
   - The attacker can elevate privileges of any user account, including their own, to gain admin access

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?
   - Implements proper session management using express-session for server-side session tracking
   - Uses secure cookies with HttpOnly and SameSite attributes to protect session information
   - Authenticates users based on their session data rather than request body parameters
   - Separates the authentication of the logged-in user (session.userId) from the user being modified
   - Implements a separation of concerns between the authenticated user and the target of modifications