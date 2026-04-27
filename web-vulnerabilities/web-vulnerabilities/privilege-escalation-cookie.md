# Privilege Escalation via Client-Side Role Control (Cookie Manipulation)

## Description

The application determines administrative privileges based on a client-controlled cookie. By modifying the `Admin` cookie value, an attacker can gain unauthorized access to restricted functionality.

## Impact Summary

This vulnerability allows an attacker to escalate privileges and gain administrative access by manipulating client-side data. This can lead to full compromise of application functionality, including user management and data manipulation.

## Proof of Concept

Original cookie:
Admin=false

Modified cookie:
Admin=true

Steps:

1. Intercept request to `/admin` using Burp Suite
2. Modify the cookie from `Admin=false` to `Admin=true`
3. Send the request to access the admin panel
4. Observe successful access to restricted functionality

## Reproduction Steps

1. Log in as a normal user
2. Intercept a request to `/admin`
3. Modify the `Admin` cookie value to `true`
4. Forward the request
5. Access the admin panel and perform administrative actions (e.g., delete user `carlos`)

## Impact

An attacker can gain full administrative access without proper authorization. This allows unauthorized actions such as deleting users or modifying sensitive data.

## Root Cause

The application relies on client-side data (cookies) to enforce authorization decisions instead of validating privileges on the server.

## Recommendation

* Do not trust client-controlled data for authorization decisions
* Enforce role-based access control on the server
* Use secure session management to store user roles
