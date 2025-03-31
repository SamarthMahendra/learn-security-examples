# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.
   - The primary vulnerability is NoSQL injection through unsanitized user input directly passed to MongoDB's query engine
   - The application doesn't validate or sanitize the `id` parameter from the request query
   - There's no error handling for database operations which can cause the server to crash when malformed queries are submitted
   - There's no rate limiting to prevent excessive requests from a single source

2. Briefly explain how a malicious attacker can exploit them.
   - An attacker can submit special MongoDB operators like `$ne` (not equal) in the query parameter
   - The query `id[$ne]=` translates to finding documents where _id is not equal to an empty string, potentially matching all documents
   - This could return a large number of results or cause the server to crash due to unexpected input format
   - Without rate limiting, attackers can repeatedly send malicious requests to overload the server

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?
   - Implements rate limiting using `express-rate-limit` to restrict each IP to 1 request per 5 seconds
   - Adds proper error handling with try/catch blocks to prevent server crashes from unexpected inputs
   - Returns appropriate error codes (500) for server errors instead of crashing
   - The error handling ensures the application remains available even when database operations fail