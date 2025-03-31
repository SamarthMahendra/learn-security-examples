# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.
   - The insecure version has no logging or audit trail mechanisms for user actions
   - There's no way to track who performed what actions in the application
   - User interaction with the server is not recorded or timestamped
   - Without logging, users can deny (repudiate) their actions since there's no evidence
   - The system cannot verify or prove which users performed specific actions

2. Briefly explain why the vulnerability is addressed in __secure.ts__.
   - The secure version implements comprehensive logging for all requests and actions
   - Each request is logged with a timestamp, method, URL, and IP address
   - User-specific actions like sending messages are logged with user identification
   - Error events are captured and logged with detailed context information
   - The logs provide accountability by creating a tamper-evident record of user activities

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?
   - The secure version implements the **Observer Pattern** through middleware for logging
   - The middleware observes all incoming requests and automatically logs them before processing
   - It uses a decorator pattern to enhance the base functionality with logging capabilities
   - The logging middleware intercepts all requests and adds detailed logging without modifying core functionality
   - Additionally, it implements error handling middleware that logs all errors in a consistent format
   - The logs are written to a persistent stream (file) to maintain a permanent audit trail