# The next challange it's called "Sequel Pro"
## Exploit : SQL Injection Authentication Bypass


## From the description —> "At Sequel Pro there is a vulnerable login page. You can login as ctf with password ctf. Can you find out the secret of admin?" we know it is a login page and our ultimate goal is to authenticate as the admin user

<img width="702" height="235" alt="image" src="https://github.com/user-attachments/assets/3b81676a-5e59-4d04-a999-54bc08da7c8b" />

### First, let's test the intended way by logging in with the provided credentials:

```
ctf:ctf
```

<img width="623" height="157" alt="image" src="https://github.com/user-attachments/assets/73c956f9-7132-428a-9d92-7afdd3b18cb4" />


### We successfully logged in, but the ctf account doesn't give us the flag. This confirms we need to access the admin account

### Since we don't know the admin's password, the first thing I tried was an SQL Injection on the username field to bypass the authentication mechanism

#### I used the following payload:

```sql
#User
' or 1=1 -- -
```
```sql
#Password (random characters, it doesn't matter)
dwadwadawdawd
```

### We successfully logged into the admin account, revealing the flag

<img width="607" height="175" alt="image" src="https://github.com/user-attachments/assets/81245177-0237-4c9f-8fce-517afaa44436" />

# How SQL it's working 

### This exploit works because the backend `PHP` code takes the user input and directly concatenates it into the `SQL` query without proper sanitization or parameterized queries

### The vulnerable backend code most likely looks like this:

```php
$username = $_POST['username'];
$password = $_POST['password'];

$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";
```
### When we input our payload `' or 1=1 -- -`, the final `SQL` query executed by the database becomes:

```sql
SELECT * FROM users WHERE username = '' or 1=1 -- -' AND password = 'dwadwadawdawd'
```
-------------

-> `'` -> Closes the intended string for the username field.

-> `OR 1=1` -> Injects a mathematical condition that is always true. Since 1 is always equal to 1 the entire `WHERE` clause evaluates to true

--> `-- -`  --> The `--` is the `SQL` comment symbol. It tells the database to ignore everything that follows. Because of this the password check is completely dropped from the query.

-------------


