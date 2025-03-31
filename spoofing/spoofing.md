# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.
   - The server sets session cookies with `httpOnly: false`, allowing client-side JavaScript to access them
   - No CSRF protection is implemented to prevent cross-site request forgery attacks
   - The session cookie doesn't have the `sameSite` flag set, allowing it to be sent in cross-origin requests
   - These vulnerabilities enable attackers to steal session credentials and impersonate legitimate users

2. Briefly explain different ways in which vulnerability can be exploited.
   - **Cookie Theft**: As demonstrated in `mal-steal-cookie.html`, JavaScript can access non-HttpOnly cookies and send them to a malicious server
   - **Cross-Site Request Forgery (CSRF)**: The `mal-csrf.html` demonstrates how an attacker can make the victim's browser submit unauthorized requests with their session credentials
   - An attacker could trick users into visiting malicious sites that steal their cookies or perform actions on their behalf
   - The stolen session can be used to access sensitive operations like `/sensitive` endpoint and gain privileged access

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.
   - The secure version sets cookies with `httpOnly: true`, preventing JavaScript from accessing cookies
   - Adds `sameSite: true` setting to ensure cookies are only sent with same-site requests
   - Uses a secret from the command line arguments rather than hardcoding it in the application
   - These protections prevent CSRF attacks and cookie theft by ensuring cookies can't be accessed by malicious scripts
   - The security settings ensure session credentials are only used in legitimate scenarios from the same origin