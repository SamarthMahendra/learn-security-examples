# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**
   - The application directly uses unsanitized user input in MongoDB queries without any validation
   - The `username` parameter from the query string is passed directly to the MongoDB query without type checking
   - There's no input validation or sanitization to prevent injection of NoSQL query operators
   - The server discloses full user details including passwords in responses, exposing sensitive information

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can use NoSQL injection by submitting a query like `username[$ne]=`
   - The `$ne` operator in MongoDB matches documents where the username is not equal to the empty string
   - This results in matching all users in the database, effectively bypassing authentication
   - The vulnerable endpoint returns the complete user document including sensitive data like passwords

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?
   - Implements strict type checking to ensure the username is actually a string
   - Sanitizes the input by removing non-alphanumeric characters using regex `username.replace(/[^\w\s]/gi, '')`
   - Adds proper error handling with try/catch blocks to prevent exposing detailed errors to users
   - Returns appropriate error responses and prevents leaking of internal server information
   - Provides custom error messages instead of exposing internal server details