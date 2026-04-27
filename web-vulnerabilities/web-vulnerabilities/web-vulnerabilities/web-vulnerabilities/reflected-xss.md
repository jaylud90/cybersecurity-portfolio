# Reflected Cross-Site Scripting (XSS) in HTML Context

## Description

The application reflects user input directly into the HTML response without proper encoding. This allows an attacker to inject malicious scripts that execute immediately in the victim’s browser.

## Proof of Concept

Payload:

```html id="x5a1fz"
<script>alert(1)</script>
```

Request:

```
GET /?search=<script>alert(1)</script>
```

Response includes:

```html id="l9z0ny"
<script>alert(1)</script>
```

## Impact

An attacker can execute arbitrary JavaScript in a victim’s browser, potentially leading to:

* Session hijacking
* Data theft
* Unauthorized actions performed on behalf of the user

## Root Cause

The application fails to encode user input before including it in the HTML response. As a result, the browser interprets the input as executable code.

## Recommendation

* Encode all user input before rendering in HTML
* Use secure output handling practices
* Avoid directly inserting user-controlled data into the page
