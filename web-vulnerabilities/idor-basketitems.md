# Insecure Direct Object Reference (IDOR) in BasketItems API

## Description

The application allows users to access basket item data by modifying the ID parameter in the request. The server does not verify whether the requested resource belongs to the authenticated user, resulting in unauthorized access to other users’ data.

## Proof of Concept

Original request:
GET /api/BasketItems/9

Modified request:
GET /api/BasketItems/1

Response:
{
"ProductId": 1,
"BasketId": 1,
"quantity": 2
}

## Impact

An attacker can access other users’ basket data by modifying the ID parameter. This may expose sensitive information and lead to further exploitation.

## Recommendation

The server should enforce proper access control by verifying that the requested resource belongs to the authenticated user before returning any data.
