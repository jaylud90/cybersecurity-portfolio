# Stored Cross-Site Scripting (XSS) in HTML Context

## Description

The application stores user-supplied input and later includes it in the HTML response without proper encoding. This allows an attacker to inject malicious scripts that execute when the stored content is viewed by users.

## Proof of Concept

Payload:

```html
<script>alert(1)</script>
```

Steps to reproduce:

1. Navigate to the blog or comment section of the application
2. Submit the payload in a comment field
3. Return to the page where the comment is displayed
4. Observe that the script executes automatically when the page loads

## Impact

An attacker can execute arbitrary JavaScript in the browser of any user who views the affected page. This could lead to:

* Session hijacking
* Theft of sensitive information
* Unauthorized actions performed on behalf of the user

## Root Cause

The application fails to properly encode user input before rendering it in the HTML response. As a result, the browser interprets the input as executable code instead of plain text.

## Recommendation

* Encode user input before rendering it in HTML (e.g., convert `<` to `&lt;`)
* Avoid inserting raw user input directly into the page
* Use secure templating frameworks that automatically escape output
