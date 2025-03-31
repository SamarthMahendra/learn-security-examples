# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The server directly injects user input into HTML responses without sanitization
   - User-provided data from `req.body.name` is stored in the session and displayed in the HTML without escaping
   - This creates a Cross-Site Scripting (XSS) vulnerability where malicious scripts can execute in other users' browsers
   - The application fails to validate or sanitize input, allowing HTML and JavaScript injection

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can submit HTML/JavaScript code in the name field of the form
   - The malicious code gets stored in the session and rendered in the response
   - For example, by submitting `<script>document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>`, the attacker can completely modify the page content
   - The injected script executes in the victim's browser with access to cookies, local storage, and DOM
   - This could lead to cookie theft, credential stealing, or redirecting users to malicious sites

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?
   - The secure version implements input sanitization through the `escapeHTML` function
   - All user input is passed through this function which escapes special HTML characters
   - The function converts potentially dangerous characters (`&`, `<`, `>`, `"`, `'`) to their HTML entity equivalents
   - For example, `<script>` becomes `&lt;script&gt;`, which browsers render as text, not as executable code
   - This prevents HTML and script injection while preserving the intended display of user content