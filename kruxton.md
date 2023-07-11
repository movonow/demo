[Best POS Management System](https://www.sourcecodester.com/php/16127/best-pos-management-system-php.html)


Best POS Management System 1.0 login page contains a SQL injection vulnerability via `username` parameter in `/kruxton/admin_class.php`. An attacker can login system as an administrator without valid username or password, the attacker can then READ/WRITE/DELETE the system data.


This is the first report for the vulnerability. [CVE search result](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Best+pos+management+system)
<img width="1260" alt="image" src="https://github.com/movonow/demo/assets/39987032/c3525b35-3dbd-4393-84e7-7ede15530c1e">

---

## Vulnerability Type
CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')

## Mitigation
There must be user input validation before used in an SQL command.

## Impact
Allows an attacker to bypass user authentication and manipulate system information.

CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H

## Proof of Concept
The `username` is indeed validated in login page, but only on client side, which can be bypassed.

[Watch Exploit Best-Pos-Management-System Video](https://clipchamp.com/watch/Qld8q2DFT8s)

## Vendor
[SourceCodester](https://www.sourcecodester.com/)

## source of disclosure

In `admin_class.php:login` function, the `username` parameter is directly spliced into the SQL statement without sanitization:

```php
function login(){
	extract($_POST);		
	$qry = $this->db->query("SELECT * FROM users where username = '".$username."' and password = '".md5($password)."' ");
	if($qry->num_rows > 0){
		foreach ($qry->fetch_array() as $key => $value) {
		if($key != 'passwors' && !is_numeric($key))
			$_SESSION['login_'.$key] = $value;
		}
		return 1;
	}else{
		return 3;
	}
}
```

### Version 1.0
Software download link:
https://www.sourcecodester.com/sites/default/files/download/mayuri_k/kruxton.zip

